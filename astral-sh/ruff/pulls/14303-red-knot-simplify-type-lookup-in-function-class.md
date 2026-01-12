```yaml
number: 14303
title: "[red-knot] simplify type lookup in function/class definitions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/deftypes
created_at: 2024-11-13T00:47:57Z
updated_at: 2024-11-13T14:03:24Z
url: https://github.com/astral-sh/ruff/pull/14303
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] simplify type lookup in function/class definitions

---

_@carljm_

When we look up the types of class bases or keywords (`metaclass`), we currently do this little dance: if there are type params, then look up the type using `SemanticModel` in the type-params scope, if not, look up the type directly in the definition's own scope, with support for deferred types.

With inference of function parameter types, I'm now adding another case of this same dance, so I'm motivated to make it a bit more ergonomic.

Add support to `definition_expression_ty` to handle any sub-expression of a definition, whether it is in the definition's own scope or in a type-params sub-scope.

Related to both #13693 and #14161.

---

_Label `red-knot` added by @carljm on 2024-11-13 00:47_

---

_Review requested from @MichaReiser by @carljm on 2024-11-13 00:47_

---

_Review requested from @AlexWaygood by @carljm on 2024-11-13 00:47_

---

_Review requested from @sharkdp by @carljm on 2024-11-13 00:47_

---

_Comment by @github-actions[bot] on 2024-11-13 01:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-11-13 07:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2509 on 2024-11-13 11:32_

```suggestion
        let metaclass_ty = definition_expression_ty(db, class_definition, metaclass_node);
        Some(metaclass_ty)
```

---

_@AlexWaygood approved on 2024-11-13 11:32_

---

_Merged by @carljm on 2024-11-13 13:53_

---

_Closed by @carljm on 2024-11-13 13:53_

---

_Branch deleted on 2024-11-13 13:53_

---
