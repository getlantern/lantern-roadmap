### Enable on-the-fly adjustment of log levels

In the field, it can be nice to get verbose logging information from time to time, but having 
this be the default impacts performance.  It would be nice to have a feature in the UI 
that allows the user to turn up/down the log level when we need them to in order to debug a field issue.

#### Tech research

The Log4J API allows changing the log level programmatically:

LogManager.getRootLogger().setLevel(Level.DEBUG);

It also has a facility for reloading its configuration (only for XML-based configuration)

http://logging.apache.org/log4j/1.2/faq.html#a3.6

We would need to build some UI around doing this, probably in the preferences
area.  We could define a few high-level categories that are meaningful like:

- UI
- Peer Tracking
- Proxying

Under the covers, we could map these to 1 or more calls to setLevel() for all
the relevant packages/classes.

(from Skivvies)

Instead, how about we make it so Lantern Controller can trigger a prompt on a
user's instance like: "The Lantern developers are requesting access to the
following logs of your Lantern activity to help troubleshoot an issue. Would you
like to send this? [Text area with logs here.] No/Yes/Always Allow This".
Clicking Yes or Always could also set the log level to DEBUG. There could be a 
corresponding checkbox under settings labeled something like "Allow Lantern 
developers access to logs" which would default to off but which would get
checked if they clicked Always in the prompt; that way they can always undo
clicking Always. Checking the box / clicking Always could also give us
permission to remotely adjust their log level at will.

This would make it easier for us to just get access to what we need with a
minimum of effort for users. We could take care of any filtering and compressing
of their logs we want to do before sending, and then securely transmitting them,
rather than making users deal with (even having to find them on the filesystem
before doing) all that.

(from Ox)
@skivvies I think that long-term your proposal could be really awesome from a
servicability perspective. The one potentially challenging thing is that if and 
when we find ourselves needing detailed logging, it might sometimes be because
the user can't establish connectivity, in which case it might not be possible
for us to remotely change the log level.
