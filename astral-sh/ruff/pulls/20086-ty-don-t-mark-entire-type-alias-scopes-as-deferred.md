```yaml
number: 20086
title: "[ty] don't mark entire type-alias scopes as Deferred"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/typealias-deferred
created_at: 2025-08-25T17:47:37Z
updated_at: 2025-08-25T18:32:20Z
url: https://github.com/astral-sh/ruff/pull/20086
synced_at: 2026-01-12T15:56:54Z
```

# [ty] don't mark entire type-alias scopes as Deferred

---

_@carljm_

## Summary

This has been here for awhile (since our initial PEP 695 type alias support) but isn't really correct. The right-hand-side of a PEP 695 type alias is a distinct scope, and we don't mark it as an "eager" nested scope, so it automatically gets "deferred" resolution of names from outer scopes (just like  a nested function). Thus it's redundant/unnecessary for us to use `DeferredExpressionState::Deferred` for resolving that RHS expression -- that's for deferring resolution of individual names within a scope. Using it here causes us to wrongly ignore applicable outer-scope narrowing.

## Test Plan

Added mdtest that failed before this PR (the second snippet -- the first snippet always passed.)


---

_Review requested from @AlexWaygood by @carljm on 2025-08-25 17:47_

---

_Label `ty` added by @carljm on 2025-08-25 17:47_

---

_Review requested from @sharkdp by @carljm on 2025-08-25 17:47_

---

_Review requested from @dcreager by @carljm on 2025-08-25 17:47_

---

_Comment by @github-actions[bot] on 2025-08-25 17:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-25 17:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-08-25 18:31_

nice!

---

_Merged by @carljm on 2025-08-25 18:32_

---

_Closed by @carljm on 2025-08-25 18:32_

---

_Branch deleted on 2025-08-25 18:32_

---
