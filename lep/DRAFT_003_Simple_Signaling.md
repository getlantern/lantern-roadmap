#LEP 003 - Simple Signaling 

Date:   May 1, 2014

Status: Draft

Author: Team Lantern (Ox Cart Scribe)

## Proposal

Replace XMPP with a basic, custom HTTP-based signaling mechanism combined with
[Mozilla Persona](https://www.mozilla.org/en-US/persona/) in order to securely
ssupport:

- Running over host-spoofed proxies like CloudFlare
- Anonymization
- Store-and-forward delivery of kaleidoscope advertisements

## Background

Lantern currently uses XMPP (in particular Google Talk) for its signaling layer.
Lantern requires signaling for two purposes:

1. Deliver kaleidoscope advertisements to peers in order to let them know when a
   peer is available.

2. Allow two peers to exchange messages during an ICE session to punch holes
   through their respective firewalls.

There are three major problems with using Google Talk for Lantern's signaling
layer:

1. XMPP can't be tunneled over host-spoofed proxies like CloudFlare without some
   additional work.

2. Using Google Talk specifically requires Lantern users to have a Google
   account.

3. When using XMPP to perform ICE, both peers' email addresses are exposed,
   which is problematic when the peers in question aren't direct friends and
   shouldn't need to know each other's personal information.

4. Google's support for XMPP seems to be waning(1)

## Proposed Solution

Use an HTTP-based signaling server to which each peer maintains two long-lived
connections, one for continually getting messages intended for that peer and
another for sending messages to other peers via the signaling server.

### Inbound Flow

```
Client -> Server: GET
Server -> Client: Once a message is available, issue complete response to GET
                  (in order to make sure that CloudFlare flushes the response)
Client -> Server: GET
Server -> Client: Next message

And so on ...
```

### Outbound Flow

```
Client -> Server: POST
Server -> Client: Minimal response to POST
Client -> Server: POST
Server -> Client: Minimal response to POST

And so on...
```

## Authentication

1. Peer logs in with Mozilla Persona
2. Server issues some sort of anonymized id and private credential to client
   (e.g. X509 certificate).
3. Client subsequently authenticates to the server using the issued credentials.
4. Messaging between peers can use the anonymized id to prevent leaking the
   user's real email address.

--------------------------------------------------------------------------------

(1) http://windowspbx.blogspot.nl/2013/05/hangouts-wont-hangout-with-other.html