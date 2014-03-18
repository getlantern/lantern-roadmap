### Fallback Splitting and Reinviting Users

Lantern 1.2 is much more blocking resistant, but we don't want to reinvite everyone all at once as
we know there were censors in the last group. So we need a strategy to get people on Lantern and
bring them in to the new trust network.

Key to this is how the fallback servers are used, with strategies to split or remove users on any
fallback that is compromised / blocked. From there we should aim to reinvite most everyone.

#### Requirements
* Be able to easily reshuffle users on a given fallback to more than one new fallback
* Re-invite all previous Lantern users
* Develop trust metrics for us to easily score how likely someone is to be a censor looking to block fallbacks.

#### Implementation approach

#### Tickets
https://github.com/getlantern/lantern/issues/1358

#### Owner
[@aranhoide](https://github.com/aranhoide)

