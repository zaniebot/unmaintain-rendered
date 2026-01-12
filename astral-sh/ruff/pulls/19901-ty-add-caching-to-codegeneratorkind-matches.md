```yaml
number: 19901
title: "[ty] Add caching to `CodeGeneratorKind::matches()`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - internal
  - performance
  - ty
assignees: []
base: alex/namedtuple-properties
head: alex/optimize-codegenerator
created_at: 2025-08-13T17:02:21Z
updated_at: 2025-08-14T10:54:31Z
url: https://github.com/astral-sh/ruff/pull/19901
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Add caching to `CodeGeneratorKind::matches()`

---

_@AlexWaygood_

## Summary

Stacked on top of https://github.com/astral-sh/ruff/pull/19899.

There are some small performance regressions in the Codspeed report for #19899 -- this appears to mitigate that!

## Test Plan

Existing tests


---

_Label `performance` added by @AlexWaygood on 2025-08-13 17:02_

---

_Label `ty` added by @AlexWaygood on 2025-08-13 17:02_

---

_Comment by @github-actions[bot] on 2025-08-13 17:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 21:44:24.067127850 +0000
+++ new-output.txt	2025-08-13 21:44:24.137128742 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(4c9d)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(acd1)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 17:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-13 17:36_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-13 17:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-13 17:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-13 17:36_

---

_Label `internal` added by @AlexWaygood on 2025-08-13 17:41_

---

_@carljm approved on 2025-08-13 23:56_

Were cycles observed here, or are you adding the cycle handling pre-emptively?

---

_Comment by @AlexWaygood on 2025-08-14 00:05_

Cycles were indeed observed! See e.g. https://github.com/astral-sh/ruff/actions/runs/16944090646/job/48020592511

---

_Comment by @AlexWaygood on 2025-08-14 10:54_

I split this out as a standalone change in https://github.com/astral-sh/ruff/pull/19912

---

_Closed by @AlexWaygood on 2025-08-14 10:54_

---

_Branch deleted on 2025-08-14 10:54_

---
