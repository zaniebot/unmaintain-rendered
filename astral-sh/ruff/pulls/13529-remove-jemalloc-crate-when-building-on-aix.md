```yaml
number: 13529
title: Remove jemalloc crate when building on AIX
type: pull_request
state: merged
author: mustartt
labels:
  - release
assignees: []
merged: true
base: main
head: disable-jemalloc-on-aix
created_at: 2024-09-26T17:04:29Z
updated_at: 2024-09-26T17:20:54Z
url: https://github.com/astral-sh/ruff/pull/13529
synced_at: 2026-01-12T15:55:44Z
```

# Remove jemalloc crate when building on AIX

---

_@mustartt_

## Summary
Building ruff on AIX breaks on `tiki-jemalloc-sys` due to OS header incompatibility

## Test Plan
`cargo test`



---

_Comment by @github-actions[bot] on 2024-09-26 17:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-09-26 17:20_

This seems fine to me. Thanks.

---

_Label `release` added by @charliermarsh on 2024-09-26 17:20_

---

_Merged by @charliermarsh on 2024-09-26 17:20_

---

_Closed by @charliermarsh on 2024-09-26 17:20_

---
