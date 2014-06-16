#LEP 005 - KScope Server

Date:   June 16, 2014

Status: Draft

Author: Ox Cart

## Proposal

Run the [kaleidoscope](https://github.com/getlantern/kaleidoscope) algorithm
on a server instead of in order to support:

- Offline forwarding of kaleidoscope (kscope) ads
- Selective relaxation of kscope to increase visibility of peers
- Publishing of port-mapped peers to Lantern's DNS management infrastructure for
  use in a host-spoofed configuration

## Background

Lantern peers find out about available peer proxies via the kscope algorithm,
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
later comes online, because A doesn't resent the ad unless A went offline and
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

To support host spoofing, the server would also forward ads to a priviliged
peer representing Lantern's DNS management system.

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

The AppEngine controller application is a good candidate because:

1. Clients are already authenticated to it
2. It knows the trust relationships based on the LanternFriend table
3. It is able to receive ads via XMPP
4. It is able to push ads via XMPP
5. It has access to the AppEngine datastore for storing routing tables, which
   should work well given that the routing tables are read much more frequently
   than they are updated
6. It has access to memcache for caching ads and is able to deliver them to
   clients in response to presence notifications
7. It is written in Java, so should be able to use the existing kscope
   implementation with minimal modification 

### Client

1. Send own kscope ads to controller XMPP bot
2. Stop forwarding kscope ads
3. Anything else?
