```yaml
number: 15871
title: Add convenience helper methods for AST nodes representing function parameters
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/annotation-method
created_at: 2025-02-01T15:25:17Z
updated_at: 2025-02-01T17:26:04Z
url: https://github.com/astral-sh/ruff/pull/15871
synced_at: 2026-01-10T19:57:22Z
```

# Add convenience helper methods for AST nodes representing function parameters

---

_Pull request opened by @AlexWaygood on 2025-02-01 15:25_

## Summary

Currently retrieving information from our AST nodes representing parameters can be annoyingly tedious/verbose:
- To obtain the annotation of a `ruff_python_ast::nodes::Parameter` variable `param`, you have to use `param.annotation.as_deref()` to get an `Option<&Expr>`
- To obtain the annotation of an `ast::ParameterWithDefault`, you have to use `param.parameter.annotation.as_deref()` to get an `Option<&Expr>`
- To obtain the name of an `ast::ParameterWithDefault`, you have to use `&param.parameter.name`
- To obtain the default of an `ast::ParameterWithDefault`, you have to use `parame.default.as_deref()`

This PR introduces several convenience methods on `ast::Parameter` and `ast::ParameterWithDefault` to make life more ergonomic:
- `Parameter::annotation()` and `ParameterWithDefault::annotation()`, both of which return `Option<&Expr>`
- `Parameter::name()` and `ParameterWithDefault::name()`, both of which return `&Identifier`
- `ParameterWithDefault::default()`, which returns `Option<&Expr>`

The addition of these methods allows us to write a lot of code more concisely and elegantly.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2025-02-01 15:25_

---

_@AlexWaygood reviewed on 2025-02-01 15:28_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3171 on 2025-02-01 15:28_

I considered implementing `Deref<Target = Parameter>` for `ParameterWithDefault` so that these methods would be implemented "for free", but decided against it. A `ParameterWithDefault` does more than simply wrap a `Parameter`: for example, if its `default` field is not `None`, it will have a different `range` value to the `range` of its inner `parameter` field.

---

_Comment by @github-actions[bot] on 2025-02-01 15:36_

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

_Marked ready for review by @AlexWaygood on 2025-02-01 15:37_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-01 15:37_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-01 15:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-01 15:37_

---

_@carljm approved on 2025-02-01 17:12_

These methods definitely make it nicer to work with parameters; in particular, de-referencing the Box around the `annotation` expr.

---

_Merged by @AlexWaygood on 2025-02-01 17:16_

---

_Closed by @AlexWaygood on 2025-02-01 17:16_

---

_Branch deleted on 2025-02-01 17:26_

---
