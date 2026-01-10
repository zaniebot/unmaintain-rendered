```yaml
number: 16820
title: "[red-knot] Simplify `IterationError` and `ContextManagerError`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/try-iterate-simpler
created_at: 2025-03-17T18:51:26Z
updated_at: 2025-03-18T11:31:41Z
url: https://github.com/astral-sh/ruff/pull/16820
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Simplify `IterationError` and `ContextManagerError`

---

_Pull request opened by @AlexWaygood on 2025-03-17 18:51_

## Summary

This PR simplifies `IterationError` and `ContextManagerError` so that they no longer "remember" what type it was that was (respectively) not iterable or not valid as a context manager. Instead, the type that was iterated over (or was used as a context manager) is passed back in when calling the error struct's `report_diagnostic` method.

The motivations for this are:
- It significantly simplifies the code
- It reduces the size of these types on the stack

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-03-17 18:51_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-17 18:51_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-17 18:51_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-17 18:51_

---

_Comment by @github-actions[bot] on 2025-03-17 18:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3689 on 2025-03-17 20:30_

While we're here, should we `iterable_ty` → `iterable_type`?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/unpacker.rs`:59 on 2025-03-17 20:30_

and `value_ty` → `value_type`?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3675 on 2025-03-17 20:32_

Ahhh, the method even already existed, nice!

---

_@dcreager approved on 2025-03-17 20:32_

Love it

---

_@carljm approved on 2025-03-17 20:43_

---

_Merged by @AlexWaygood on 2025-03-18 11:30_

---

_Closed by @AlexWaygood on 2025-03-18 11:30_

---

_Branch deleted on 2025-03-18 11:30_

---
