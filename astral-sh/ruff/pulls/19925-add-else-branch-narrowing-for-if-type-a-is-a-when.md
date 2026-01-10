```yaml
number: 19925
title: "Add `else`-branch narrowing for `if type(a) is A` when `A` is `@final`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-narrow-negate
created_at: 2025-08-15T08:08:08Z
updated_at: 2025-08-15T13:52:32Z
url: https://github.com/astral-sh/ruff/pull/19925
synced_at: 2026-01-10T17:52:17Z
```

# Add `else`-branch narrowing for `if type(a) is A` when `A` is `@final`

---

_Pull request opened by @AlexWaygood on 2025-08-15 08:08_

Fixes https://github.com/astral-sh/ty/issues/996

---

_Label `ty` added by @AlexWaygood on 2025-08-15 08:08_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-15 08:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-15 08:08_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-15 08:08_

---

_Comment by @github-actions[bot] on 2025-08-15 08:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-15 08:09:49.016986621 +0000
+++ new-output.txt	2025-08-15 08:09:49.086987631 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(10451)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(e4a4)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-15 08:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dcreager approved on 2025-08-15 13:50_

---

_Merged by @AlexWaygood on 2025-08-15 13:52_

---

_Closed by @AlexWaygood on 2025-08-15 13:52_

---

_Branch deleted on 2025-08-15 13:52_

---
