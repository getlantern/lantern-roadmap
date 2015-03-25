#LEP 011 - Performance Monitor Goals and Metrics

Date:   March 24, 2015

Status: Draft

Author: fffw

## Abstract

To continuously monitor the performance of Lantern network, we need to define dimensions and metrics clearly, and collect data to fulfill the goal at both given and get mode.

We must also keep user's anonymity.

## Proposal

The output, is a multi-dimensional model which can pivot from any dimension. The presentation solution is TBD, hopefully there are existing services or open source projects.
The fact to measure is data flow. For example, we can query:
* Average time required to establish connection from Iran to Facebook last weekend
* RTT from the given mode servers at each data center to Google
* Total throughput from Iran to Amsterdam data center per day
* Error rate comparison between beta version and previous release of Lantern in each country
* ...

###Terms
*  **Ingress**: the node where request enters Lantern network
*  **Egress**: the node where request exits Lantern network and hit the site

###Dimensions
* Ingress country/territory
* Egress country/territory
* Egress server pool, if that is the case
* Ingress Lantern version
* Egress Lantern version
* Site to visit
* Time of day (per half hour)
* Day of year/month/week

###Metrics
* Time required to establish connection (gauge, measured at both ingress and egress)
* Time elapsed between first write and the first byte received (gauge, measured at both ingress and egress)
* Request/Response throughput (counter)
* Errors encountered (counter)
* Connections made (counter)
* Parallel connections (gauge)
* Connection duration (gauge)

###Data to be collected
* Unique instance id
* total bytes sent per connection
* total bytes received per connection
* remote host
* remote address
* local address
* Duration of the connection
* Error encountered or not

All data will be timestamped and submitted at the very end of connection.

###Local calculation and submitting (per 5 min)

* mask the last octet of local/remote address

* accumulate counters in the period

* min/max/avg for all gauges


###Calculation at statshub

* Keep all raw data

* Add an layer to aggregate across dimensions, in half-hour grain

* Add customized stream layer for realtime monitor

###Other considerations

*  Authentication
	* Burn a key in Lantern to report statistics
	* Need another key to get data

*  Extensibility

   Should be easy to add more dimensions / metrics without modify existing logic.
