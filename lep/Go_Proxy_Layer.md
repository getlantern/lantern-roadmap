#LEP 002 - Go Based Proxying Layer

Date:   March 26, 2014

Status: Draft

Author: Ox Cart

## Proposal

Implement CloudFlare host spoofing support in Lantern (bug 1441) by 
incorporating flashlight into Lantern as a child process.  This will certainly
be higher effort in the short term than just incorporating the changes needed
to Lantern directly, however it will yield medium and long-term benefits by
reducing the effort for supporting host spoofing with services other than
CloudFlare, directly addressing some existing roadmap items, making progress
towards other roadmap items and making it generally easier to maintain, innovate
on and get contributions to Lantern's core networking features in the future.
The big caveat here is that a lot of the longer-term benefits are only realized
by switching completely from Java to Go, so moving in that direction now most
likely only makes sense if we're open, maybe even committed to, that happening
at some point.

## Background

Proxying HTTP traffic is central to Lantern's functioning.  Lantern relies on
a proxy local to the user's browser.  It also uses proxies on other hosts,
including Lantern peers, servers hosted by Lantern and ideally servers hosted by
other parties as well.  Team Lantern's ability to inovate in this area, from
tweaking details of the HTTP traffic, to customizing TLS, to mimicking off-the-
shelf servers, to using custom protocols in place of TCP, a lot of the work of
staying blocking-resistant happens at this layer.  This is also the most
performance critical part of the Lantern system, as it affects the user's
browsing performance and affects Team Lantern's ability to cost effectively
provide cloud-based proxies.

Lantern's proxying layer is currently written in Java and based on the Netty
asynchronous networking library.  These perform very well in terms of CPU
utilization but are somewhat difficult to work with because doing non-blocking
I/O requires the use of asynchronous calling semantics (i.e. callbacks).  The
difficulty is compounded because of Java's lock-based concurrency abstractions.

Furthermore, Java's support for working with TLS leaves something to be desired.
In particular, Java only supports SNI on clients as of Java 7 and only supports
server-side SNI as of Java 8.  Also, doing programmatic certificate management
is fairly difficult(1).  The net effect of this is that Java is not optimal for
experimentation and prototyping in this area.

For example, in a recent experiment, Team Lantern wanted to see whether it would
be possible to proxy traffic through CloudFlare to a Lantern-hosted cloud proxy
using host spoofing.  During the course of this experiment, it became necessary
to experiment with various aspects of the HTTP protocol, including custom
headers, URL rewriting, chunking/not chunking, SNI, man in the middling clients,
and so on.  The experiment was conducted using the Go programming language(2).
Several features of that language made it well suited to this experiment.

## Go Benefits

### Full-featured HTTP support

Go's standard library has full featured support for HTTP clients and servers(3).
The included httputil(4) package even provides out-of-the-box support for proxy
servers, as well as full access to the primitives underlying their HTTP
implementation, such as chunked streams.  The APIs are simple and well
documented.

### User-friendly TLS/crypto libraries

Go's crypto libraries, and TLS in particular, are user-friend and full featured
(5).  However, they have not undergone any significant cryptographic review such
as FIPS certification(6).  For what it's worth, Oracle Java and OpenJDK have not
received FIPS certification either, and NSS and OpenSSL only achieve FIPS
certification conditional on their operating in "FIPS Mode"(7). 

### Non-blocking I/O with synchronous calling semantics

All I/O in Go is non-blocking, yielding similar performance benefits to Netty.
However, the calling semantics are all synchronous, such that Go code reads like
plain imperative code in which the control flow is readily apparent(8).  This
makes it easy to implement changes in the control flow and also makes it easy
for someone who didn't write the original code to understand and collaborate.
Also, this means once the prototype is done, it's close to production-ready.

One of the key aspects of this programming model is that all I/O is stream-
based, making for a much easier programming model(9).  Adding features like
chunking, buffering and so on just mean wrapping one stream in another.
Piping data is simply a matter of using the function io.Copy().
Simple and easy.

### Rapid cycle time

Go programs compile extremely quickly(10).  Building an executable for the
CloudFlare prototype takes under 1 second on a MacBook Air.  Compiling and
packaging LittleProxy on that same machine takes over 12 seconds.

### Statically Linked Binaries with Easy Cross-Compilation

Testing networked software often involves deploying programs to various machines
throughout the network.  Go programs are statically linked binaries that include
all of their dependencies, so deploying is simply a matter of scp'ing a binary
and running it at the destination.  Go provides excellent support for cross-
compilation(11), so one can produce binaries for a variety of target platforms
from a single developer or build machine, allowing one for example to develop on
a Macintosh, run the client on that Mac and test with a server running on Linux.

## Decision Criteria

The host spoofing experiment was successful.  The resulting code is called
flashlight and weighs in at a modest 216 lines of code(12).  It is now time to
productize the host spoofing approach, and Team Lantern needs to decide whether
to do this by absorbing the changes into Lantern and LittleProxy, or whether to
just wrap flashlight in a command-line API that would allow it to be used
directly from Lantern, in which case it would essentially function as a
replacement for LittleProxy.  Several factors should be taken into account when
making this decision.

### What would it take to make Lantern work with CloudFlare

1. Update LittleProxy to support MITM with dynamic certificate generation based
   on the host:port supplied in the CONNECT requests.  LittleProxy already has
   some support for MITM.  Implementing this change will require some changes to
   the connection establishment flow, which involves asynchronous code and
   callbacks and is consequently non-trivial.

2. Update Lantern to support new types of proxies (e.g. CloudFlare)

3. Update Lantern to manipulate requests to CloudFlare proxies and read these
   correctly on the server-side, mimicking what flashlight already does.  This
   should all be straightforward changes using the existing request filtering
   mechanisms provided by Lantern.  There will also be some configuration
   plumbing on the server-side to support running a proxy behind CloudFlare,
   again nothing difficult to change.

4. Update Lantern to plug into LittleProxy's MITM mechanism and provide dynamic
   certificates based on the requested hosts.  This will require either using
   the Java keytool program, or programmatically generating certificates.  If
   using the keytool, whose invocation is too slow to do at HTTP request time,
   we will need to pre-generate certificates based on the whitelist, update
   those certificates as the whitelist changes, and plumb this into the proxy
   layer using asynchronous events.  Programmatically generating certificates
   would involve a fair bit of code, as described previously.  Both approaches
   are non-trivial.

5. Lantern already generates its own certificate when first run.  This
   certificate will need to be updated to allow its use in signing other
   certificates (see step 4).

6. Update Lantern to call out to the platform-specific utility for adding its
   own certificate to the system keychain (e.g. the "security" command on OS X).

7. Update the Salt scripts to support deployment of fallbacks that use host
   spoofing.

### What changes are unnecessary if using flashlight in production?

Steps 1, 3, 4, and 5 become unnecessary.  Step 7, while necessary in both cases,
is somewhat different in practice as it will involve deploying flashlight-based
fallbacks, which is a bigger change.

### What additional changes are required if using flashlight in production?

1. Flashlight needs to add the ability to operate against different types of
   proxies at the same time and to round-robin through a proxy list in order to
   distribute the load and fall back to one proxy if a prior proxy didn't
   respond.

2. Flashlight needs an api based on stdin/stdout to allow Lantern to inform it
   what the proxy list is.

3. Flashlight needs the ability to connect directly to TCP proxies without using
   the host spoofing trick.

4. Flashlight needs to record usage statistics and report these on stdout so
   that Lantern can keep its map up-to-date and report statistics to statshub.

5. The Lantern installer builder needs to be updated to include Flashlight in
   the production package.

6. The Lantern build script will need to create a symlink to the appropriate
   flashlight binary based on the current platform to facilitate use of
   flashlight with Lantern during development/test.

7. GetModeProxy and GiveModeProxy in Lantern need to be updated to execute
   Flashlight, keep its proxy list up-to-date and read statistics udpdates
   from it.

8. Flashlight needs to implement the auth token and Apache mimicry features of
   Lantern to aid blocking resistance.

8. Automated unit tests need to be written for flashlight

9. Flashlight needs the ability to connect to peer proxies using UDT.  This is
   currently impossible as there is no Go binding for the UDT protocol.  On the
   flipside, Team Lantern has already discussed the fact that UDT is likely not
   at all blocking resistant because it is highly fingerprintable and uncommonly
   used.  This implies that we will need to hack on UDT to keep it viable, which
   might itself be well done in Go as opposed to the original C++.

### Are there further changes on the horizon?

Team Lantern hopes to apply the host spoofing trick to a variety of platforms
other than CloudFlare.  The CloudFlare experiment showed that implementing host
spoofing is somewhat particular to the cloud platform in use, so it is very
likely that host spoofing with other platforms will require further
experimentation and customization.  This multiplies the cost benefits of using
Go.

### Does flashlight work or is it just a prototype?

Testing with the CoAdvisor HTTP standards compliance tool shows a 62% success
rate for flashlight running through CloudFlare, compared with a 65% success rate
for Lantern running directly against a fallback proxy.  That's extremely
encouraging considering the relative youth of flashlight and that it's being
tested on a more constrained platform that inserts its own headers and which
requires flashlight to man-in-the-middle traffic, none of which is the case with
the vanilla Lantern test.

### Does flashlight help with any upcoming roadmap items?

1. Enable user setup of fallbacks - flashlight is a simple binary with a few
   command-line options and no dependencies on other Lantern components like
   signaling, authentication or NAT traversal.  A fallback is really just a
   flashlight instance, which anyone could run by installing the Lantern
   distribution and then running the flashlight command with a few parameters.

2. Stripped-down fallbacks - see #1 above.

3. Installer improvements - long term, by moving entirely to the Go platform,
   it becomes possible to build relatively small, self-contained binaries that
   don't have large external dependencies like Java.  This speeds up download
   and first-time installation, and makes the installation sequence less
   complicated and error prone.  Replacing LittleProxy with flashlight is a step
   in that direction, but the benefit isn't fully realized until completely
   elminiating Java from the picture.

4. Tor integration - the Tor model for integrating other software is to bundle
   binaries for that software in the Tor browser bundle.  To Ox's knowledge,
   none of these binaries currently rely on Java.  Having a self-containe
   Lantern to include with the Tor browser bundle would likely help ease
   integration.

### What are other benefits of switching from LittleProxy to flashlight?

1. Maintainability - though this is somewhat subjective, one could argue that
   flashlight will be inherently more maintainable, and easier to contribute to,
   than LittleProxy/Lantern.  One objective measure of this is lines of
   code(14).

   LittleProxy in its entirety is 3447 lines of code, plus another
   453 lines just for its build file.  Lantern doesn't use all of LittleProxy,
   but even just the 3 classes that constitute the absolute core of LittleProxy
   are 1195 lines of code(15).  This doesn't count the code in Lantern that's
   directly related to proxying (e.g. filters, GetModeProxy, GiveModeProxy,
   etc.)

   Flashlight in its current form is 216 lines of code and the mitm library that
   it uses(16) is 168 lines of code, and flashlight has no build file.  This
   makes it anywhere from 67% to 94% fewer lines of code, depending on how you 
   choose to count.  That count will admittedly grow as we integrate it into
   Lantern, but LittleProxy+Lantern would also grow as we add host spoofing to
   them.

2. Agility - many of the benefits already listed translate to faster innovation
   and feature development in Lantern.

3. Lower idle memory consumption - Go's idle memory consumption is lower than
   Java's.  This is particularly nice for Give mode users for whom Lantern
   should have as little impact as possible, especially when it's not busy
   serving a workload.

4. Potentially lower active memory consumption(17) - This is a boon both to end-
   users as well as to Lantern's proxy hosting operations, which are memory-
   constrainted at the moment.

### Will Team Lantern be able to maintain and hack on Go code?

Ox is currently the only team member who has been doing a lot of Go programming,
although Go is still relatively new to him too.  Aranhoide and Reginald have
gotten to review the statshub codebase, which is written in Go.

One of the benefits that organizations using Go sometimes site is that the
language itself is easy to learn quickly(18).

The best way to understand this aspect of flashlight is to have everyone on the
team take a look at both LittleProxy and flashlight and see how easy it is to
figure out what's going on and how they would make a basic change like
transmitting and receiving an additional custom header between the client and
server proxies.  We should bear in mind that everyone on the team has at least
some Java experience and has had a long time to get to know the current product,
and that flashlight doesn't even have automated unit tests yet.

## Footnotes

(1)  - https://www.mayrhofer.eu.org/create-x509-certs-in-java provides an
       overview of programmatic certificate management with Java

(2)  - http://golang.org/

(3)  - http://godoc.org/net/http

(4)  - http://godoc.org/net/http/httputil

(5)  - http://golang.org/pkg/crypto/tls/

(6)  - https://groups.google.com/forum/#!searchin/golang-nuts/crypto$20production/golang-nuts/LjhVww0TQi4/l200jnvnuA0J

(7)  - http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140val-all.htm

(8)  - This discussion thread is enlightening:
       https://groups.google.com/forum/#!topic/golang-nuts/AQ8JOHxm9jA

(9)  - See http://golang.org/pkg/io/#Reader and http://golang.org/pkg/io/#Writer
       for the core I/O primitives.

(10) - Compilation speed is a primary design objective of Go:
       http://golang.org/doc/faq#What_is_the_purpose_of_the_project

(11) - http://dave.cheney.net/2012/09/08/an-introduction-to-cross-compilation-with-go

(12) - https://github.com/oxtoacart/flashlight

(13) - Test results here: http://coad.measurement-factory.com/.
       Login information is available here:
       https://github.com/getlantern/too-many-secrets/blob/master/coad.measurement-factory.com_login.txt

(14) - As measured by CLOC - http://cloc.sourceforge.net/

(15) - ProxyConnection.java, ClientToProxyConnection.java and
       ProxyToServerConnection.java

(16) - https://github.com/oxtoacart/go-mitm

(17) - Anecdotal from Ox's observations playing around with it.  Need citation
       or own benchmark.

(18) - e.g. http://word.bitly.com/post/29550171827/go-go-gadget       