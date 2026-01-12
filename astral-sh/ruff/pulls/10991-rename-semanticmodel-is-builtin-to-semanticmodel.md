```yaml
number: 10991
title: "Rename `SemanticModel::is_builtin` to `SemanticModel::has_builtin_binding`"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: rename-is-builtin
created_at: 2024-04-17T09:57:49Z
updated_at: 2024-04-18T10:11:45Z
url: https://github.com/astral-sh/ruff/pull/10991
synced_at: 2026-01-12T15:55:34Z
```

# Rename `SemanticModel::is_builtin` to `SemanticModel::has_builtin_binding`

---

_@AlexWaygood_

## Summary

`SemanticModel::is_builtin` returns `true` if a _symbol_ has a builtin binding in the current scope (i.e., the name is bound because it is part of the builtins scope that Python pre-populates without any imports or assignments specified by the user). This is distinct to `SemanticModel::resolve_builtin_symbol` and `SemanticModel::match_builtin_expr`, which can be used to determine whether an AST _node_ refers to a symbol from the `builtins` module.

This PR renames `SemanticModel::is_builtin` to `SemanticModel::has_builtin_binding`, to make the distinction between these different methods clearer

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-04-17 10:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-04-17 10:22_

> This is distinct to SemanticModel::resolve_builtin_symbol and SemanticModel::match_builtin_expr, which can be used to determine whether an AST node refers to a symbol from the builtins module.

I don't think I follow what the distinction is (and thus, why the new name is more explicit). Is the distinction that one resolves an expression and the other a name? 

---

_Comment by @AlexWaygood on 2024-04-17 10:32_

> I don't think I follow what the distinction is (and thus, why the new name is more explicit). Is the distinction that one resolves an expression and the other a name?

Partly, yes. I think the name implies currently that you could use it to test whether a node refers to a builtin symbol or not, but it's not the correct method to use for that purpose.

I also find calling it `is_builtin` confusing because it's _not_ checking whether a symbol is available in the `builtins` scope by default -- it's checking whether a symbol _has a builtin binding_ at the specific point when we're calling into the semantic model. I.e.

```py
...  # <-- If we're analysing this node, `semantic.is_builtin("print")` returns true

print = "foo"

...  # <-- but at this later point in the same module, it no longer does,
     # because `print` now has a local binding rather than a builtin binding
```

`print` is still a builtin at the point of the second call, in that it's still available in the builtin scope; you can still access it from Python code by importing `builtins` and accessing it via `builtins.print`. But it no longer has a builtin _binding_ in this scope; the builtin binding has been overridden by a local binding, which takes precedence when looking up the `print` name in that scope.

---

_@MichaReiser approved on 2024-04-17 10:34_

---

_Comment by @MichaReiser on 2024-04-17 10:35_

Thanks for the explanation. So for me, the importance of the renaming is really about making it clear that it isn't a function that tests if `name` maps to a builtin name, but tests whether name is bound to a builtin (right now)

---

_Merged by @AlexWaygood on 2024-04-18 10:11_

---

_Closed by @AlexWaygood on 2024-04-18 10:11_

---

_Branch deleted on 2024-04-18 10:11_

---
