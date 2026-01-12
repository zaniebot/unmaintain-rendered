```yaml
number: 18536
title: "[ty] Fix panic when pulling types for `UnaryOp` expressions inside `Literal` slices"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/neg-literal-crash
created_at: 2025-06-07T15:21:51Z
updated_at: 2025-06-11T10:48:09Z
url: https://github.com/astral-sh/ruff/pull/18536
synced_at: 2026-01-12T15:56:21Z
```

# [ty] Fix panic when pulling types for `UnaryOp` expressions inside `Literal` slices

---

_@AlexWaygood_

## Summary

We panicked if we tried to pull types for something like `Literal[-1]`.

## Test Plan

- Removed many typeshed files from the corpus test ignorelist
- Added a new snippet to our corpus tests so that we have coverage even if typeshed changes


---

_Label `ty` added by @AlexWaygood on 2025-06-07 15:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-07 15:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-07 15:21_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-07 15:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-07 15:21_

---

_Comment by @github-actions[bot] on 2025-06-07 15:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-06-07 15:26_

---

_Closed by @AlexWaygood on 2025-06-07 15:26_

---

_Branch deleted on 2025-06-07 15:26_

---

_Label `bug` added by @dhruvmanila on 2025-06-11 10:48_

---
