```yaml
number: 14317
title: "[red-knot] Use memory address as AST node key"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14313
created_at: 2024-11-13T12:28:38Z
updated_at: 2024-11-13T13:35:56Z
url: https://github.com/astral-sh/ruff/pull/14317
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Use memory address as AST node key

---

_Pull request opened by @sharkdp on 2024-11-13 12:28_

## Summary

Use the memory address to uniquely identify AST nodes, instead of relying on source range and kind. The latter fails for ASTs resulting from invalid syntax examples. See #14313 for details.

Also results in a 1-2% speedup (https://codspeed.io/astral-sh/ruff/runs/67349cf55f36b36baa211360)

closes #14313 

## Review

Here are the places where we use `NodeKey` directly or indirectly (via `ExpressionNodeKey` or `DefinitionNodeKey`):

```rs
// semantic_index.rs
pub(crate) struct SemanticIndex<'db> { 
    // [...]
    /// Map expressions to their corresponding scope.
    scopes_by_expression: FxHashMap<ExpressionNodeKey, FileScopeId>,

    /// Map from a node creating a definition to its definition.
    definitions_by_node: FxHashMap<DefinitionNodeKey, Definition<'db>>,

    /// Map from a standalone expression to its [`Expression`] ingredient.
    expressions_by_node: FxHashMap<ExpressionNodeKey, Expression<'db>>,
    // [...]
}

// semantic_index/builder.rs
pub(super) struct SemanticIndexBuilder<'db> {
    // [...]
    scopes_by_expression: FxHashMap<ExpressionNodeKey, FileScopeId>,
    definitions_by_node: FxHashMap<ExpressionNodeKey, Definition<'db>>,
    expressions_by_node: FxHashMap<ExpressionNodeKey, Expression<'db>>,
}

// semantic_index/ast_ids.rs
pub(crate) struct AstIds {
    /// Maps expressions to their expression id.
    expressions_map: FxHashMap<ExpressionNodeKey, ScopedExpressionId>,
    /// Maps expressions which "use" a symbol (that is, [`ast::ExprName`]) to a use id.
    uses_map: FxHashMap<ExpressionNodeKey, ScopedUseId>,
}

pub(super) struct AstIdsBuilder {
    expressions_map: FxHashMap<ExpressionNodeKey, ScopedExpressionId>,
    uses_map: FxHashMap<ExpressionNodeKey, ScopedUseId>,
}
```

## Test Plan

Added two failing examples to the corpus.


---

_Label `red-knot` added by @sharkdp on 2024-11-13 12:28_

---

_Review requested from @carljm by @sharkdp on 2024-11-13 12:28_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-13 12:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-13 12:28_

---

_@sharkdp reviewed on 2024-11-13 12:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/node_key.rs`:11 on 2024-11-13 12:30_

Thought about renaming it to `NodeAddress`, but that lead to too many downstream renamings and a large diff. Let me know if someone is unhappy with keeping `NodeKey`.

---

_Comment by @github-actions[bot] on 2024-11-13 12:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/node_key.rs`:6 on 2024-11-13 13:05_

It would be great to add an example here

---

_@MichaReiser approved on 2024-11-13 13:06_

---

_Merged by @sharkdp on 2024-11-13 13:35_

---

_Closed by @sharkdp on 2024-11-13 13:35_

---

_Branch deleted on 2024-11-13 13:35_

---
