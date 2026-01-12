```yaml
number: 11268
title: Remove unnecessary check for RUF020 enabled
type: pull_request
state: merged
author: tusharsadhwani
labels:
  - internal
assignees: []
merged: true
base: main
head: typo-never-union
created_at: 2024-05-03T18:10:19Z
updated_at: 2024-05-03T18:23:39Z
url: https://github.com/astral-sh/ruff/pull/11268
synced_at: 2026-01-12T15:55:37Z
```

# Remove unnecessary check for RUF020 enabled

---

_@tusharsadhwani_

## Summary

In #9218 `Rule::NeverUnion` was partially removed from a `checker.any_enabled` call. This makes the change consistent.

## Test Plan

`cargo test`


---

_@zanieb approved on 2024-05-03 18:14_

Thanks!

---

_Label `internal` added by @zanieb on 2024-05-03 18:14_

---

_Merged by @zanieb on 2024-05-03 18:19_

---

_Closed by @zanieb on 2024-05-03 18:19_

---

_Branch deleted on 2024-05-03 18:22_

---

_Comment by @github-actions[bot] on 2024-05-03 18:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
