```yaml
number: 12958
title: "[`pylint`] - Allow `__new__` methods to have `cls` as their first argument even if decorated with `@staticmethod` for `bad-staticmethod-argument` (`PLW0211`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-plw0211-for-dunder-new
created_at: 2024-08-17T20:43:54Z
updated_at: 2024-08-22T13:15:28Z
url: https://github.com/astral-sh/ruff/pull/12958
synced_at: 2026-01-10T21:38:32Z
```

# [`pylint`] - Allow `__new__` methods to have `cls` as their first argument even if decorated with `@staticmethod` for `bad-staticmethod-argument` (`PLW0211`)

---

_Pull request opened by @diceroll123 on 2024-08-17 20:43_

## Summary

Fixes #12930

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-08-17 20:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-18 15:29_

Thanks!

---

_Renamed from "[`pylint`] - fix `__new__(cls, ...)` for `bad-staticmethod-argument` (`PLW0211`)" to "[`pylint`] - Allow `__new__` methods to have `cls` as their first argument even if decorated with `@staticmethod` for `bad-staticmethod-argument` (`PLW0211`)" by @AlexWaygood on 2024-08-18 15:30_

---

_Merged by @AlexWaygood on 2024-08-18 15:30_

---

_Closed by @AlexWaygood on 2024-08-18 15:30_

---

_Label `bug` added by @dhruvmanila on 2024-08-22 13:15_

---
