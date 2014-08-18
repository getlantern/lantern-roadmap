#LEP 007 - Peer Verification

Date:   August 18, 2014

Status: Draft

Author: Team Lantern (Myles Horton Scribe)

## Proposal

Implement a more robust system for verifying give mode peers that attempt to 
register in the Lantern DNS proxy round robin.

## Background

As more and more users from uncensored regions have installed Lantern, we've 
experienced a quality of service challenge where some of those give mode peers simply are not
able to proxy traffic. The cause is unclear, but possibilities include:

1. There's a bug in Lantern (see, for example, https://github.com/getlantern/lantern/issues/1729)
1. Their machines simply aren't powerful enough to handle the load
1. They don't have enough bandwidth
1. They are in fact adversaries attempting to attack the network

## Proposed Solution

In all of these cases, the Lantern architecture needs some means of better verifying that
hosts that register are actually able to proxy traffic. The current system simply verifies
that they can accept incoming TCP connections on port 443, but even hosts that are able to
do that are not able to proxy traffic in all cases.

**We propose to simply verify that these new hosts are, in fact, able to proxy traffic 
end-to-end and to continuously verify that ability.** When we say "end-to-end" we mean 
proxying traffic through them as if we were get mode peers, i.e. running traffic 
through whatever service we're tunneling through. We propose to do this as soon as 
peers first register and then periodically thereafter.

In order to accomplish the above, we must first enter the give mode peer into DNS (in 
order to perform a complete end-to-end test). We proposed to enter them individually and the
to only add them to the round robin once we've verified they are, in fact, able to proxy
traffic.

One challenge of the above approach is that these are hosts running Flashlight and not 
simply ordinary HTTP proxies. As a result, the testing code itself needs to implement 
the full client side of flashlight.

