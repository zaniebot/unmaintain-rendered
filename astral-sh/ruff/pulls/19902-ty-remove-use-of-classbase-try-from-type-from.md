```yaml
number: 19902
title: "[ty] Remove use of `ClassBase::try_from_type` from `super()` machinery"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/superownerkind
created_at: 2025-08-13T18:04:14Z
updated_at: 2025-08-14T21:14:33Z
url: https://github.com/astral-sh/ruff/pull/19902
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Remove use of `ClassBase::try_from_type` from `super()` machinery

---

_Pull request opened by @AlexWaygood on 2025-08-13 18:04_

Followup to conversation in https://github.com/astral-sh/ruff/pull/19560#discussion_r2271570071. `ClassBase::try_from_type` doesn't implement the right semantics here.

---

_Review requested from @carljm by @AlexWaygood on 2025-08-13 18:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-13 18:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-13 18:04_

---

_Label `ty` added by @AlexWaygood on 2025-08-13 18:04_

---

_Comment by @github-actions[bot] on 2025-08-13 18:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 18:25:27.632317118 +0000
+++ new-output.txt	2025-08-13 18:25:27.700317434 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(88c5)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(14a57)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 18:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-14 21:12_

Awesome!

---

_Merged by @AlexWaygood on 2025-08-14 21:14_

---

_Closed by @AlexWaygood on 2025-08-14 21:14_

---

_Branch deleted on 2025-08-14 21:14_

---
