```yaml
number: 19911
title: "[ty] Rename `functionArgumentNames` to `callArgumentNames` inlay hint setting"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - configuration
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/rename-inlay-hint-setting
created_at: 2025-08-14T08:46:58Z
updated_at: 2025-08-14T08:51:40Z
url: https://github.com/astral-sh/ruff/pull/19911
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Rename `functionArgumentNames` to `callArgumentNames` inlay hint setting

---

_Pull request opened by @dhruvmanila on 2025-08-14 08:46_

## Summary

This PR renames `ty.inlayHints.functionArgumentNames` to `ty.inlayHints.callArgumentNames` which would contain both function calls and class initialization calls i.e., it represents a generic call expression.


---

_Review requested from @carljm by @dhruvmanila on 2025-08-14 08:46_

---

_Label `internal` added by @dhruvmanila on 2025-08-14 08:46_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-08-14 08:46_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-14 08:46_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-14 08:46_

---

_Label `configuration` added by @dhruvmanila on 2025-08-14 08:46_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-14 08:46_

---

_Label `server` added by @dhruvmanila on 2025-08-14 08:46_

---

_Label `ty` added by @dhruvmanila on 2025-08-14 08:46_

---

_Comment by @github-actions[bot] on 2025-08-14 08:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 08:48:27.288427486 +0000
+++ new-output.txt	2025-08-14 08:48:27.353427566 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(c104)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(2f8d6)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-14 08:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @dhruvmanila on 2025-08-14 08:51_

---

_Closed by @dhruvmanila on 2025-08-14 08:51_

---

_Branch deleted on 2025-08-14 08:51_

---
