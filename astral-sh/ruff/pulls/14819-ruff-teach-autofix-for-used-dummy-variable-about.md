```yaml
number: 14819
title: "[`ruff`] Teach autofix for `used-dummy-variable` about TypeVars etc. (`RUF052`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alex/typevar-rename
created_at: 2024-12-06T14:21:34Z
updated_at: 2024-12-06T17:05:52Z
url: https://github.com/astral-sh/ruff/pull/14819
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Teach autofix for `used-dummy-variable` about TypeVars etc. (`RUF052`)

---

_@AlexWaygood_

## Summary

For some bindings, such as assignments where the right-hand-side is a `TypeVar` constructor call, renaming the binding can result in invalid code. For example, this diff that `renamer.rs` might make would take code that mypy (or any other spec-compliant type checker) would have accepted, and turns it into something a type checker will reject:

```diff
  from typing import TypeVar

- _T = TypeVar("_T")
+ T = TypeVar("_T")
```

This is because the first argument to the `TypeVar` constructor must be the same as the variable the typevar is being bound to. Detecting this issue in general is impossible (there are lots of classes where this pattern is required), but it seems reasonable to special-case several standard-library classes where this pattern is required.

Fixes #14798


## Test Plan

Added some more fixtures to RUF052 to check that the new logic in `renamer.rs` works as expected.


---

_Label `bug` added by @AlexWaygood on 2024-12-06 14:21_

---

_Label `fixes` added by @AlexWaygood on 2024-12-06 14:21_

---

_Label `preview` added by @AlexWaygood on 2024-12-06 14:21_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-12-06 14:21_

---

_Comment by @github-actions[bot] on 2024-12-06 14:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-06 16:33_

Do you know what editors do when you rename a variable that's a `TypeVar` declaration?  

---

_@MichaReiser approved on 2024-12-06 16:34_

This is very cool! Nice work

---

_Comment by @AlexWaygood on 2024-12-06 17:05_

> Do you know what editors do when you rename a variable that's a `TypeVar` declaration?

VSCode just does this, so we'll do a better job than them with this change ;)

```diff
-_T = TypeVar("_T")
+T = TypeVar("_T")
```

---

_Merged by @AlexWaygood on 2024-12-06 17:05_

---

_Closed by @AlexWaygood on 2024-12-06 17:05_

---

_Branch deleted on 2024-12-06 17:05_

---
