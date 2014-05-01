### Gui for Admin Stats 

Ox made a prototype of a stats GUI to a dashboard view at http://lantern-dashboard.s3-website-us-east-1.amazonaws.com/
We should turn that in to a real tool that can be used to really understand the Lantern network growth and use. With
both real time and cumulative views.


#### Requirements
The first two main views to have are:

* Number of users per day. This could be the max or average 'users online' number, or a tracking of number of unique
users per day who use Lantern. But this number should be perhaps our most important metric of success - how many people
use Lantern each day.

* Total Bandwidth. Ideally we could see this daily, hourly, every minute, and weeks and months. This tells us how much
people are actual using Lantern

Then a number of 'nice to haves', some of which may be a bit more challenging:

* Number of users invited (on different time scales - hour, day, week, month). Ideally be able to filter the graph by
an individual user, to see how many invites a single user sent out, and by country.

* Lantern downloads (per hour, day, week, month).

* Number of new users (how many people downloaded and successfully installed lantern), on different time scales.

* Friends per user, both total for the network, and for an individual user. Would be great to show over time, how many
friends a user invites. Should ideally show by country (or at least censored or not). And also how many total invites
the user has. 

* Number of users by total bandwidth - see like the number of people who have transferred more than 10mb, 100mb, 1gb, etc. 
And ideally show both peer bandwidth and fallback bandwidth, and give vs get.

* Number of users who have invited more than X users - explore how many users are inviting.

* Number of users who have no friends outside of a censored country. And ideally be able to easily show how many degrees 
away they need to reach an uncensored connection.

* Proxy health - be able to see how much bandwidth each proxy is using, what it's uptime is, how many users are assigned
to it, how many active users are there, etc.

Some of these might be able to be re-articulated as a single 'user view', where you can see a bunch of stats about a user.
Like their historical bandwidth and invitations - how much they've been using lantern and how much they've been inviting.
Ideally could click on any user and be shown the stats page for them, and click on their friends to explore the network.


#### Tech research

Ox did the prototype with highcharts. Other options could be explored.

#### Implementation approach

Most all the groundwork is done with the [Admin Statistics Infrastructure](stats-infra.md), which is collecting
most all the relevant data.

#### Wireframes
Niki? Could also do the first round without wireframes, just code the easiest thing, and then do a UI round after
the first version.


#### Discussion
(links to any relevant discussion on lantern-devel. Don't discuss in the roadmap item, discuss on list and
report back here on the consensus path forward and link to the discussion)

#### Tickets
(links to one or more github tickets where the state of this work is tracked

#### Owner
(github name of the person who will lead work on this)
