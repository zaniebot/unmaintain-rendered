```yaml
number: 1452
title: Option to Ignore Filesystem Types
type: issue
state: open
author: lousyd
labels: []
assignees: []
created_at: 2020-01-02T18:55:00Z
updated_at: 2020-07-21T13:13:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1452
synced_at: 2026-01-12T16:13:23Z
```

# Option to Ignore Filesystem Types

---

_@lousyd_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

yum install ripgrep

#### What operating system are you using ripgrep on?

RHEL 7.7

#### Describe your question, feature request, or bug.

I would like to use ripgrep in a way that I can tell it to ignore files on certain filesystems. ripgrep can ignore certain filenames and paths, and it can be told not to cross filesystem boundaries with "--one-file-system". Taking this one step further, I'd like to be able to say, for example, to ignore nfs filesystems. A similar feature can be found in updatedb (mlocate 0.26), which has a "--add-prunefs" option used to "Add entries in white-space-separated list FS to PRUNEFS". The daily cron job for updatedb, on my system, does this:
```
nodevs=$(awk '$1 == "nodev" && $2 != "rootfs" && $2 != "zfs" { print $2 }' < /proc/filesystems)
/usr/bin/updatedb -f "$nodevs"
```
This basically looks in the kernel's table of known filesystems and pulls out the ones marked "nodev", then tells updatedb to ignore files located on a filesystem that is one of those types. It would be awesome to have an option for ripgrep to tell it to do something like that. I very rarely want to search files under /proc or /run or so on. This would help with that. And this option would be especially useful to me to be able to avoid searching NFS mounts. Obviously sometimes you want to be able to search NFS mounts, but I have servers with NFS mounts that are terabytes in size and it's just user data, so I want ripgrep to ignore those and just search local filesystems.

I would like to see this implemented as an environment variable and a command line option.


---

_Comment by @genivia-inc on 2020-01-13 03:52_

Interesting idea. I would suggest options to specify file systems to include and exclude in/from recursive searches. This covers the case of file system types too, since we can get a list of file systems given a file type from `/etc/mtab`, like so:
~~~
nodevs=`grep -w -e rootfs -e zfs /etc/mtab | sed 's/[^ ]* \([^ ]*\).*/\1/' | paste -d, - -`
~~~
Then we simply pass this to `--exclude-fs` to exclude the specified file systems:
~~~
grep --exclude-fs="$nodevs" PATTERN PATH
~~~
where `--exclude-fs` excludes the specified file systems and devices, similar to `--exclude-dir` but applicable to all contents of the FS, including symlinks into the FS.

This gave me an idea to [build something like this](https://github.com/Genivia/ugrep#fs), which I tested on Linux, Mac OS X (FreeBSD), and Cygwin. It's fairly straightforward to implement. I wasn't sure what to use to separate the paths and opted for commas instead of colons, since Cygwin drives e.g. `d:/` should be valid to pass to `--exclude-fs`. IMO commas seem OK, as it is not likely that mounts include them in their name.

---

_Comment by @genivia-inc on 2020-01-13 19:11_

P.S. I meant to use `paste -d, - -` to replace newlines, instead of `tr`, which is now corrected with an edit.

---

_Comment by @lousyd on 2020-07-21 13:13_

I just noticed that "df" from GNU coreutils 8.22 and later has a "--local" option. The man page says "-l, --local    limit listing to local file systems". This is similar to what's requested here.

---
