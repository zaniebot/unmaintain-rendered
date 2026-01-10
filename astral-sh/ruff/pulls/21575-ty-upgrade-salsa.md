```yaml
number: 21575
title: "[ty] upgrade salsa"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/upgrade-salsa
created_at: 2025-11-22T02:19:33Z
updated_at: 2025-11-22T10:46:59Z
url: https://github.com/astral-sh/ruff/pull/21575
synced_at: 2026-01-10T16:48:02Z
```

# [ty] upgrade salsa

---

_Pull request opened by @carljm on 2025-11-22 02:19_

## Summary

Upgrade salsa to the latest version, with some changes needed to cycle handling.

## Test Plan

CI


---

_Label `internal` added by @carljm on 2025-11-22 02:19_

---

_Label `ty` added by @carljm on 2025-11-22 02:19_

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 02:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-22 02:21:21.214707918 +0000
+++ new-output.txt	2025-11-22 02:21:24.712727161 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:465:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19ea3)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:469:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19ea3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-22 02:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-22 02:37_


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

_Marked ready for review by @carljm on 2025-11-22 02:50_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-22 02:50_

---

_Review requested from @sharkdp by @carljm on 2025-11-22 02:50_

---

_Review requested from @dcreager by @carljm on 2025-11-22 02:50_

---

_@AlexWaygood approved on 2025-11-22 10:06_

---

_@MichaReiser approved on 2025-11-22 10:46_

---

_Merged by @MichaReiser on 2025-11-22 10:46_

---

_Closed by @MichaReiser on 2025-11-22 10:46_

---

_Branch deleted on 2025-11-22 10:46_

---
