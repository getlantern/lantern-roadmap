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

### Completed
