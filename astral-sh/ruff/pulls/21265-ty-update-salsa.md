```yaml
number: 21265
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
head: micha/update-salsa
created_at: 2025-11-03T21:13:50Z
updated_at: 2025-11-03T22:00:31Z
url: https://github.com/astral-sh/ruff/pull/21265
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Update salsa

---

_Pull request opened by @MichaReiser on 2025-11-03 21:13_

## Summary

Update Salsa to pull in https://github.com/salsa-rs/salsa/pull/1017 and https://github.com/salsa-rs/salsa/pull/1018

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-11-03 21:13_

---

_Label `ty` added by @MichaReiser on 2025-11-03 21:13_

---

_Comment by @github-actions[bot] on 2025-11-03 21:16_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-03 21:43:52.565637100 +0000
+++ new-output.txt	2025-11-03 21:43:55.870642661 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18af5)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/664750a/src/function/execute.rs:464:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18af5)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-11-03 21:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~30MB
+     memo metadata = ~31MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~63MB
+     memo metadata = ~66MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~113MB
+     memo metadata = ~119MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-11-03 21:35_

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

_Merged by @MichaReiser on 2025-11-03 22:00_

---

_Closed by @MichaReiser on 2025-11-03 22:00_

---

_Branch deleted on 2025-11-03 22:00_

---
