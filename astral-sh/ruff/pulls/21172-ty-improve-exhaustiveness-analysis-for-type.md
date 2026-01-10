```yaml
number: 21172
title: "[ty] Improve exhaustiveness analysis for type variables with bounds or constraints"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/neg-intersection2
created_at: 2025-10-31T19:07:29Z
updated_at: 2025-10-31T20:51:13Z
url: https://github.com/astral-sh/ruff/pull/21172
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Improve exhaustiveness analysis for type variables with bounds or constraints

---

_Pull request opened by @AlexWaygood on 2025-10-31 19:07_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1456.

This PR backs out the changes made in https://github.com/astral-sh/ruff/pull/21130 and replaces them with a more generalised implementation of exhaustiveness checking for `match`es over type variables with upper bounds or constraints. The approach in #21130 works for type variables with an enum upper bound, and could have been adapted to work for type variables with a `bool` upper bound. But it would _not_ work for a type variable with a union upper bound, because of the fact that we distribute over a union (to create a union of intersections) when adding a union type to an intersection.

Instead, the approach this PR takes is to speculatively solve all type variables in the positive part of an intersection to their upper bound/constraints when finally building the intersection. If that speculative intersection simplifies to `Never`, so must the original intersection.

## Test Plan

New mdtests added.


---

_Label `ty` added by @AlexWaygood on 2025-10-31 19:07_

---

_Comment by @github-actions[bot] on 2025-10-31 19:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 19:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-10-31 19:41_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-31 19:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-31 19:41_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-31 19:41_

---

_@carljm approved on 2025-10-31 20:46_

Very nice!

---

_Merged by @AlexWaygood on 2025-10-31 20:51_

---

_Closed by @AlexWaygood on 2025-10-31 20:51_

---

_Branch deleted on 2025-10-31 20:51_

---
