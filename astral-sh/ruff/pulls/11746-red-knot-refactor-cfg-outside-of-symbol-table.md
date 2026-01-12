```yaml
number: 11746
title: "[red-knot] refactor CFG outside of symbol table"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/refactor
created_at: 2024-06-05T00:43:28Z
updated_at: 2024-06-05T12:23:44Z
url: https://github.com/astral-sh/ruff/pull/11746
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] refactor CFG outside of symbol table

---

_@carljm_

## Summary

Separate SymbolTable and FlowGraph into their own modules, and make both submodules of `red_knot::semantic` module. Also move `types` module into `semantic`.

`red_knot::semantic::SemanticIndexer` builds a `SemanticIndex` (consisting of the symbol table and flow graph). The `symbol_table` query is now `semantic_index`, and `SymbolTablesStorage` is `SemanticIndexStorage`.

Both `SymbolTable` and `FlowGraph` are immutable, and constructed using a separate builder.

Fixes #11657.

## Test Plan

Refactor; existing tests pass.


---

_Label `red-knot` added by @carljm on 2024-06-05 00:43_

---

_Review requested from @MichaReiser by @carljm on 2024-06-05 00:43_

---

_Comment by @github-actions[bot] on 2024-06-05 00:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-06-05 07:10_

Thanks for doing this. I like this a lot. It makes the symbol module much more approachable.

---

_Merged by @carljm on 2024-06-05 12:23_

---

_Closed by @carljm on 2024-06-05 12:23_

---

_Branch deleted on 2024-06-05 12:23_

---
