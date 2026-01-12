```yaml
number: 11132
title: "[red-knot] remove layer of indirection from SymbolTable"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: cjm/red-knot/symbols-types
created_at: 2024-04-24T15:59:20Z
updated_at: 2024-04-26T20:06:09Z
url: https://github.com/astral-sh/ruff/pull/11132
synced_at: 2026-01-12T15:55:34Z
```

# [red-knot] remove layer of indirection from SymbolTable

---

_@carljm_

## Summary

Now that symbol definitions don't hold direct references to AST nodes, and thus don't have a lifetime bound, there's no longer any reason to separate the "core SymbolTable" from the definitions. All of the symbol table, including definitions, can be cached together, and it all needs to be invalidated together if the module AST changes. So let's simplify this and have fewer structs.

## Test Plan

Existing tests.


---

_Review requested from @MichaReiser by @carljm on 2024-04-24 15:59_

---

_@MichaReiser approved on 2024-04-24 16:00_

Makes sense

---

_Merged by @carljm on 2024-04-24 16:02_

---

_Closed by @carljm on 2024-04-24 16:02_

---

_Branch deleted on 2024-04-24 16:02_

---

_Comment by @github-actions[bot] on 2024-04-24 16:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @carljm on 2024-04-26 20:06_

---
