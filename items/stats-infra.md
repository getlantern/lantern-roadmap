### Statistics Infrastructure (statshub)

##### Overview

We currently have statistics on total gigabytes transferred, number of users online and ever, invitations sent
and more. But we don't have a good way to see how that built up and what happened in the past. There are also a
number of questions we want to ask that take a bunch of custom GQL queries to figure out. We should have nice
GUI interfaces for our big questions about use, and also the ability to dig in to what's happened in the past.

##### Features

* Store snapshots / historical information
* Fast queries of lots of information
* Graphs of users online, ever, and bytes transferred

**Other questions to be able to answer with infrastructure**

* How many bytes has a user given vs. gotten?
* How many people did a user invite? How many invites do they have left?

##### Tech

The best way forward seems to be to use a database infrastructure that is more optimized for this type of analysis.
We will use Redis, Heroku and Google's big query....

##### Implementation



#### Tickets

https://github.com/getlantern/lantern/issues/1427
