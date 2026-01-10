---
number: 7351
title: uv config file discovery avoiding filesystem traversal (because resolution blows up with unexpected directory called uv.toml)
type: issue
state: open
author: pelson
labels:
  - bug
assignees: []
created_at: 2024-09-13T08:16:59Z
updated_at: 2024-09-14T03:36:28Z
url: https://github.com/astral-sh/uv/issues/7351
synced_at: 2026-01-10T01:24:14Z
---

# uv config file discovery avoiding filesystem traversal (because resolution blows up with unexpected directory called uv.toml)

---

_Issue opened by @pelson on 2024-09-13 08:16_

Firstly, the reproducer:

```
$ mkdir uv.toml
$ uv pip install --python ./venv/bin/python setuptools
error: failed to read from file `./uv.toml`
  Caused by: Is a directory (os error 21)
```

`uv` version `0.4.9`

(I don't actually propose that this behaviour changes!)

----

This sounds like a pathological case (and it probably is), but it hit me in real-life today... I was using `uv` to install into a location which mounted through NFS. On my system, my sys admins have a special mount point at `/nfs` which will mkdir when you stat in the `/nfs/` root, and hence uv triggered the creation of a directory at `/nfs/uv.toml` upon stat.

I'm reporting this here because I think it is worth highlighting. Honestly, I think probably `uv` is right to fail when there is a directory called `uv.toml`, just like it fails when `uv.toml` is a file with invalid TOML syntax. However, it is worth noting that such a filesystem does not trigger `git` to trip up when searching for a parent repo:

```
$ cd /nfs/the-key-mountpoint/foo/bar; git status
fatal: not a git repository (or any parent up to mount point /nfs/the-key-mountpoint)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).
```

And luckily, it even works when `cd /nfs` directly (I think because it finds a `.git` directory, but it then fails to find the `.git/HEAD` file (and there is no magic on the subdirectory level of my mount point).

Concretely, an option which could help here, and which would IMO be reasonable (and quicker), would be to refuse to traverse filesystems when walking the tree for possible config locations. In principle I think this is done by checking `stat::st_dev` / `MetadataExt.dev()` is unchanged when accessing the parent.


---

_Label `bug` added by @charliermarsh on 2024-09-13 13:04_

---

_Comment by @charliermarsh on 2024-09-14 03:36_

It seems reasonable to me to follow Git's lead here (stop at filesystem boundaries with an env var to override) \cc @zanieb 

---
