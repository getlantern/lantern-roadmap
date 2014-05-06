## Introduction

This repository is dedicated to maintaining an evolving roadmap for Lantern.

See [roadmap.md](roadmap.md) for the main product roadmap, based on features.

The [items/](items/) folder contains the actual details on the tasks for the roadmap. 
To read those it is best to just follow the links from the roadmap, so the items are ordered properly.

Lantern Enhancement Proposals (LEP's) are the technical complement to the roadmap, containing the in 
depth engineering approach and implications. They are found in the [lep/](lep/) folder, as well as linked
to from roadmap items.

### Adding to the roadmap

To create a new roadmap item add an entry to `roadmap.md` following the same structure as the 
other links, and create a new page under `items/`. You can use this [template](items/_template.md)
to copy and paste in to your new item to follow the general structure of other items.

But there are no hard rules for items and the headings, except to make them useful for project
management and developers. 

### Maintaining the roadmap

Every week or two the roadmap should be updated for at least immediate and short term categories. Moving
items from one time frame to another needs to meet some minimum requirements:

##### Immediate Term

Any item listed in [immediate term](https://github.com/getlantern/lantern-roadmap#immediate-within-4-weeks) 
should have an 'owner': a developer who is working on it, and who expects to finish and get on master within
4 weeks. This should be placed directly on the main roadmap document. No one should 
own more than one immediate term priority. They can also be working on some short term items, but
the immediate one is their top priority. All items in immediate term should have a fully completed
Lantern Enhancement Proposal, and its status should be APPROVED.

##### Short Term

Any item list in [short term](https://github.com/getlantern/lantern-roadmap/blob/master/README.md#short-term-1---3-months)
should be ready for implementation. It should have a complete
set of requirements, and also any relevant wireframes. Wireframes may evolve with testing, but if an 
item needs wireframes it should have them by the time it gets to short term. It should also have an
LEP - the technology plan of how it will be built. This should be set to APPROVED before moving to immediate
term. 

##### Medium Term

Medium term items should have a real commitment to them, be it funding secured or a volunteer developer
agreed to do all the work. They should have the requirements detailed out to sufficient degree to begin
wireframes or technology plan. Medium term items should also be in accomplishable chunks. If it is a huge
scope in long term than just the first couple milestones should get in to medium term. All medium term items
should have an LEP, though it can be in DRAFT state.

##### Long Term and Ideas

These two are more buckets to make sure everything is recorded. Long term should be at least somewhat fleshed
out. Ideas can just be place holders to remind to fill out the requirements. Long term should also ideally be
at least in a funding pitch - the idea to secure funding for it relatively soon.

##### Deferred 

Deferred items had some work done on them, but for some reason didn't finish.

##### Completed

Completed items should be fully released in a version of Lantern. If all the original goals were not accomplished than
another roadmap item can be created to lay out what to do next. When moving an item to Completed be sure to update the
LEP to be IMPLEMENTED, so the LEP list stays up to date.
