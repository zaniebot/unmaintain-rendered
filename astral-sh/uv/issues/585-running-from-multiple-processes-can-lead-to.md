```yaml
number: 585
title: Running from multiple processes can lead to failure due to stale wheel deletion
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-07T15:15:47Z
updated_at: 2024-01-27T20:21:47Z
url: https://github.com/astral-sh/uv/issues/585
synced_at: 2026-01-12T15:58:24Z
```

# Running from multiple processes can lead to failure due to stale wheel deletion

---

_@charliermarsh_

We'd like the installer and resolver to work when run from multiple concurrent processes (with a few exceptions, for example, if the user deletes the cache from one session while running a command from another, that seems okay).

In general the cache is purely additive, with one exception: we remove "stale" built distributions when we notice that they're no longer fresh. This could, in theory, lead to failures, if one process picks up a cached wheel, and then in-between picking it up and installing it, another process removes it from the cache.

I've never actually seen this, it's a theoretical concern.

---

_Label `bug` added by @charliermarsh on 2023-12-07 15:15_

---

_Comment by @zanieb on 2023-12-07 16:44_

What's the solution here? Add a file lock for use of wheels?

---

_Comment by @charliermarsh on 2023-12-07 16:51_

I think the safest thing would be to avoid removing the stale wheels, and always treat the cache as append-only.

---

_Comment by @konstin on 2023-12-08 09:40_

The case i can see, still very constructed:

1 A: See that the source dist cache for foo is stale
2 B: See that the source dist cache for foo is stale
3 A: Deletes the cache
4 A: Builds new wheels
5 A: Puts new wheels into place
6 B: Deletes the cache
7 A: Tries to install from the cache, the cache isn't there

If we want to prevent this we could put RW locks around the source dist cache dirs or try something around https://stackoverflow.com/a/10886940/3549270 so that the cache dir always exists

---

_Comment by @zanieb on 2024-01-27 20:17_

@charliermarsh resolved by your cache refresh work?

---

_Comment by @charliermarsh on 2024-01-27 20:21_

Yup thanks!

---

_Closed by @charliermarsh on 2024-01-27 20:21_

---
