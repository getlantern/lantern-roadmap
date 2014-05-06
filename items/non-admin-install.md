### Non-admin installation 

Right now you need to be an admin to install Lantern. There is no reason for this for 
give mode users, so we shouldn't require it. Can defer the call for admin rights for all users.

#### Requirements
The user requirement is simple - a user installing lantern should not need to put in their admin password.

We should be able to install in a way that doesn't require admin access. In discussion this won't be possible
for get mode users, as we potentially modify their system proxy. But it'd be a better experience to defer that
step as well, only when we actually need it.

This should open up a nice base of give mode users - people who run lantern on their work computer. Many don't have
admin access to their own computers, but would be happy to run it and tend to have their computers on for much of 
the day.

#### Tickets

#### Owner


