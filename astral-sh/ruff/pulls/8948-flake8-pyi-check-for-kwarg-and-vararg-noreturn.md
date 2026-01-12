```yaml
number: 8948
title: "[`flake8-pyi`] Check for kwarg and vararg `NoReturn` type annotations"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: noreturn-false-negative
created_at: 2023-12-01T16:30:35Z
updated_at: 2023-12-01T17:20:34Z
url: https://github.com/astral-sh/ruff/pull/8948
synced_at: 2026-01-12T15:55:27Z
```

# [`flake8-pyi`] Check for kwarg and vararg `NoReturn` type annotations

---

_@tjkuson_

## Summary

Triggers `no-return-argument-annotation-in-stub` (`PYI050`) for vararg and kwarg `NoReturn` type annotations.

Related to #8771.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-12-01 16:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @tjkuson on 2023-12-01 16:43_

---

_@charliermarsh approved on 2023-12-01 17:18_

Thanks!

---

_Label `rule` added by @charliermarsh on 2023-12-01 17:18_

---

_Renamed from "Check for kwarg and vararg `NoReturn` type annotations" to "[`flake8-pyi`] Check for kwarg and vararg `NoReturn` type annotations" by @charliermarsh on 2023-12-01 17:18_

---

_Merged by @charliermarsh on 2023-12-01 17:18_

---

_Closed by @charliermarsh on 2023-12-01 17:18_

---

_Branch deleted on 2023-12-01 17:20_

---
