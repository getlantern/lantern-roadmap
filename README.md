## Introduction

This document contains the current state of the Lantern roadmap. It will be dynamic as progress
is made and priorities shift as we learn more from users. 

### Immediate (within 4 weeks)
* Focus on all get mode features for Lantern 1.0.0 final release.

### Release 1.1, codename "Dandelion" (1 month - 3 months)

Release 1.1 focuses on growing the Lantern network with the help of diaspora
communities.  We see the diaspora as critical to building the foundation of a
Lantern network that will benefit new censored users by providing them with
live proxies and that will motivate new uncensored users by demonstrating the
vitality and value of Lantern.

Release 1.1 is codenamed "Dandelion" because of that plant's use of diaspores
for self-propagation.  We will refer to members of the diaspora as "diaspores".

#### Dandelion Assumptions

 * Members of the diaspora are already motivated to join the Lantern network and
   run Lantern for personal reasons, and don't need to be sold on this.
 * Members of the diaspora live in the uncensored world (i.e. we are not
   modeling for individuals who have emigrated from one censored region to
   another censored region).
 * Members of the diaspora have connections in the censored world whom they can
   bring onto the Lantern network.
 * Members of the diaspora have connections in the uncensored world whom they
   can bring onto the Lantern network.
 * Some members of the diaspora work for organizations that actively support
   censorship circumvention and are known to and trusted by the Lantern team.
 * Some members of the diaspora work for organizations that are not involved in
   censorship circumvention but that have strong networks across back home and
   might be amenable to helping build the Lantern network.
   
#### Network Growth Cycle

We expect the Lantern network to grow in a cyclic process somewhat like the
following:

 1. Lantern core team member invites diaspore
 2. Diaspore installs Lantern
 3. Diaspore invites friends and family back home to Lantern
 4. Diaspore invites other members of diaspora to run Lantern in Give mode 
 5. Friends and family install and run Lantern
 6. Diaspore invites friends in the uncensored world to Lantern
 7. Friends and family in uncensored world install Lantern
 8. Friends and family back home connect through friends in uncensored world
    and are given the opportunity to send a thank you to them
 9. Friends in the uncensored world continue running Lantern
 
Release 1.1 will be successful if it can drive this cycle trough step 9.

Note that the ordering here matters.  Inviting other uncensored diaspora to run
Lantern can happen pretty early, since they are assumed to be motivated to
support Lantern.  However, if friends in the uncensored world are invited too
early (before there's much activity on the network), they won't benefit from
the map visualization and won't get quite the warm fuzzy-feeling.

#### Key Dandelion Goals

In order to support the above network growth cycle, Dandelion should focus on
the following goals (both technical and process):

 * Make it as easy as possible for diaspora to install Lantern.
 * Make it as easy as possible to invite friends and family back home.
 * Make it as easy as possible for friends and family back home to get up and
   running with Lantern.
 * Give diaspora tools for following up on invitations.
 * Help diaspora support their friends and family back home in using Lantern.
 * Give diaspora tools to generate compelling invitations to friends in the
   uncensored world.
 * Encourage diaspora to invite other give mode users once they have some
   activity from folks back home.
 * Give friends and family back home a way to thank give mode users
 * Reach out to diaspora to get them to use Lantern.
 * Make the process of supporting Lantern as easy as possible for diaspora
   organizations that want to help.  

#### Focus Areas

##### Invitation (big?)

 * Track users' progress through the invitation and installation process
   * Invited
   * Installed
   * Configured
   * Started proxying through Lantern
   * Track any problems they may have
   * Track connections to 2nd degree friends
 * Display invitee's progress to inviters (privacy implications?)
 * Track and display aggrevate invitation process metrics
 * Send reminder emails to invitees who haven't accepted
 * Send reminder emails to inviters about invitees who haven't accepted
 * Send reminder emails to inviters about invitees who've installed Lantern but
   don't seem to be using it much
 * Support bulk invites in the UI
 * Support personalized invitations
 * Send email give mode users with active get mode user base to suggest that
   they invite more give mode users to help relieve the burden of serving their
   friends. 
 
##### Installation (medium?)

 * Installation on every platform should be fool-proof
 * Provide great documentation on the installation process
 * Remove requirement to be admin user to install and run Lantern?
 * Ditch custom installers if at all possible (e.g. by disseminating fallback
   proxy information through KScope or by hand)?
 * Support push of upgrade notifications so we can easily get people off broken
   version of Lantern when necessary

##### Support for Institutional Lantern Use (unknown)

 * Make sure Lantern runs well on a typical corporate LAN
 * Support some form of corporate identity?
 * Rebranded Lantern
 * Institutional sponsorship of fallback proxies
 * Probably need to talk to potential institutional users to see what they would
   need/want.

##### Marketing/Outreach

### After Release 1.1 (3 month - 9 month)
* Implement secure chat between trusted contacts
* Implement file sharing between trusted contacts
* Implement secure video calling between trusted contacts

### Long Term (9 months to 18 months - funding dependent)

### Ideas (catch all)
* Allow users to make the map visualization their screen saver

#### Evaluate new Web Framework for UI Backend
Good listing of available frameworks:

http://www.techempower.com/benchmarks/#section=data-r6&hw=i7&test=json

##### Vert.x + sockjs (myleshorton)
See https://github.com/getlantern/lantern/issues/589

Vert.x has some nice properties, like its use of Netty and minimal config
(http://vertx.io/). It also works with sockjs
(https://github.com/sockjs/sockjs-client).

The downside is that it's not easily embeddable/designed to support embedding at
this time. Might be a nice change once it is as the vert.x and sockjs
communities seem a bit more active than jetty/cometd.

Performance wise, vert.x is not as fast as pure Netty, but it has a nice API.

Also, vert.x using JavaScript appears to be [faster than nodejs]
(http://vertxproject.wordpress.com/2012/05/09/vert-x-vs-node-js-simple-http-benchmarks/) 

##### Play Framework (myleshorton)
See https://github.com/getlantern/lantern/issues/589

Built-in support for async networking, comet, websockets, scala templates, etc.

Very active.

http://www.playframework.com/

##### Make users' home page our version of the Google search page
Could be a great way to fund Lantern! Along the lines of what FireFox does with their start page.


#### Switch to Gradle Build System (ox)
Ox finds Maven to be annoying and unintuitive.  [Gradle](http://www.gradle.org/)
is much nicer to use.

#### Enable on-the-fly adjustment of log levels in Lantern client (ox)
In the field, it can be nice to get verbose logging information from time to time, but having this be the default impacts performance.  It would be nice to have a feature in the UI that allows the user to turn up/down the log level when we need them to in order to debug a field issue.

The Log4J API allows changing the log level programmatically:

LogManager.getRootLogger().setLevel(Level.DEBUG);

It also has a facility for reloading its configuration (only for XML-based configuration)

http://logging.apache.org/log4j/1.2/faq.html#a3.6

We would need to build some UI around doing this, probably in the preferences
area.  We could define a few high-level categories that are meaningful like:

- UI
- Peer Tracking
- Proxying

Under the covers, we could map these to 1 or more calls to setLevel() for all
the relevant packages/classes.

(from Skivvies)

Instead, how about we make it so Lantern Controller can trigger a prompt on a
user's instance like: "The Lantern developers are requesting access to the
following logs of your Lantern activity to help troubleshoot an issue. Would you
like to send this? [Text area with logs here.] No/Yes/Always Allow This".
Clicking Yes or Always could also set the log level to DEBUG. There could be a 
corresponding checkbox under settings labeled something like "Allow Lantern 
developers access to logs" which would default to off but which would get
checked if they clicked Always in the prompt; that way they can always undo
clicking Always. Checking the box / clicking Always could also give us
permission to remotely adjust their log level at will.

This would make it easier for us to just get access to what we need with a
minimum of effort for users. We could take care of any filtering and compressing
of their logs we want to do before sending, and then securely transmitting them,
rather than making users deal with (even having to find them on the filesystem
before doing) all that.

What do you think? /cc @cholmes

(from Ox)
@skivvies I think that long-term your proposal could be really awesome from a
servicability perspective. The one potentially challenging thing is that if and 
when we find ourselves needing detailed logging, it might sometimes be because
the user can't establish connectivity, in which case it might not be possible
for us to remotely change the log level.

### Completed
