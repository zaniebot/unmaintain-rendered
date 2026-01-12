```yaml
number: 21117
title: "[ty] follow the breaking API changes made in salsa-rs/salsa#1015"
type: pull_request
state: merged
author: mtshiba
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: update-salsa
created_at: 2025-10-29T01:40:50Z
updated_at: 2025-10-30T15:31:02Z
url: https://github.com/astral-sh/ruff/pull/21117
synced_at: 2026-01-12T15:57:16Z
```

# [ty] follow the breaking API changes made in salsa-rs/salsa#1015

---

_@mtshiba_

## Summary

In preparation for merging #20566

This PR updates salsa to follow the breaking API changes made in salsa-rs/salsa#1015.
This PR itself does not change any behavior.

## Test Plan

N/A

---

_Comment by @github-actions[bot] on 2025-10-29 01:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-29 01:42:53.428628100 +0000
+++ new-output.txt	2025-10-29 01:42:56.572654354 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/25b3ef1/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-29 01:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-29 01:59_

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

_Marked ready for review by @mtshiba on 2025-10-29 02:25_

---

_Review requested from @carljm by @mtshiba on 2025-10-29 02:25_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-29 02:25_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-29 02:25_

---

_Review requested from @dcreager by @mtshiba on 2025-10-29 02:25_

---

_Label `ty` added by @AlexWaygood on 2025-10-29 12:31_

---

_Label `internal` added by @MichaReiser on 2025-10-29 14:55_

---

_@MichaReiser approved on 2025-10-29 14:56_

Thank you

---

_Merged by @MichaReiser on 2025-10-29 14:56_

---

_Closed by @MichaReiser on 2025-10-29 14:56_

---

_Branch deleted on 2025-10-30 15:31_

---
