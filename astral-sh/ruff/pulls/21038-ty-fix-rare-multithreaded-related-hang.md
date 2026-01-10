```yaml
number: 21038
title: "[ty] Fix rare multithreaded related hang"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/update-salsa-cycle-fn
created_at: 2025-10-23T07:05:18Z
updated_at: 2025-10-23T07:35:47Z
url: https://github.com/astral-sh/ruff/pull/21038
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Fix rare multithreaded related hang

---

_Pull request opened by @MichaReiser on 2025-10-23 07:05_

## Summary

This PR updates salsa to pull in https://github.com/salsa-rs/salsa/pull/1010 which fixes two cases where Salsa hangs because it enters an infinite loop when multiple threads are involved in a single fixpoint iteration. 

The update also pulls in https://github.com/salsa-rs/salsa/pull/1012 which a) makes the `cycle_fn` optional if it only returns `Iterate`, allowing us to remove a ton of code and b) exposes the last provisional value and the query id to the `cycle_fn`, which should help with https://github.com/astral-sh/ruff/pull/20566

## Test Plan

Existing tests


---

_Label `bug` added by @MichaReiser on 2025-10-23 07:05_

---

_Label `ty` added by @MichaReiser on 2025-10-23 07:05_

---

_Comment by @github-actions[bot] on 2025-10-23 07:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-23 07:07:10.810910418 +0000
+++ new-output.txt	2025-10-23 07:07:14.248930801 +0000
@@ -1,5 +1,5 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d817)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17c43)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d38145c/src/function/execute.rs:417:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d817)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d38145c/src/function/execute.rs:417:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17c43)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-23 07:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-23 07:24_

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

_Marked ready for review by @MichaReiser on 2025-10-23 07:25_

---

_Review requested from @carljm by @MichaReiser on 2025-10-23 07:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-23 07:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-23 07:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-23 07:25_

---

_Merged by @MichaReiser on 2025-10-23 07:25_

---

_Closed by @MichaReiser on 2025-10-23 07:25_

---

_Branch deleted on 2025-10-23 07:25_

---

_Renamed from "Fix rare multithreaded related hang" to "[ty] Fix rare multithreaded related hang" by @MichaReiser on 2025-10-23 07:25_

---

_Comment by @sharkdp on 2025-10-23 07:35_

> The update also pulls in [salsa-rs/salsa#1012](https://github.com/salsa-rs/salsa/pull/1012) which a) makes the `cycle_fn` optional if it only returns `Iterate`, allowing us to remove a ton of code

ohhh :heart_eyes: 

---
