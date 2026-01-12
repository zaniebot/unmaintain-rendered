```yaml
number: 12920
title: "[red-knot] Add definition for with items"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/with-item-def
created_at: 2024-08-16T08:22:07Z
updated_at: 2024-08-22T02:30:22Z
url: https://github.com/astral-sh/ruff/pull/12920
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Add definition for with items

---

_@dhruvmanila_

## Summary

This PR adds symbols and definitions introduced by `with` statements.

The symbols and definitions are introduced for each with item. The type inference is updated to call the definition region type inference instead.

## Test Plan

Add test case to check for symbol table and definitions.

---

_Label `red-knot` added by @dhruvmanila on 2024-08-16 08:22_

---

_Comment by @github-actions[bot] on 2024-08-16 08:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-16 10:58_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-16 10:58_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-16 10:58_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-16 10:58_

---

_@MichaReiser reviewed on 2024-08-16 11:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:627 on 2024-08-16 11:41_

Nit: I prefer to use `expect`. It's shorter 
```suggestion
        let optional_vars = optional_vars.as_deref().expect("With item definition without optional vars");
        };
```

---

_@MichaReiser reviewed on 2024-08-16 11:41_

If this lands after https://github.com/astral-sh/ruff/pull/12919, then add `HasTy` implementation for `WithItem` and pull the type in the corpus test

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:631 on 2024-08-16 16:09_

Let's add a TODO for correct type inference here (but it's out of scope for this PR, since it requires features we don't have, like inferring types from a call).

The correct type for optional-vars here is the return type of the `__enter__` method on the type of `context_expr`.

This gets even a bit more complex when we consider unpacking, which I'll address in another comment.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:217 on 2024-08-16 16:12_

Unfortunately with-statements allow for unpacking assignment:

```
>>> class MyContext:
...     def __enter__(self):
...             return 1, 2
...     def __exit__(*a, **kw):
...             pass
...
>>> with MyContext() as (a, b):
...     pass
...
>>> a
1
>>> b
2
```

This means we need to store a bit more information in `DefinitionKind::WithItem`, like we do for `DefinitionKind::Assignment` -- each symbol in the unpacking expression gets its own `Definition`, and we will need to store both the overall `WithItem` node and the specific target `Name` node in order to handle this correctly in type inference. (We don't yet handle unpacking assignment for regular assignments in inference either, but you can use the way we handle them in creating Definitions as a model.)

Let's also add a semantic-index test for an unpacking assignment from a with-item, as in the above example.

---

_@carljm requested changes on 2024-08-16 16:13_

Looks good! Unfortunately unpacking throws in a wrinkle that we need to handle.

---

_Comment by @carljm on 2024-08-16 19:33_

> add `HasTy` implementation for `WithItem` and pull the type in the corpus test

I think we probably shouldn't implement `HasTy` for `WithItem`, because it is not an expression, and it is not clear what the type of it should be. Is it the type of the `context_expr`, or the type of the `optional_vars` (which will be derived from the return type of `__enter__` on the `context_expr`)? The `optional_vars` are the only part of `WithItem` where there's a definition, but if anything the type of `context_expr` would make more sense as the overall type of the `WithItem`. Where we have this kind of unclarity for a non-Expr nodeI think it is better not to implement `HasTy` at all, and require asking for the type of the specific part you want.

---

_@dhruvmanila reviewed on 2024-08-20 05:42_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:217 on 2024-08-20 05:42_

Make sense. Thanks for explaining. I've updated the code to reflect this change in https://github.com/astral-sh/ruff/pull/12920/commits/ba8bec693640c47c882e546d6aaf0ec33fe7c097 and added a test case for unpacking assignment in https://github.com/astral-sh/ruff/pull/12920/commits/aceee2e02e048222b56b5cc70664b8f215769b6c.

---

_Review requested from @carljm by @dhruvmanila on 2024-08-20 05:44_

---

_@carljm approved on 2024-08-21 23:04_

Looks great!

---

_Merged by @dhruvmanila on 2024-08-22 02:30_

---

_Closed by @dhruvmanila on 2024-08-22 02:30_

---

_Branch deleted on 2024-08-22 02:30_

---
