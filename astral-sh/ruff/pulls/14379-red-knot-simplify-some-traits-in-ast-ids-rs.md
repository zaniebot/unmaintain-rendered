```yaml
number: 14379
title: "[red-knot] Simplify some traits in `ast_ids.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-trait
created_at: 2024-11-16T15:37:12Z
updated_at: 2024-11-16T17:31:28Z
url: https://github.com/astral-sh/ruff/pull/14379
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Simplify some traits in `ast_ids.rs`

---

_@AlexWaygood_

## Summary

Another small cleanup I'm splitting out from the branch I have locally working towards https://github.com/astral-sh/ruff/issues/13933. The traits `HasScopedUseId` and `HasScopedAstId` don't need to be generic: for both traits, the `Id` associated type is the same for all implementations of the trait. We can remove some complexity here by removing the associated types and specifying concrete types in the trait definitions rather than associated types.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `internal` added by @AlexWaygood on 2024-11-16 15:37_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-16 15:37_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-16 15:37_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-16 15:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-16 15:37_

---

_Comment by @github-actions[bot] on 2024-11-16 15:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:79 on 2024-11-16 16:37_

We should rename the trait if we remove the type parameter to `HasScopedExpressionId`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:81 on 2024-11-16 16:37_

```suggestion
    fn scoped_expression_id(&self, db: &dyn Db, scope: ScopeId) -> ScopedExpressionId;
```

---

_@MichaReiser approved on 2024-11-16 16:39_

The traits were generic because we used to have many different `AstIds`, more than just use and expressions. 

I do think removing the associated types makes sense if we only have two ids. An alternative design would be to unify the two traits instead, and use the associated type to differentiate between the different ids. 

---

_Merged by @AlexWaygood on 2024-11-16 17:22_

---

_Closed by @AlexWaygood on 2024-11-16 17:22_

---

_Branch deleted on 2024-11-16 17:22_

---
