### Anonymize Jabber IDs

Users who don't trust one another and aren't even on one another's rosters still 
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
Looked in to prosody and ejabberid. Ended up with ejabberid (more thoughts here are welcome).

#### Implementation approach
Set up xmpp.getlantern.org, anonymize from google talk through there.

#### Discussion
(links to any relevant discussion on lantern-devel. Don't discuss in the roadmap item, discuss on list and
report back here on the consensus path forward and link to the discussion)

#### Tickets
https://github.com/getlantern/lantern/issues/535

#### Owner
[@skivvies](https://github.com/skivvies)
