```yaml
number: 19949
title: "[ty] Remove unused code"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/unused-ty-code
created_at: 2025-08-17T14:52:08Z
updated_at: 2025-08-17T17:54:25Z
url: https://github.com/astral-sh/ruff/pull/19949
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Remove unused code

---

_@AlexWaygood_

## Summary

Most functions in `types.rs` do not need to be `pub`. Them being `pub` prevents Clippy from warning about unused code, and also seems to suppress other useful lints such as `trivially_copy_pass_by_ref`.

## Test Plan

`cargo build`


---

_Label `internal` added by @AlexWaygood on 2025-08-17 14:52_

---

_Label `ty` added by @AlexWaygood on 2025-08-17 14:52_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-17 14:52_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-17 14:52_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-17 14:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-17 14:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9152 on 2025-08-17 14:53_

I don't think we really need the indirection here, and removing this method allowed Clippy to point out that there was a callsite in `arguments.rs` where a `union.elements(db).to_vec()` call would be more efficient than calling `union.elements(db).iter().copied().collect()`

---

_@AlexWaygood reviewed on 2025-08-17 14:53_

---

_Comment by @github-actions[bot] on 2025-08-17 14:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-17 14:54:19.115031349 +0000
+++ new-output.txt	2025-08-17 14:54:19.185031945 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1582c)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(ac82)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-17 14:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-08-17 17:46_

I do except that more methods need to be made public to implement LSP and linter features but I'm fine promoting the methods once that need arises

---

_Merged by @AlexWaygood on 2025-08-17 17:54_

---

_Closed by @AlexWaygood on 2025-08-17 17:54_

---

_Branch deleted on 2025-08-17 17:54_

---
