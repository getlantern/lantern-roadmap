Modern applications like the Chrome browser as well as the main smartphone platforms have taught users to expect security updates and application upgrades to be applied automatically and transparently.  In the case of Lantern, being able to quickly push out updates to the whole user base is also important from a blocking resistance perspective, as we expect to continue to encounter situations in which an update is necessary to bypass the latest move by the censors.

## Requirements

Auto Update: The Lantern client should automatically detect when an update is available, download it, stage it and install it on the next restat of Lantern.

Manual Update: Once an update has been staged, it would be nice to let the user know that it's available and give them the option of immediately installing it.  If the user chooses to do so, Lantern should automatically restart into the state in which the user left it.

Incremental Update: The complete Lantern distribution is quite large.  To minimize bandwidth requirements, Lantern should ideally update only the pieces that need updating.  This can be elaborated to various levels of sophistication:

- Only update changed resources or components (i.e. a specific language file, the fteproxy executable, the Lantern jar, etc)
- Incremetally update binaries themselves (i.e. instead of downloading the whole Lantern jar, only download the changed class files, or instead of downloading the whole fteproxy executable, only download the delta of bytes)

## Implementation Approaches

### Git


### bsdiff/bspatch

http://www.daemonology.net/bsdiff/


