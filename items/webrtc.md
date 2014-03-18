### Standalone WebRTC for NAT Traversal 

WebRTC NAT traversal is more reliable and robust than what we currently have for NAT Traversal. It's better because it 
uses three IEFT NAT traversal standards (ICE, STUN, and TURN) to establish connectivity between peers. 
We will create a new library called Natty to isolate these techniques and provide an independent implementation.

#### Requirements

* Build code into a standalone library, so we don't have to pull in all of WebRTC
* Put the code into an independent repository so that other projects can make use of it, to build open souce community.

#### Tech approach

We will build directly from the webrtc source code, but keep as an independent library that Lantern and
other apps can use for just NAT traversal.


#### Tickets

https://github.com/getlantern/lantern/issues/1386

#### Owner

[@atavism](https://github.com/atavism)
