```yaml
number: 19891
title: "[ty] Various minor cleanups to tuple internals"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-cleanups
created_at: 2025-08-13T09:43:18Z
updated_at: 2025-08-13T13:47:18Z
url: https://github.com/astral-sh/ruff/pull/19891
synced_at: 2026-01-12T15:56:49Z
```

# [ty] Various minor cleanups to tuple internals

---

_@AlexWaygood_

## Summary

Various minor cleanups split out from https://github.com/astral-sh/ruff/pull/19877, which we decided not to go with:
- Rename `Tuple::from_elements()` to `Tuple::heterogeneous()`. It better expresses what the method does to external callers.
- Refactor `TupleType::empty()` and `TupleType::homogeneous()` to return `TupleType<'db>` rather than `Option<TupleType<'db>>`. They always return `Some()` on `main`, but this isn't currently expressed by the type signature. Refactoring them to return `TupleType<'db>` avoids the need for one `.expect()` call and one `.unwrap()` call.
- Remove the complicated trait bounds from `TupleType::new()`. These were originally a performance optimisation, but we're no longer sure it buys us anything: see https://github.com/astral-sh/ruff/pull/19877#discussion_r2270156985.
- Move `Type::homogeneous_tuple()`, `Type::empty_tuple()` and `Type::heterogeneous_tuple()` from `tuple.rs` to `instance.rs`. The same functionality can be implemented with less indirection in that submodule, because we have access to the `NominalInstanceType` internals from that module.

## Test Plan

Existing tests


---

_Label `internal` added by @AlexWaygood on 2025-08-13 09:43_

---

_Label `ty` added by @AlexWaygood on 2025-08-13 09:43_

---

_Comment by @github-actions[bot] on 2025-08-13 09:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 13:44:30.744994771 +0000
+++ new-output.txt	2025-08-13 13:44:30.811995136 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(3cd6)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(2cbf)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 09:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-13 09:58_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-13 09:58_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-13 09:58_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-13 09:58_

---

_@carljm approved on 2025-08-13 13:41_

---

_Merged by @AlexWaygood on 2025-08-13 13:46_

---

_Closed by @AlexWaygood on 2025-08-13 13:46_

---

_Branch deleted on 2025-08-13 13:46_

---
