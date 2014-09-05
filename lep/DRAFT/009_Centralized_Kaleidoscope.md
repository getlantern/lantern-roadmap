# LEP 009 - Centralized Kaleidoscope

Date:   August 22, 2014

Status: Draft

Author: Ox Cart

## Background

Lantern uses the [kscope][] limited advertisement protocol to distribute
information about proxies in a blocking-resistant manner.  Kaleidoscope
distributes advertisements about proxy availability along a multi-degree trust-
network.  At each hop, the advertisements are forwarded to a limited subset of
other Lantern's trusted by the current one.

As currently implemented, this forwarding is done by the Lantern client
instances themselves, in real time.  This poses a significant problem for
deliverability because it relies on every node in the trust chain to be running
concurrently in order for a message to be delivered.

For example, let's say that we have a message that would be delivered from
Lantern A to Lantern D via the following route: A -> B -> C -> D.  For D to find
out about A, B and C have to be online while A and D are also online.  If either
is offline, then D never finds out about A, which goes unused.

## Proposal



[kscope]: (http://kscope.news.cs.nyu.edu/pub/TR-2008-918.pdf) "Kaleidoscope"