---
number: 6909
title: Two-step cache clean and prune needed to remove package from cache
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2024-09-01T11:00:50Z
updated_at: 2024-09-02T11:50:02Z
url: https://github.com/astral-sh/uv/issues/6909
synced_at: 2026-01-10T01:24:07Z
---

# Two-step cache clean and prune needed to remove package from cache

---

_Issue opened by @bluss on 2024-09-01 11:00_

Removing a big package like torch or similar requires both cache clean and prune to remove it from cache. It's expected that the first clean would remove it?


```shell
uv cache clean triton
# Removed 3 files for triton (1.5KiB)
uv cache prune
# Pruning cache at: 
# Removed 316 files (553.1MiB)

uv cache clean nvidia-cudnn-cu11
# Removed 3 files for nvidia-cudnn-cu11 (1.2KiB)
uv cache prune
# Pruning cache at: 
# Removed 31 files (974.5MiB)
```

The packages were installed like in #6907

We can also try with another package like hatch

```shell
uvx hatch
uv cache clean hatch
# Removed 4 files for hatch (61.7KiB)
uv cache prune
# Pruning cache at: 
# Removed 1848 files (79.6MiB)
```

Using uv 0.4.1

---

_Comment by @charliermarsh on 2024-09-01 15:59_

No this shouldn't be required. It looks like a bug.

---

_Label `bug` added by @charliermarsh on 2024-09-01 15:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-01 17:07_

---

_Referenced in [astral-sh/uv#6915](../../astral-sh/uv/pulls/6915.md) on 2024-09-01 17:09_

---

_Closed by @charliermarsh on 2024-09-01 17:27_

---

_Comment by @weihenglim on 2024-09-02 02:14_

Not sure if this is the intended behavior, but it seems like `uv cache clean <package>` still leaves some dangling cache entries on Windows using the latest version:
```cmd
>uv cache clean -v
DEBUG uv 0.4.2
No cache found at: C:\Users\User\AppData\Local\uv\cache

>uvx hatch --version
Installed 38 packages in 185ms <--- wow, that's confusingly fast on a cold cache
Hatch, version 1.12.0

>uv cache clean -v hatch
DEBUG uv 0.4.2
DEBUG Removing dangling cache entry: \\?\C:\Users\User\AppData\Local\uv\cache\archive-v0\zwnmKQVgeSUWVlA2QvK0E
Removed 122 files for hatch (466.2KiB)

>uv cache prune -v
DEBUG uv 0.4.2
Pruning cache at: C:\Users\User\AppData\Local\uv\cache
DEBUG Removing dangling cache entry: \\?\C:\Users\User\AppData\Local\uv\cache\environments-v1\1416b019b2dc3cb0
DEBUG Removing dangling cache entry: \\?\C:\Users\User\AppData\Local\uv\cache\archive-v0\mroAG5Am_YNe1aEo898zB
Removed 1577 files (52.2MiB)

>uv cache clean -v
DEBUG uv 0.4.2
Clearing cache at: C:\Users\User\AppData\Local\uv\cache
Removed 1395 files (53.8MiB)
```

---

_Comment by @charliermarsh on 2024-09-02 11:25_

Should be fixed in the linked PR.

---

_Comment by @weihenglim on 2024-09-02 11:28_

The above was tested on `v0.4.2`, was the PR not included in that release?

---

_Comment by @charliermarsh on 2024-09-02 11:43_

Sorry, it was but what you’re seeing isn’t the same thing. We remove all ephemeral environments when you prune. That will also remove other dependencies besides hatch.

---

_Comment by @weihenglim on 2024-09-02 11:50_

Ah gotcha, so `uv cache clean hatch` will only remove `hatch` but not it's dependencies

---
