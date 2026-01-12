```yaml
number: 431
title: " Use `sys.executable` as python root path"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: sys-executable
created_at: 2023-11-15T20:51:45Z
updated_at: 2023-11-16T11:16:50Z
url: https://github.com/astral-sh/uv/pull/431
synced_at: 2026-01-12T16:03:56Z
```

#  Use `sys.executable` as python root path

---

_@konstin_

Previously, we were assuming that `which <python>` return the path to the python executable. This is not true when using pyenv shims, which are bash scripts. Instead, we have to use `sys.executable`. Luckily, we're already querying the python interpreter and can do it in that pass.

We are also not allowed to cache the execution of the python interpreter through the shim because pyenv might change the target. As a heuristic, we check whether `sys.executable`, the real binary, is the same our canonicalized `which` result.

---

_Review requested from @zanieb by @konstin on 2023-11-15 20:51_

---

_@zanieb approved on 2023-11-15 22:39_

---

_Comment by @charliermarsh on 2023-11-15 23:17_

Do you mind adding a summary prior to merging?

---

_Comment by @konstin on 2023-11-16 11:16_

Wrote all that commit description and then forgot to paste it :bow: 

---

_Merged by @konstin on 2023-11-16 11:16_

---

_Closed by @konstin on 2023-11-16 11:16_

---

_Branch deleted on 2023-11-16 11:16_

---
