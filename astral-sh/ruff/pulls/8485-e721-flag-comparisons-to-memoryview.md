```yaml
number: 8485
title: "[`E721`] Flag comparisons to `memoryview`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - bug
assignees: []
merged: true
base: main
head: feat/E721memoryview
created_at: 2023-11-04T10:49:12Z
updated_at: 2023-11-04T14:28:16Z
url: https://github.com/astral-sh/ruff/pull/8485
synced_at: 2026-01-12T15:55:26Z
```

# [`E721`] Flag comparisons to `memoryview`

---

_@qdegraaf_

## Summary

Adds `memoryview` to the list of typeclasses that `fn is_type()` uses for type comparison checks so that it raises a violation if `is`, `is not` or `isinstance()` are not used. 

## Test Plan

Added examples to existing fixture

## Issue Link

Closes: https://github.com/astral-sh/ruff/issues/8483

---

_Comment by @github-actions[bot] on 2023-11-04 11:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-04 13:30_

---

_@charliermarsh approved on 2023-11-04 13:33_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-11-04 13:34_

---

_Merged by @charliermarsh on 2023-11-04 13:41_

---

_Closed by @charliermarsh on 2023-11-04 13:41_

---

_Branch deleted on 2023-11-04 14:28_

---
