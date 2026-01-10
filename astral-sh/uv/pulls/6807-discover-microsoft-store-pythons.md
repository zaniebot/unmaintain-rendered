```yaml
number: 6807
title: Discover Microsoft Store Pythons
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/read-python-from-store
created_at: 2024-08-29T12:32:13Z
updated_at: 2024-08-29T20:56:42Z
url: https://github.com/astral-sh/uv/pull/6807
synced_at: 2026-01-10T12:53:34Z
```

# Discover Microsoft Store Pythons

---

_Pull request opened by @konstin on 2024-08-29 12:32_

Microsoft Store Pythons do not always register themselves in the registry, so we port <https://github.com/python/cpython/blob/58ce131037ecb34d506a613f21993cde2056f628/PC/launcher2.c#L1744> and look them up on the filesystem in known locations.

## Test Plan

So far I've confirmed that we find a store Python when I use `cargo run python list`, can we make this a part of any of the platform tests maybe?



---

_Label `enhancement` added by @konstin on 2024-08-29 12:32_

---

_Label `windows` added by @konstin on 2024-08-29 12:32_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-29 16:32_

---

_@charliermarsh approved on 2024-08-29 20:01_

---

_Merged by @konstin on 2024-08-29 20:56_

---

_Closed by @konstin on 2024-08-29 20:56_

---

_Branch deleted on 2024-08-29 20:56_

---
