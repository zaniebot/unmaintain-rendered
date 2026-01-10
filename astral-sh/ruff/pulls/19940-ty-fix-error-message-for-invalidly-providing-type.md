```yaml
number: 19940
title: "[ty] Fix error message for invalidly providing type arguments to `NamedTuple` when it occurs in a type expression"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/namedtuple-type-expression2
created_at: 2025-08-16T17:41:50Z
updated_at: 2025-08-16T17:46:16Z
url: https://github.com/astral-sh/ruff/pull/19940
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix error message for invalidly providing type arguments to `NamedTuple` when it occurs in a type expression

---

_Pull request opened by @AlexWaygood on 2025-08-16 17:41_

## Summary

This fixes a minor oversight in https://github.com/astral-sh/ruff/pull/19915 (which hasn't appeared in a release yet, so I'm adding the `internal` label to this PR)

## Test Plan

mdtest


---

_Review requested from @carljm by @AlexWaygood on 2025-08-16 17:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-16 17:41_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-16 17:41_

---

_Label `internal` added by @AlexWaygood on 2025-08-16 17:41_

---

_Label `ty` added by @AlexWaygood on 2025-08-16 17:41_

---

_Comment by @github-actions[bot] on 2025-08-16 17:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-16 17:43:55.228315107 +0000
+++ new-output.txt	2025-08-16 17:43:55.301315377 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(b082)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(c9f8)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Merged by @AlexWaygood on 2025-08-16 17:45_

---

_Closed by @AlexWaygood on 2025-08-16 17:45_

---

_Branch deleted on 2025-08-16 17:45_

---

_Comment by @github-actions[bot] on 2025-08-16 17:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
