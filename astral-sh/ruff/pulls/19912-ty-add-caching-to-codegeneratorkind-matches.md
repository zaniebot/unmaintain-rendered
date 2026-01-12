```yaml
number: 19912
title: "[ty] Add caching to `CodeGeneratorKind::matches()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-codegenerator-standalone
created_at: 2025-08-14T10:32:48Z
updated_at: 2025-08-14T10:54:12Z
url: https://github.com/astral-sh/ruff/pull/19912
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Add caching to `CodeGeneratorKind::matches()`

---

_@AlexWaygood_

https://github.com/astral-sh/ruff/pull/19901 has been approved, but... not the PR that it's stacked on top of :-). This PR experiments to see whether #19901 makes sense as a standalone change.

---

_Label `performance` added by @AlexWaygood on 2025-08-14 10:32_

---

_Label `ty` added by @AlexWaygood on 2025-08-14 10:32_

---

_Label `performance` added by @AlexWaygood on 2025-08-14 10:32_

---

_Label `ty` added by @AlexWaygood on 2025-08-14 10:32_

---

_Comment by @github-actions[bot] on 2025-08-14 10:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 10:34:28.091971548 +0000
+++ new-output.txt	2025-08-14 10:34:28.158971487 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(174df)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(120ec)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-14 10:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-14 10:54_

Speedups of 2-3% on the walltime benchmarks 🥳

---

_Marked ready for review by @AlexWaygood on 2025-08-14 10:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-14 10:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-14 10:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-14 10:54_

---

_Merged by @AlexWaygood on 2025-08-14 10:54_

---

_Closed by @AlexWaygood on 2025-08-14 10:54_

---

_Branch deleted on 2025-08-14 10:54_

---
