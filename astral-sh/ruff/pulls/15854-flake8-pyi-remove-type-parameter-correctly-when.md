```yaml
number: 15854
title: "[`flake8-pyi`] Remove type parameter correctly when it is the last (`PYI019`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: PYI019-2
created_at: 2025-01-31T16:16:23Z
updated_at: 2025-01-31T16:29:58Z
url: https://github.com/astral-sh/ruff/pull/15854
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-pyi`] Remove type parameter correctly when it is the last (`PYI019`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-31 16:16_

## Summary

Part of #15821.

Prior to this change, a PEP 695 type variable would not be removed correctly were it to be placed at the very end of the type parameter list:

```python
# Original
def f[T, S](self: S) -> S: ...

# Before
def f[TS](self) -> Self: ...

# After
def f[T](self) -> Self: ...
```

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-31 16:16_

---

_@AlexWaygood approved on 2025-01-31 16:21_

Thank you!

---

_Label `bug` added by @AlexWaygood on 2025-01-31 16:22_

---

_Label `fixes` added by @AlexWaygood on 2025-01-31 16:22_

---

_Label `preview` added by @AlexWaygood on 2025-01-31 16:22_

---

_Merged by @AlexWaygood on 2025-01-31 16:22_

---

_Closed by @AlexWaygood on 2025-01-31 16:22_

---

_Comment by @github-actions[bot] on 2025-01-31 16:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-01-31 16:29_

---
