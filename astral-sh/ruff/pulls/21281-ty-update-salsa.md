```yaml
number: 21281
title: "[ty] Update salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/update-salsa2
created_at: 2025-11-05T12:57:53Z
updated_at: 2025-11-05T22:15:03Z
url: https://github.com/astral-sh/ruff/pull/21281
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Update salsa

---

_@MichaReiser_

## Summary

Update Salsa to pull in https://github.com/salsa-rs/salsa/pull/1021, which is needed for https://github.com/astral-sh/ruff/pull/20566


## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-11-05 12:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-05 12:57_

---

_Label `internal` added by @MichaReiser on 2025-11-05 12:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-05 12:57_

---

_Label `ty` added by @MichaReiser on 2025-11-05 12:57_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-05 12:57_

---

_Label `internal` added by @MichaReiser on 2025-11-05 12:57_

---

_Label `ty` added by @MichaReiser on 2025-11-05 12:57_

---

_@MichaReiser reviewed on 2025-11-05 12:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:122 on 2025-11-05 12:59_

It would be correcter for us to use `count < ITERATIONS_BEFORE_FALLBACK` but one existing mdtest fails if we do so (but it fixes a corpus test panic).

I decided not to spend any time on debugging this further, given that https://github.com/astral-sh/ruff/pull/20566 is about to solve this holistically

---

_Comment by @github-actions[bot] on 2025-11-05 13:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-05 12:59:52.040959050 +0000
+++ new-output.txt	2025-11-05 12:59:55.325995121 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/664750a/src/function/execute.rs:464:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18af5)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18af5)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-11-05 13:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-05 13:11_

---

_Comment by @github-actions[bot] on 2025-11-05 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@carljm approved on 2025-11-05 21:42_

---

_Merged by @MichaReiser on 2025-11-05 22:15_

---

_Closed by @MichaReiser on 2025-11-05 22:15_

---

_Branch deleted on 2025-11-05 22:15_

---
