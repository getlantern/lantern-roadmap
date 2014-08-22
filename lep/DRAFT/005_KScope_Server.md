# LEP 005 - KScope Server

Date:   August 22, 2014

Status: Draft

Author: Ox Cart

## Proposal

Run the [kaleidoscope](https://github.com/getlantern/kaleidoscope) algorithm
on a server instead of individual clients in order to support:

- Offline forwarding of kaleidoscope (kscope) ads
- Selective relaxation of kscope to increase visibility of peers

## Background

Lantern peers find out about available peer proxies via the [Kaleidoscope] algorithm,
which involves ads being forwarded along a chain of trusted peers along
randomized but stable routes.

For example, imagine the following friend relationships:

A  -> B1
   -> B2
B1 -> C1
   -> C2
B2 -> C3
   -> C4
C1 -> D1
   -> D2
C2 -> D3
   -> D4
C3 -> D5
   -> D6
C4 -> D7
   -> D8

When a peer proxy running under the identity of A becomes available it might
get advertised along the following route:

A  -> B1
   -> B2
B1 -> C2
B2 -> C3
C2 -> D3
C3 -> D6

At the moment, the forwarding is handled by Lantern peers themselves, and ads
are sent only once.  Thus, if B1 is not online at the time that A sends it an
ad, neither B1 nor C2 nor D3 will find out about A being available.  Even if B1
later comes online, because A doesn't resend the ad unless A went offline and
back online, A will continue to remain unknown to a substantial portion of the
trust network.

To solve this, we propose to handle the forwarding on a server that is itself
aware of the trust relationships and routing tables.  In our example, this would
work like this:

A  -> S -> B1
        -> B2
        -> C2
        -> C3
        -> D3
        -> D6

In addition to immediately forwarding ads to online peers, the server would also
allow peers to query for ads relevant to them, which allows them to obtain old
ads whenever they come online.  For example, if C3 queried for its ads, it would
receive a reply including peer A.  Note - these ads aren't expected to remain
fresh indefinitely, so keeping them around for future delivery is more of a
caching function.

## Implementation

### Server

The kscope server needs to:

1. Be able to authenticate clients
2. Know the trust relationships between users
3. Be able to receive ads
4. Be able to push ads to clients
5. Be able to maintain a persistent version of the kscope routing tables
6. Be able to cache kscope ads for future delivery to clients
7. Implement the kscope algorithm

This server could be implemented in Go using this go-based [kscope library] as
a starting point.  Much like [waddell][], it could use [framed][] to provide a
basic TCP-based protocol for communicating with clients.

Authentication can be performed using PKI, as described in LEP 008.

### Client

1. Send own kscope ads to new server
2. Get kscope ads from new server
3. Authenticate with a client certificate



[Kaleidoscope]: http://kscope.news.cs.nyu.edu/pub/TR-2008-918.pdf

[kscope library]: https://github.com/getlantern/kscope "kscope library"

[waddell]: https://github.com/getlantern/waddell "waddell"

[framed]: https://github.com/getlantern/framed "framed"