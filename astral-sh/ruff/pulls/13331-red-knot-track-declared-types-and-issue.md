```yaml
number: 13331
title: "[red-knot] track declared types and issue diagnostics"
type: pull_request
state: closed
author: carljm
labels: []
assignees: []
draft: true
base: main
head: cjm/declared-types
created_at: 2024-09-12T01:48:00Z
updated_at: 2024-09-12T03:27:26Z
url: https://github.com/astral-sh/ruff/pull/13331
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] track declared types and issue diagnostics

---

_@carljm_

Subdivide Definitions into Bindings (assign a value) and Declarations (declare the acceptable upper bound type for a symbol), and add the ability to query both the inferred (from bindings) and declared types of a symbol at a given point in control flow. Emit diagnostics for bindings or declarations that would violate the "inferred type must be assignable to declared type" invariant for a symbol.

Our interpretation of a local type declaration is control-flow-sensitive, allowing for shadowing, or re-declaring an existing variable with a new type in the same scope.


---

_Comment by @github-actions[bot] on 2024-09-12 02:17_

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

_Closed by @carljm on 2024-09-12 03:27_

---
