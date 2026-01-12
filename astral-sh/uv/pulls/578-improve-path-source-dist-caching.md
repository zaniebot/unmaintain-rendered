```yaml
number: 578
title: " Improve path source dist caching "
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konstin/cached-by-timestamp
created_at: 2023-12-06T11:02:52Z
updated_at: 2023-12-06T16:47:02Z
url: https://github.com/astral-sh/uv/pull/578
synced_at: 2026-01-12T16:04:02Z
```

#  Improve path source dist caching 

---

_@konstin_

Path distribution cache reading errors are no longer fatal.

We now invalidate the path file source dists if its modification timestamp changed, and invalidate path dir source dists if `pyproject.toml` or alternatively `setup.py` changed, which seems good choices since changing pyproject.toml should trigger a rebuild and the user can `touch` the file as part of their workflow.

`CachedByTimestamp` is now a shared util. It doesn't have methods as i don't think it's worth it yet for two users.

Closes #478

TODO(konstin): Write a test. This is probably twice as much work as that fix itself, so i made that PR without one for now.

---

_@charliermarsh approved on 2023-12-06 16:46_

LGTM!

---

_Merged by @charliermarsh on 2023-12-06 16:47_

---

_Closed by @charliermarsh on 2023-12-06 16:47_

---

_Branch deleted on 2023-12-06 16:47_

---
