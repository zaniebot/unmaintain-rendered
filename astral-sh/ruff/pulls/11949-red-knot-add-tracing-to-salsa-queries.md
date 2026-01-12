```yaml
number: 11949
title: "[red-knot] Add tracing to Salsa queries"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-tracing
created_at: 2024-06-20T11:22:45Z
updated_at: 2024-06-20T11:39:22Z
url: https://github.com/astral-sh/ruff/pull/11949
synced_at: 2026-01-12T15:55:39Z
```

# [red-knot] Add tracing to Salsa queries

---

_@MichaReiser_

## Summary

This PR adds tracing instrumentation to Salsa queries. 

I intentionally didn't use the `#[tracing::instrument]` attribute because, depending on the ordering, the attribute is applied to the query implementation only or the query implementation and the `get` function that retrieves the cached value or calls the query implementation.
Using `debug_span!` avoids the ambiguity and has the added benefit that we can call the `DebugWithDb` implementation instead of just `debug` on the arguments. 



---

_Review requested from @carljm by @MichaReiser on 2024-06-20 11:22_

---

_Label `red-knot` added by @MichaReiser on 2024-06-20 11:23_

---

_Merged by @MichaReiser on 2024-06-20 11:33_

---

_Closed by @MichaReiser on 2024-06-20 11:33_

---

_Branch deleted on 2024-06-20 11:33_

---

_Comment by @github-actions[bot] on 2024-06-20 11:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
