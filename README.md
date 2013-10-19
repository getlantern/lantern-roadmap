## Introduction

This document contains the current state of the Lantern roadmap. It will be dynamic as progress
is made and priorities shift as we learn more from users. 

### Immediate (within 4 weeks)
* Focus on all get mode features for Lantern 1.0.0 final release.

### Short (1 month - 3 months)
* Focus on all give mode features for Lantern 1.1 release.
* Implement design changes to make give mode users feel more connected on the Lantern map visualization to make users more likely to invite other users.

### Medium (3 month - 9 month)
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
