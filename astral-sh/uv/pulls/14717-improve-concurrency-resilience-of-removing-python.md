```yaml
number: 14717
title: Improve concurrency resilience of removing Python versions from the Windows registry
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: konsti/more-resilient-registry-removal
created_at: 2025-07-18T12:25:00Z
updated_at: 2025-07-18T12:47:57Z
url: https://github.com/astral-sh/uv/pull/14717
synced_at: 2026-01-12T16:11:21Z
```

# Improve concurrency resilience of removing Python versions from the Windows registry

---

_@konstin_

With the previous order of operations, there could be warnings from race conditions between two process A and B removing and installing Python versions.

* A removes the files for CPython3.9.18
* B sees the key CPython3.9.18
* B sees that CPython3.9.18 has no files
* A removes the key for CPython3.9.18
* B try to removes the key for CPython3.9.18, gets and error that it's already gone, issues a warning

We make the more resilient in two ways:

* We remove the registry key first, avoiding dangling registry keys in the removal process
* We ignore not found errors in registry removal operations: If we try to remove something that's already gone, that's fine.

Fixes #14714 (hopefully)

---

_Review requested from @zanieb by @konstin on 2025-07-18 12:25_

---

_Label `bug` added by @konstin on 2025-07-18 12:25_

---

_Label `windows` added by @konstin on 2025-07-18 12:25_

---

_@zanieb approved on 2025-07-18 12:36_

---

_Renamed from "More resilient registry removal" to "Improve concurrency resilience of removing Python versions from the Windows registry" by @zanieb on 2025-07-18 12:42_

---

_Merged by @konstin on 2025-07-18 12:47_

---

_Closed by @konstin on 2025-07-18 12:47_

---

_Branch deleted on 2025-07-18 12:47_

---
