```yaml
number: 21178
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-11-01T00:52:01Z
updated_at: 2025-11-01T01:03:43Z
url: https://github.com/astral-sh/ruff/pull/21178
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-11-01 00:52_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2025-11-01 00:52_

---

_Review requested from @carljm by @github-actions[bot] on 2025-11-01 00:52_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-11-01 00:52_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-11-01 00:52_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-11-01 00:52_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-11-01 00:52_

---

_Label `ty` added by @github-actions[bot] on 2025-11-01 00:52_

---

_Closed by @AlexWaygood on 2025-11-01 00:55_

---

_Reopened by @AlexWaygood on 2025-11-01 00:55_

---

_Comment by @github-actions[bot] on 2025-11-01 00:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-01 00:57:02.760322978 +0000
+++ new-output.txt	2025-11-01 00:57:05.906342279 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1784f)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-11-01 00:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyodide (https://github.com/pyodide/pyodide)
- src/py/pyodide/console.py:434:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `_WriteStream` does not satisfy upper bound `IO[str] | None` of type variable `_T_io`
- src/py/pyodide/console.py:438:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `_WriteStream` does not satisfy upper bound `IO[str] | None` of type variable `_T_io`
- Found 986 diagnostics
+ Found 984 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-11-01 01:03_

---

_Closed by @AlexWaygood on 2025-11-01 01:03_

---

_Branch deleted on 2025-11-01 01:03_

---
