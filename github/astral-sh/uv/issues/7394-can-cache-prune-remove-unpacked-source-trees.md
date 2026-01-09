---
number: 7394
title: Can cache prune remove unpacked source trees?
type: issue
state: closed
author: bluss
labels:
  - cache
assignees: []
created_at: 2024-09-14T14:41:54Z
updated_at: 2024-09-16T23:18:21Z
url: https://github.com/astral-sh/uv/issues/7394
synced_at: 2026-01-07T13:12:17-06:00
---

# Can cache prune remove unpacked source trees?

---

_Issue opened by @bluss on 2024-09-14 14:41_

Building wheels from source leaves unpacked source trees after the build, such as this for cython:

`~/.cache/uv/built-wheels-v3/pypi/cython/3.0.11/4c57QS8T3uI0oPlqtRE1W/cython-3.0.11.tar.gz`

In this case it's 127 MB after build, 85 MB of which are in the ./build/ subtree with build artifacts.
If these files are not necessary after the .whl is produced and cached, can the build tree be removed? For example by uv cache prune.

---

_Comment by @charliermarsh on 2024-09-14 14:45_

Hmm... We do need them in case we have to re-build the wheel. For example, if you then try to install `cython` on a different Python version, we would need to rebuild (since the `cython` wheels are Python-minor-version specific, like 
`Cython-3.0.11-cp39-cp39-win32.whl`).

---

_Label `cache` added by @charliermarsh on 2024-09-14 14:45_

---

_Comment by @charliermarsh on 2024-09-14 14:45_

Perhaps we could add a setting for this, though...? Or make it part of `uv cache prune --ci`?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-16 21:29_

---

_Comment by @charliermarsh on 2024-09-16 21:31_

I think we should remove these in `uv cache prune --ci`.

---

_Comment by @bluss on 2024-09-16 22:04_

Ok I understand now, they are downloaded and unpacked where they are, and the build happens in the tree. It sounds good to remove them at least in some mode of pruning. I realize the process for building wheels don't leave a lot of control for how to handle details of the build (cleaning the tree after build -- removing build intermediates -- and before reusing the unpacked source for another build?)

---

_Referenced in [astral-sh/uv#7446](../../astral-sh/uv/pulls/7446.md) on 2024-09-16 23:01_

---

_Closed by @charliermarsh on 2024-09-16 23:18_

---
