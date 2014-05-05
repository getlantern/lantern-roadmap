## Introduction

This document contains the current state of the Lantern roadmap. See the [readme](readme.md) for more information
on how to use and update it.

### Immediate (within 4 weeks)

**[Fallback Splitting and reinviting users](items/fallback-splitting.md)** - We need to bring back in all our existing users, but in a new trust network, with strategies for dealing with censor infiltration of fallbacks. 
([@aranhoide](https://github.com/aranhoide))

**[Standalone WebRTC for NAT Traversal](items/webrtc.md)** - Use WebRTC's advanced NAT traversal, factored out to its 
own library. ([@atavism](https://github.com/atavism))

**[Server Name Indication tunelling](items/sni.md)** - Use SNI on TLS to proxy through online cdn systems. ([@oxtoacart](https://github.com/oxtoacart) and [@skivvies](https://github.com/skivvies))


### Short term (1 - 3 months)

**[Gui for Admin Stats](items/admin-gui.md)** - Extend the prototype of a stats GUI to a dashboard view to get a real time
and cumulative view of Lantern network growth.

**[Improve signin process](items/signin.md)** - There are a number of workflow improvements we could make to ease 
setting up Lantern, for both give and get mode users.

**[Enable user set up of Fallbacks](items/user-fallbacks.md)** - Many users have access to machines that can be on
24/7 and serve as fallbacks. But we need to make it easier for them to set up and have a fallback that is not controlled
by Brave New Software.

**[Improve whitelist experience](items/whitelist.md)** - There are a few low hanging whitelist improvements we could make,
like easing the process of adding to the whitelist (example browser plugin?) and including whitelist info in our s3 config.

**[Auto-update](items/autoupdate.md)** - Right now we make users download new versions of Lantern. We should follow the
lead of chrome and firefox and just automatically update for our users when possible.

**[Interoperable Transports](items/transports.md)** - Tor has a pluggable transports API, but the core transports can
work just as well with Lantern. We need to incorporate more and build a joint community around transports.

**[Better documentation infrastructure](items/documentation.md)** - We are mostly done with migrating documentation to 
Sphinx. Need to finish it so it's in master, live on the website and in transifex.

**[Improve invitation experience](items/invitation.md)** - Do some tracking and UX improvement on users inviting others. How 
do we get people to invite more? And to accept invitations? Look in to personalized messages, etc.

### Medium term (3 - 9 months)

**[Non-admin installation](items/non-admin-install.md)** - Right now you need to be an admin to install Lantern. There is
no reason for this for give mode users, so we shouldn't require it. Can defer the call for admin rights for all users.

**[Whitelist for Give mode](items/give-whitelist.md)** - Many users are excited to share their connection, but not for any
possible use. Should give users the option to only let their censored proxy buddies access certain sites.

**[Achievement-based rewards](items/user-rewards.md)** - Should provide better user feedback, especially for give mode
users, on when they reach milestones. Can start with uptime and bytes transferred.

**[Better onboard experience for Give mode](items/give-onboard.md)** - Users in give mode should experience actually
helping people in censored countries. Right now they get a very empty map. Should figure out how to get them more
activity faster.

**[Non-google logins](items/no-google.md)** - Currently Lantern requires a google account to even use it. We should
let people who don't have google accounts join. This will require some deep changes, but should lead to a more
decentralized architecture.

**[Jurisdiction Hopping functionality](items/jurisdiction-hopping.md)** - A feature that could help us greatly expand
our give mode user base would be to provide a self-ish functionality of being able to view sites that are restricted in some way in one country but not another (like television shows, sporting events, etc).

**[Installer improvements](items/installer.md)** - We can move more functionality from the installer to the app itself, so
the app is a binary that configures its environment when it runs. Also should just improve the overall installer story, so it works on all platforms)

**[Better user prompting](items/unobtrusive-prompting.md)** - We should provide a more intelligent but unobstrusive 
prompting for things going on in Lantern. Investigate changing icon in toolbar, and desktop notifications.

**[Tor integration](items/tor-integration.md)** - We've proven integrating Tor in Lantern is technically feasible. Should
figure out a good user experience for our users, and be sure we're contributing properly to the Tor ecosystem.

**[Map Improvements and alternatives](items/map.md)** - Our map could be improved, to let users zoom in, be more accurate, etc. We should also add some non-map visualizations to show things that aren't best through a map.

**[Collapse Give and Get Mode](items/collapse-give-get.md)** - The distinction between give and get is not great, and 
ideally everyone can serve as give in some cases and get in others.

**[Security Audit](items/security-audit.md)** - We'll start a security audit of core Lantern components.

### Long term ( 9 - 24 months)

**[Stripped down fallbacks](items/stripped-fallbacks.md)** - Our fallbacks are currently a full Lantern version, but don't use the full Lantern capabilities. We should investigate if a lighter version might help.

**[Censored users proxying eachother](items/censored-proxy.md)** - Different content is blocked in Iran and China, so we
should let censored users serve as proxies for one another.


### Ideas

**Encrypted Video Chat** - Let users do video chat with one another. Likely as a separate product, but using the Lantern
infrastructure.

**Secure text chat** - Let users chat with Lantern friends. May be worth investigating integration with others like
cryptocat. But Lantern has a key asset here, which is the trusted friend list.

**Secure file transfer** - Since we already have a p2p network it would be easy to implement secure file transfer.
Should evaluate this from a product standpoint, may make sense as a Lantern 'add-on'. Could also be a good open
source contribution.

**Use map visualization as screen saver** - could be cool.

**Switch to Gradle Build System** - Maven to be annoying and unintuitive.  [Gradle](http://www.gradle.org/)
is much nicer to use.

### Deferred

**[Anonymize Jabber IDs](items/xmpp.md)** - To achieve better anonymity for our users we will not rely 
directly on Google Talk, but instead run our own XMPP server with anonymized ID's. 


### Completed

**[Admin Statistics Infrastructure](items/stats-infra.md)** - While our network tracks a good bit of information we have
no good historical view in to it, or an user-friendly admin interface to review what's going on. We will implement
a statistics tracking capability and interface. ([@oxtoacart](https://github.com/oxtoacart))

