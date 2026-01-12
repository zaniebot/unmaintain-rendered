```yaml
number: 508
title: Migrate interpreter query to custom caching
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-cacache
created_at: 2023-11-28T16:36:28Z
updated_at: 2023-11-28T17:15:00Z
url: https://github.com/astral-sh/uv/pull/508
synced_at: 2026-01-12T16:03:59Z
```

# Migrate interpreter query to custom caching

---

_@konstin_

This removes the last usage of cacache by replacing it with a custom, flat json caching keyed by the digest of the executable path.

![image](https://github.com/astral-sh/puffin/assets/6826232/8f777c4c-1f1b-4656-ba7b-002175270556)

A step towards #478. I've made `CachedByTimestamp<T>` generic over `T` but intentionally not moved it to `puffin-cache` yet.

---

_Label `internal` added by @konstin on 2023-11-28 16:37_

---

_@charliermarsh approved on 2023-11-28 16:56_

Excellent.

---

_Merged by @charliermarsh on 2023-11-28 17:14_

---

_Closed by @charliermarsh on 2023-11-28 17:14_

---

_Branch deleted on 2023-11-28 17:15_

---
