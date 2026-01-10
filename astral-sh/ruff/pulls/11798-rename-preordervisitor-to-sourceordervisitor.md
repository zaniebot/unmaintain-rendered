```yaml
number: 11798
title: "Rename `PreorderVisitor` to `SourceOrderVisitor`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rename-preorder-visitor
created_at: 2024-06-07T15:35:07Z
updated_at: 2024-06-07T17:18:04Z
url: https://github.com/astral-sh/ruff/pull/11798
synced_at: 2026-01-10T21:56:00Z
```

# Rename `PreorderVisitor` to `SourceOrderVisitor`

---

_Pull request opened by @MichaReiser on 2024-06-07 15:35_

## Summary

There has been some confusion about the order in which `PreoderVisitor` visits the AST nodes because it is poorly named (I'm allowed to say this, I wrote it ;))

This PR renames the `PreorderVisitor` to `SourceOrderVisitor` and hints users in the documentation to to `Visitor` that does evaluation order visiting.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-06-07 15:35_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-07 15:35_

---

_Label `internal` added by @MichaReiser on 2024-06-07 15:35_

---

_Comment by @github-actions[bot] on 2024-06-07 15:56_

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

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/visitor/source_order.rs`:11 on 2024-06-07 16:03_

nit: I'd find this slightly clearer

```suggestion
/// If you need a visitor that visits the nodes in the order they're evaluated at runtime,
/// use [`Visitor`](super::Visitor) instead.
```

Would it also be worth renaming the other one to `EvaluationOrderVisitor`?

---

_@AlexWaygood approved on 2024-06-07 16:04_

Nice, this is much clearer IMO

---

_@carljm reviewed on 2024-06-07 16:18_

---

_Review comment by @carljm on `crates/ruff_python_ast/src/visitor/source_order.rs`:11 on 2024-06-07 16:18_

I don't think renaming the other one is really necessary; evaluation order is what I would expect by default (that's what CPython's AST visitor does, for example.)

---

_@carljm approved on 2024-06-07 16:19_

Looks great!

---

_@AlexWaygood reviewed on 2024-06-07 16:21_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/visitor/source_order.rs`:11 on 2024-06-07 16:21_

Right, I wondered if it would make distinguishing the two slightly clearer, but I agree it's what I'd expect by default

---

_Merged by @MichaReiser on 2024-06-07 17:01_

---

_Closed by @MichaReiser on 2024-06-07 17:01_

---

_Branch deleted on 2024-06-07 17:01_

---
