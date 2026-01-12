```yaml
number: 20797
title: "[ty] Simplify and fix `CallableTypeOf[..]` implementation"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-1331
created_at: 2025-10-10T08:34:17Z
updated_at: 2025-10-10T10:04:39Z
url: https://github.com/astral-sh/ruff/pull/20797
synced_at: 2026-01-12T15:57:10Z
```

# [ty] Simplify and fix `CallableTypeOf[..]` implementation

---

_@sharkdp_

## Summary

Simplify and fix the implementation of `ty_extensions.CallableTypeOf[..]`.

closes https://github.com/astral-sh/ty/issues/1331

## Test Plan

Added regression test.

---

_Label `ty` added by @sharkdp on 2025-10-10 08:34_

---

_Review requested from @carljm by @sharkdp on 2025-10-10 08:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-10 08:34_

---

_Review requested from @dcreager by @sharkdp on 2025-10-10 08:34_

---

_@sharkdp reviewed on 2025-10-10 08:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/ty_extensions.md`:505 on 2025-10-10 08:34_

This was `(self, x: int) -> str` before

---

_Comment by @github-actions[bot] on 2025-10-10 08:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-10 08:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-10-10 09:57_

The nice thing about the previous implementation was it _was_ quite good at exposing places where `Type::bindings()` was inconsistent with `Type::into_callable()` 😄

but that's not really a good reason to keep a buggy implementation around

---

_Merged by @sharkdp on 2025-10-10 10:04_

---

_Closed by @sharkdp on 2025-10-10 10:04_

---

_Branch deleted on 2025-10-10 10:04_

---
