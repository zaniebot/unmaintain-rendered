```yaml
number: 21122
title: "[ty] Test performance impact of upstream salsa change"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: micha/salsa-test-perf
created_at: 2025-10-29T13:35:36Z
updated_at: 2025-11-16T11:46:50Z
url: https://github.com/astral-sh/ruff/pull/21122
synced_at: 2026-01-12T15:57:16Z
```

# [ty] Test performance impact of upstream salsa change

---

_@MichaReiser_

_No description provided._

---

_Label `internal` added by @MichaReiser on 2025-10-29 13:35_

---

_Label `ty` added by @MichaReiser on 2025-10-29 13:35_

---

_Comment by @github-actions[bot] on 2025-10-29 13:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-29 13:38:58.485920196 +0000
+++ new-output.txt	2025-10-29 13:39:01.609944959 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/25b3ef1/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/e0ad0ff/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-29 13:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-29 13:53_

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

_Closed by @MichaReiser on 2025-10-29 13:54_

---

_Branch deleted on 2025-11-16 11:46_

---
