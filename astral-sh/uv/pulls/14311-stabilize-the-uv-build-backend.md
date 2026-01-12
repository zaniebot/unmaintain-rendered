```yaml
number: 14311
title: Stabilize the uv build backend
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/stabilize-the-uv-build-backend
created_at: 2025-06-27T10:42:41Z
updated_at: 2025-07-02T20:37:46Z
url: https://github.com/astral-sh/uv/pull/14311
synced_at: 2026-01-12T16:11:09Z
```

# Stabilize the uv build backend

---

_@konstin_

The uv build backend has gone through some feedback cycles, we expect no more major configuration changes, and we're ready to take the next step: The uv build backend in stable.

This PR stabilizes:

* Using `uv_build` as build backend
* The documentation of the uv build backend
* The direct build fast path, where uv doesn't use PEP 517 if you're using `uv_build` in a compatible version.
* `uv build --list`, which is limited to `uv_build`.

It does not:
* Make `uv_build` the default on `uv init`
* Make `--package` the default on `uv init`


---

_Label `enhancement` added by @konstin on 2025-06-27 10:42_

---

_@zanieb approved on 2025-06-27 17:36_

---

_@zanieb approved on 2025-07-02 13:21_

---

_Merged by @zanieb on 2025-07-02 20:37_

---

_Closed by @zanieb on 2025-07-02 20:37_

---

_Branch deleted on 2025-07-02 20:37_

---
