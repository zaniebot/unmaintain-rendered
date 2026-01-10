```yaml
number: 19937
title: "[ty] Fix example in environment docs"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/fix-extra-paths-example
created_at: 2025-08-16T14:16:02Z
updated_at: 2025-08-16T14:37:29Z
url: https://github.com/astral-sh/ruff/pull/19937
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix example in environment docs

---

_Pull request opened by @MichaReiser on 2025-08-16 14:16_

## Summary

ty doesn't support `~` expansion in configuration paths (we may add support for it in the future).

Reported in https://github.com/astral-sh/ty/issues/1020#issuecomment-3193694521


---

_Label `ty` added by @MichaReiser on 2025-08-16 14:16_

---

_Review requested from @carljm by @MichaReiser on 2025-08-16 14:16_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-16 14:16_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-16 14:16_

---

_Comment by @github-actions[bot] on 2025-08-16 14:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-16 14:18:46.117813800 +0000
+++ new-output.txt	2025-08-16 14:18:46.184814339 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(61e1)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(13cdb)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-16 14:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `documentation` added by @AlexWaygood on 2025-08-16 14:37_

---

_Merged by @MichaReiser on 2025-08-16 14:37_

---

_Closed by @MichaReiser on 2025-08-16 14:37_

---

_Branch deleted on 2025-08-16 14:37_

---
