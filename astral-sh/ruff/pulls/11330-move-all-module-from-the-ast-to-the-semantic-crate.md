```yaml
number: 11330
title: Move all module from the AST to the semantic crate
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: move-all-to-ruff-python-semantic
created_at: 2024-05-08T06:46:32Z
updated_at: 2024-05-08T09:07:03Z
url: https://github.com/astral-sh/ruff/pull/11330
synced_at: 2026-01-10T22:05:26Z
```

# Move all module from the AST to the semantic crate

---

_Pull request opened by @MichaReiser on 2024-05-08 06:46_


## Summary

This commit moves the `all` module from the `ruff_python_ast` crate to the `ruff_python_semantic` crate. The `all` module contains the `DunderAllName` struct and related functionality, which is more semantically related and thus fits better in the `ruff_python_semantic` crate.

The changes include updates to the import paths in the `ruff_linter` and `ruff_python_semantic` crates to reflect the new location of the `all` module. The `ruff_python_ast` crate no longer exports the `all` module.

This refactoring improves the organization of the codebase, making it easier to understand and maintain. It also ensures that each crate has a clear and distinct responsibility.

## Test Plan

`cargo test`


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-05-08 06:46_

---

_Label `internal` added by @MichaReiser on 2024-05-08 06:46_

---

_Comment by @github-actions[bot] on 2024-05-08 07:05_

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

_@AlexWaygood reviewed on 2024-05-08 08:04_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/all.rs`:127 on 2024-05-08 08:04_

Did you consider making this function a method on the `SemanticModel` struct? It would mean you could easily make use of the `semantic.resolve_builtin_symbol()` method. That would have two advantages:

1. We wouldn't have to pass in the `is_builtin` closure to this function, which feels slightly awkward if the function is now in the same crate as the `SemanticModel`
2. It's more accurate, as we would recognise `__all__ = builtins.list(["foo", "bar"])` as a valid `__all__` definition, as well as `__all__ = list(["foo", "bar"])`

```suggestion
                    if self
                        .resolve_builtin_symbol(map_subscript(func))
                        .is_some_and(|symbol| matches!(symbol, "tuple" | "list")) {
```

---

_@AlexWaygood reviewed on 2024-05-08 08:06_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/all.rs`:83 on 2024-05-08 08:06_

micro-nit: I personally prefer referring to all AST nodes by their fully qualified names outside the `ruff_python_ast` crate, as it means that you don't have to continually update the `use ruff_python_ast::{x, y, z, ...}` line at the top when you discover in a future refactor that you need access to more nodes.

---

_Comment by @AlexWaygood on 2024-05-08 08:06_

Thanks for doing this!

---

_@MichaReiser reviewed on 2024-05-08 08:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/all.rs`:127 on 2024-05-08 08:44_

I prefer not to make any code changes other than moving the module in this PR. Moving seemed a quick win to me to improve code separation but I don't want to spend any significant time on the `all` module otherwise.

---

_@AlexWaygood reviewed on 2024-05-08 08:49_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/all.rs`:127 on 2024-05-08 08:49_

Makes sense, I agree that this change by itself is an improvement! 

---

_@AlexWaygood approved on 2024-05-08 08:49_

---

_Merged by @MichaReiser on 2024-05-08 08:56_

---

_Closed by @MichaReiser on 2024-05-08 08:56_

---

_Branch deleted on 2024-05-08 08:56_

---
