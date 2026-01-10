```yaml
number: 12406
title: Pass around index with associated metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - no-build
assignees: []
merged: true
base: main
head: charlie/index-i
created_at: 2025-03-24T01:55:55Z
updated_at: 2025-03-24T14:15:52Z
url: https://github.com/astral-sh/uv/pull/12406
synced_at: 2026-01-10T11:10:39Z
```

# Pass around index with associated metadata

---

_Pull request opened by @charliermarsh on 2025-03-24 01:55_

## Summary

This PR modifies the requirement source entities to store a (new) container struct that wraps `IndexUrl`. This will allow us to store user-defined metadata alongside `IndexUrl`, and propagate that metadata throughout resolution.

Specifically, I need to store the "kind" of the index (Simple API vs. `--find-links`), but I also ran into this problem when I tried to add support for overriding `Cache-Control` headers on a per-index basis: at present, we have no way to passing around metadata alongside an `IndexUrl`.

---

_Label `internal` added by @charliermarsh on 2025-03-24 01:56_

---

_Label `no-build` added by @charliermarsh on 2025-03-24 02:04_

---

_Marked ready for review by @charliermarsh on 2025-03-24 02:06_

---

_@zanieb approved on 2025-03-24 02:17_

---

_Merged by @charliermarsh on 2025-03-24 14:15_

---

_Closed by @charliermarsh on 2025-03-24 14:15_

---

_Branch deleted on 2025-03-24 14:15_

---
