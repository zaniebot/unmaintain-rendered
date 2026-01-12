```yaml
number: 20325
title: "[ty] Use 'unknown' specialization for upper bound on Self"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1156
created_at: 2025-09-10T08:57:05Z
updated_at: 2025-09-10T15:00:30Z
url: https://github.com/astral-sh/ruff/pull/20325
synced_at: 2026-01-12T15:56:59Z
```

# [ty] Use 'unknown' specialization for upper bound on Self

---

_@sharkdp_

## Summary

closes https://github.com/astral-sh/ty/issues/1156

## Test Plan

Added a regression test


---

_Label `bug` added by @sharkdp on 2025-09-10 08:57_

---

_Label `ty` added by @sharkdp on 2025-09-10 08:57_

---

_Comment by @github-actions[bot] on 2025-09-10 08:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 09:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-09-10 09:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5713 on 2025-09-10 09:17_

This would previously use the "default" specialization, and so we would end up with an upper bound of `Container[bytes]` for the example in the mdtest. With the change here, we instead use `Container[Unknown]` as the upper bound.

---

_@sharkdp reviewed on 2025-09-10 09:18_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:5 on 2025-09-10 09:18_

Default values for type parameters have only been introduced recently (https://peps.python.org/pep-0696/)

---

_Marked ready for review by @sharkdp on 2025-09-10 09:18_

---

_Review requested from @carljm by @sharkdp on 2025-09-10 09:18_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-10 09:18_

---

_Review requested from @dcreager by @sharkdp on 2025-09-10 09:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5711 on 2025-09-10 13:58_

I wondered if `class.top_materialization()` would work here, but it looks like under the hood that uses `default_specialization()`, so it suffers from the same issue as our current logic on `main`. (It probably _shouldn't_ use `default_specialization()`?)

---

_@AlexWaygood approved on 2025-09-10 13:58_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5711 on 2025-09-10 14:20_

> It probably _shouldn't_ use `default_specialization()`

I think you're right — it should translate typevars into the bound/constraints in covariant position, and to `Never` (the implicit lower bound of every typevar) in contravariant position. (With the caveat that we can't really express the constraints of a constrained typevar as a single type — we'd need a "one-of" connective instead of union, and an "instance of this type but not any subtypes".)

---

_@dcreager approved on 2025-09-10 14:21_

Thank you for tracking this down!

---

_@sharkdp reviewed on 2025-09-10 15:00_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5711 on 2025-09-10 15:00_

I opened https://github.com/astral-sh/ty/issues/1164

---

_Merged by @sharkdp on 2025-09-10 15:00_

---

_Closed by @sharkdp on 2025-09-10 15:00_

---

_Branch deleted on 2025-09-10 15:00_

---
