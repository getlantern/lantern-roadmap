### Enable user setup of Fallbacks

Many users have access to machines that can be on 24/7 and serve as fallbacks. But we need to make it easier 
for them to set up and have a fallback that is not controlled by Brave New Software. The low hanging fruit
is to let people login to lantern on the command line, so it can be set up on a server. This will not be a
real fallback, but will serve a similar purpose.

#### Requirements

Main short term requirement is to let users login to their google account through the command line. This item
may break out in to its own 'immediate' issue. Should be sure to test that Lantern functions properly if the same
user is signed in as 'give' in one place and 'get' in another.

The next step would be to allow Lantern client configuration of a fallback server, so someone could point their
lantern at a fallback server they or a friend set up. There are some open questions about how this fits in to the
trust network. Part of the appeal is to have a more decentralized architecture that does not rely on registering
in the Brave New Software network, but there are lots of implications of this. An alternative is to require login
for user provided fallbacks.

#### Tickets

https://github.com/getlantern/lantern/issues/1187
