```yaml
number: 20338
title: "[ty] Fix `CallableTypeOf[…]` for bound methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/fix-callabletypeof
created_at: 2025-09-10T19:00:55Z
updated_at: 2025-09-10T19:40:23Z
url: https://github.com/astral-sh/ruff/pull/20338
synced_at: 2026-01-12T15:56:59Z
```

# [ty] Fix `CallableTypeOf[…]` for bound methods

---

_@sharkdp_

## Summary

Very confusing if the tools you are using to debug something are themselves buggy :smile:. `CallableTypeOf[bound_method]` would previously bind `self` to the bound method type itself, instead of binding it to the instance type stored inside the bound method type.

## Test Plan

Added regression test

---

_Label `ty` added by @sharkdp on 2025-09-10 19:00_

---

_Review requested from @carljm by @sharkdp on 2025-09-10 19:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-10 19:00_

---

_Review requested from @dcreager by @sharkdp on 2025-09-10 19:00_

---

_Label `internal` added by @sharkdp on 2025-09-10 19:01_

---

_@sharkdp reviewed on 2025-09-10 19:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/ty_extensions.md`:501 on 2025-09-10 19:03_

This would previously reveal `(x: int) -> bound method Foo.returns_self(x: int) -> Foo` instead of `(x: int) -> Foo` (https://play.ty.dev/f4b4b1a6-ae47-4338-9be4-07134afc237a)

---

_@AlexWaygood approved on 2025-09-10 19:04_

---

_Comment by @github-actions[bot] on 2025-09-10 19:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 19:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @sharkdp on 2025-09-10 19:13_

---

_Closed by @sharkdp on 2025-09-10 19:13_

---

_Branch deleted on 2025-09-10 19:13_

---

_@sharkdp reviewed on 2025-09-10 19:40_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:10995 on 2025-09-10 19:40_

I just noticed that I might want to call `bound_method.typing_self_type(db)` instead to handle classmethods correctly. Will take a look.

---
