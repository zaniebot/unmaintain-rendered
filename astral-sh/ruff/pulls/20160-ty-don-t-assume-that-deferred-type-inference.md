```yaml
number: 20160
title: "[ty] don't assume that deferred type inference means deferred name resolution"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/deferred
created_at: 2025-08-29T19:20:43Z
updated_at: 2025-08-29T23:19:46Z
url: https://github.com/astral-sh/ruff/pull/20160
synced_at: 2026-01-12T15:56:55Z
```

# [ty] don't assume that deferred type inference means deferred name resolution

---

_@carljm_

## Summary

We have the ability to defer type inference of some parts of definitions, so as to allow us to create a type that may need to be recursively referenced in those other parts of the definition.

We also have the ability to do type inference in a context where all name resolution should be deferred (that is, names should be looked up from all-reachable-definitions rather than from the location of use.) This is used for all annotations in stubs, or if `from __future__ import annotations` is active.

Previous to this PR, these two concepts were linked: deferred-inference always implied deferred-name-resolution, though we also supported deferred-name-resolution without deferred-inference, via `DeferredExpressionState`.

For the upcoming `typing.TypeAlias` support, I will defer inference of the entire RHS of the alias (so as to support cycles), but that doesn't imply deferred name resolution; at runtime, the RHS of a name annotated as `typing.TypeAlias` is executed eagerly.

So this PR fully de-couples the two concepts, instead explicitly setting the `DeferredExpressionState` in those cases where we should defer name resolution.

It also fixes a long-standing related bug, where we were deferring name resolution of all names in class bases, if any of the class bases contained a stringified annotation.

## Test Plan

Added test that failed before this PR.


---

_Label `ty` added by @carljm on 2025-08-29 19:20_

---

_Comment by @github-actions[bot] on 2025-08-29 19:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 19:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-29 20:38_

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

_Marked ready for review by @carljm on 2025-08-29 20:43_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-29 20:43_

---

_Review requested from @sharkdp by @carljm on 2025-08-29 20:43_

---

_Review requested from @dcreager by @carljm on 2025-08-29 20:43_

---

_@AlexWaygood approved on 2025-08-29 23:13_

Nice!

---

_Merged by @carljm on 2025-08-29 23:19_

---

_Closed by @carljm on 2025-08-29 23:19_

---

_Branch deleted on 2025-08-29 23:19_

---
