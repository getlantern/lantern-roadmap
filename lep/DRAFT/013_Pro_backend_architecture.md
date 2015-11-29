# LEP 013 - Lantern Pro Architecture

Date: November 25th, 2015

Status: Draft

Author: uaalto

## Abstract

A proposal for a Lantern Pro version consisting of a premium membership allowing access to a series of backend services.  Examples of these services comprise things like multiple proxies, proxies with lower load, and perhaps extra blocking resistance.  Both the proxy and the client are automatically configured, and all pieces are put in motion so the whole system is aware of any new pro user.

## Rationale

This LEP stems from the need to accomodate more users within reasonable costs, focusing in one of the different solutions to the problem: diverting users to customer-supported servers.  The archicture of the system must also allow for different growth hacks to be possible, such as invitations and referrals, with subsequent promotions and gifts.  The system needs to be built with the promise of providing support for these growth hacks, and facilitating a smooth user experience in the client applications.  Very important as well is the need for a reliable and consistent system, that is fault-tolerant and scalable at the same time.


## Proposal

### General overview

The Pro backend cloud involves several actors, such as services, databases and external APIs.  These are briefly described next, starting from the most prevalent (the pro-server), in order to set a context for the flow diagram below.

All backend operations related to Lantern Pro go through **pro-server**, via a series of endpoints.  There are two notable exceptions to this rule: configuration serving (performed by config-server) and the authentication to the proxy service (performed by http-proxy or its substitute).

The tasks handled by the pro-server then are:

* Process payments, query previous payments
* Create and update users
* Perform user management operations
* Manage proxies assigned to Pro users
* Link users and proxies
* Populate configurations for the config-server and the http-proxy for their operation
* Produce and handle invitations and referral codes

An equally important part of the system is the **config-server**, which handles the following tasks:

* Manages IPs space distribution of http-proxy servers for free users.
* Distributes configuration for free users.
* Distributes configuration for Pro users (which merges a free configuration as well, for extra reliability).

In a lower layer, providing the necessary support for these services, we can find the databases and the 3rd party APIs.

Currently, the only database in use is **RedisDB**.  We try not to introduce any other database in the system for logs and frozen data.  For that purpose, Redis will only store service and entry pointers (unique IDs) that will be forwarded to the corresponding 3rd party services to resolve the query.  The responsibilities of the database can be summarized (extremely) in these points:

* Manage tasks providing a fault-tolerant queue with *once and only once* semantics.  This is particularly useful for user creation and proxy assignation.
* Manage user to proxies and proxies to user relationships, consistently and atomically.  Lua scripts are used for this purpose.
* Serve as a shared memory for the distributed system as a whole, and act as a notification device between different actors (such as between the pro-server and http-proxies).
* Store the configurations that will be served by config-server.
* Store user data, configuration, authentication and connected devices.
* Provide the pro-server with low-level operations for the management of Pro-explusive proxies.

Note: The database is shared with config-server, so there is more to the database that is not listed here.

Some external APIs are also required to simplify implementations of some common parts of the system.  Currently the use of these 2 is projected:

* *Mandrill*: email notifications, with attachments if required.
* *Stripe*: payments processing and charge data storage.


### Flow diagram

The [following diagram](images/lantern_pro_architecture.png) shows how the different client actions are mapped to endpoints and backend operations.

The client actions are specific situations where the client needs to request a service from the backend.  These situations are explained by actions, so they depict the intention at the client when an endpoint is reached.

The second column comprises the endpoints provided by the *pro-server* and any complementary service, such as the *config-server*.  Request parameters and returned data of the messages is also described here.  In current design, these are HTTP endpoints, but different protocols can be considered for solving client acknowledgments to the server if the confirmation endpoint doesn't suffice.

The third column depicts the processes carried by the backend endpoints, where different subsystems and external APIs are orchestrated to satisfy client's requests.  Critical processes may require atomicity, and are marked as such.  One important thing to note is that this diagram shows only processes and not implementation details, which may imply some specific solutions to handle fault-tolerance and atomicity where required.

*Note: the diagram uses more than one RedisDB nodes for the sake of graphical simplification, but they are actually the same node.*

The [next diagram](images/user_creation.png) depicts in further detail the process for fault-tolerant user creation. 
