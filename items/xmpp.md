### Anonymize Jabber IDs

Users who aren't Lantern Friends with one another and aren't even on one another's rosters still
exchange XMPP messages (such as kaleidoscope advertisements or direct connection negotiation 
messages) directly rather than through an anonymizing relay. The way this affects UX is users 
will see email addresses, names, and profile photos (and formerly IP addresses) not just of 
their direct friends but of users up to 4 degrees away from them (i.e. total strangers), 
compromising their privacy.

To achieve better anonymity for our users we will not rely 
directly on Google Talk, but instead run our own XMPP server with anonymized ID's.

#### Requirements
(ideally user driven, and if not in 'immediate' can include 'nice to haves' that aren't hugely required)

#### Tech research

I got Prosody and axrelay deployed and working, but hit a wall when I found that Google's XMPP servers are, incredibly, still refusing encrypted server-to-server connections. If we launched with it in this state, observers of traffic between our XMPP server and Google's could correlate relay ids with originals to defeat anonymization. I reached out to our contact at Google who initially said he would ask about it and that it should be an easy push, but that turned out not to be the case. He will be joining us on our standup this Wednesday, where we can catch up about it more.

https://github.com/getlantern/axrelay#test-axrelay-with-non-google-xmpp-accounts
https://github.com/getlantern/axrelay#test-axrelay-with-gmailcom-xmpp-accounts
https://github.com/getlantern/axrelay#test-axrelay-with-google-apps-domains-xmpp-accounts

#### Implementation approach
Set up xmpp.getlantern.org, anonymize from google talk through there.

#### Discussion
(links to any relevant discussion on lantern-devel. Don't discuss in the roadmap item, discuss on list and
report back here on the consensus path forward and link to the discussion)

#### Tickets
https://github.com/getlantern/lantern/issues/535

#### Owner
[@skivvies](https://github.com/skivvies)
