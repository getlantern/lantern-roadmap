### Auto-update

Modern applications like the Chrome browser as well as the main smartphone platforms have taught users to expect security updates and application upgrades to be applied automatically and transparently.  In the case of Lantern, being able to quickly push out updates to the whole user base is also important from a blocking resistance perspective, as we expect to continue to encounter situations in which an update is necessary to bypass the latest move by the censors.

#### Requirements

Auto Update: The Lantern client should automatically detect when an update is available, download it, stage it and install it on the next restat of Lantern.

Manual Update: Once an update has been staged, it would be nice to let the user know that it's available and give them the option of immediately installing it.  If the user chooses to do so, Lantern should automatically restart into the state in which the user left it.

Incremental Update: The complete Lantern distribution is quite large.  To minimize bandwidth requirements, Lantern should ideally update only the pieces that need updating.  This can be elaborated to various levels of sophistication:

- Only update changed resources or components (i.e. a specific language file, the fteproxy executable, the Lantern jar, etc)
- Incremetally update binaries themselves (i.e. instead of downloading the whole Lantern jar, only download the changed class files, or instead of downloading the whole fteproxy executable, only download the delta of bytes)

#### Implementation Approaches

##### Git

Git has the ability to easily apply diffs of trees of resources.  We could store our exploded distributable (e.g. the /Applications/Lantern.App) in a Git repo.  To release it, we would simply give it a tag.  Clients could then look for tags that are newer than whatever tag they're on, then `git checkout <newtag>`.

The git binary itself is quite large, but [libgit2](http://libgit2.github.com/) is only about 420 KB compressed.  We could write a little C program that uses libgit2 to bootstrap the installation/updating of Lantern.

The main downside to Git is that, from what I can tell, it doesn't support binary patches, so it wouldn't be super efficient for updating our binaries.

##### bsdiff and bspatch

[bsdiff and bspatch](http://www.daemonology.net/bsdiff/) have the ability to produce binary patches that could in theory be much smaller than downloading the whole binary.  It looks like both executable are about 73 KB zipped, so that's not much download overhead.

We could combine bspatch with the Git approach by storing the original full binary in Git, and storing all updates to that binary as patches that the client applies using bspatch.  For new client installations, we could maintain separate branches that each have the full binary as of that release.  Each release ends up with its own branch, that gets updated with patches for that branch.

Simplified example:

```
2.0.0/ (branch)
  lantern.exe
  patches/
    2.0.1.patch
    2.0.2.patch
    2.1.0.patch
    2.1.1.patch
    2.1.2.patch
2.1.0/ (branch)
  lantern.exe
  patches/
    2.1.1.patch
    2.1.2.patch
```

To avoid growing these patch lists ad-infinitum, we could have clients checkout a whole new branch occasionally, thereby bringing them onto a new patch train.  As long as we're properly using Git to track file changes/renames, this checkout shouldn't be too expensive because it would only grab things that are actually new.








