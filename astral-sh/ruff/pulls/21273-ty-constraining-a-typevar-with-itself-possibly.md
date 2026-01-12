```yaml
number: 21273
title: "[ty] Constraining a typevar with itself (possibly via union or intersection)"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/union-intersection
created_at: 2025-11-04T13:37:16Z
updated_at: 2025-11-05T17:31:55Z
url: https://github.com/astral-sh/ruff/pull/21273
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Constraining a typevar with itself (possibly via union or intersection)

---

_@dcreager_

This PR carries over some of the `has_relation_to` logic for comparing a typevar with itself. A typevar will specialize to the same type if it's mentioned multiple times, so it is always assignable to and a subtype of itself. (Note that typevars can only specialize to fully static types.) This is also true when the typevar appears in a union on the right-hand side, or in an intersection on the left-hand side. Similarly, a typevar is always disjoint from its negation, so when a negated typevar appears on the left-hand side, the constraint set is never satisfiable.

(Eventually this will allow us to remove the corresponding clauses from `has_relation_to`, but that can't happen until more of #20093 lands.)

---

_Review requested from @carljm by @dcreager on 2025-11-04 13:37_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-04 13:37_

---

_Review requested from @sharkdp by @dcreager on 2025-11-04 13:37_

---

_Review requested from @MichaReiser by @dcreager on 2025-11-04 13:37_

---

_Label `internal` added by @dcreager on 2025-11-04 13:37_

---

_Label `ty` added by @dcreager on 2025-11-04 13:37_

---

_Comment by @github-actions[bot] on 2025-11-04 13:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-04 13:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-11-04 19:09_

---

_Merged by @dcreager on 2025-11-05 17:31_

---

_Closed by @dcreager on 2025-11-05 17:31_

---

_Branch deleted on 2025-11-05 17:31_

---
