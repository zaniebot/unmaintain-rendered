---
number: 518
title: "libgit2 shouldn't put write protection on git files in the cache"
type: issue
state: closed
author: konstin
labels:
  - bug
  - wish
assignees: []
created_at: 2023-11-30T10:55:15Z
updated_at: 2024-03-18T20:55:23Z
url: https://github.com/astral-sh/uv/issues/518
synced_at: 2026-01-10T01:23:04Z
---

# libgit2 shouldn't put write protection on git files in the cache

---

_Issue opened by @konstin on 2023-11-30 10:55_

Currently, when you try to remove a cache directory with a git checkout inside of it, `rm -r` complains.

```console
$ rm -r cache-all-kinds/
rm: remove write-protected regular file 'cache-all-kinds/git-v0/db/b49ffcfeb6c2e9d8/objects/pack/pack-c5780a0de30c59b83bb4638f5e8cd70797848444.pack'? 
```

Since this is only a cache directory, libgit2 shouldn't apply write protection that prevents removing it.

---

_Comment by @charliermarsh on 2023-11-30 17:56_

But `-rf` works as expected I assume?

---

_Comment by @konstin on 2023-11-30 17:57_

Yes, this is specifically about trying to remove a directory in the normal way, e.g. by deleting it in your file manager.

---

_Label `wish` added by @charliermarsh on 2023-12-01 18:49_

---

_Label `bug` added by @charliermarsh on 2023-12-01 18:49_

---

_Comment by @charliermarsh on 2023-12-01 18:52_

I kind of think this is okay.

---

_Comment by @charliermarsh on 2024-03-18 20:55_

Our `uv cache clean` handles this correctly.

---

_Closed by @charliermarsh on 2024-03-18 20:55_

---
