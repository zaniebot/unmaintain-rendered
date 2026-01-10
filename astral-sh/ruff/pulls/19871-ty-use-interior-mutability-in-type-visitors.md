```yaml
number: 19871
title: "[ty] use interior mutability in type visitors"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/visitor-interior-mutability
created_at: 2025-08-11T21:33:06Z
updated_at: 2025-08-11T22:42:56Z
url: https://github.com/astral-sh/ruff/pull/19871
synced_at: 2026-01-10T17:52:17Z
```

# [ty] use interior mutability in type visitors

---

_Pull request opened by @carljm on 2025-08-11 21:33_

## Summary

Type visitors are conceptually immutable, they just internally track the types they've seen (and some maintain a cache of results.) Passing around mutable visitors everywhere can get us into borrow-checker trouble in some cases, where we need to recursively pass along the visitor inside more than one closure with non-disjoint lifetime.

Use interior mutability (via `RefCell` and `Cell`) inside the visitors instead, to allow us to pass around shared references.

## Test Plan

Existing tests.


---

_Label `internal` added by @carljm on 2025-08-11 21:33_

---

_Label `ty` added by @carljm on 2025-08-11 21:33_

---

_Comment by @github-actions[bot] on 2025-08-11 21:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-11 21:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-08-11 22:10_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-11 22:10_

---

_Review requested from @sharkdp by @carljm on 2025-08-11 22:10_

---

_Review requested from @dcreager by @carljm on 2025-08-11 22:10_

---

_@AlexWaygood approved on 2025-08-11 22:22_

---

_Merged by @carljm on 2025-08-11 22:42_

---

_Closed by @carljm on 2025-08-11 22:42_

---

_Branch deleted on 2025-08-11 22:42_

---
