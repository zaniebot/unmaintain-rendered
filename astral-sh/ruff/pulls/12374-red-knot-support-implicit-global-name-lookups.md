```yaml
number: 12374
title: "[red-knot] support implicit global name lookups"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/scope-lookups
created_at: 2024-07-18T03:22:32Z
updated_at: 2024-07-18T17:50:46Z
url: https://github.com/astral-sh/ruff/pull/12374
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] support implicit global name lookups

---

_Pull request opened by @carljm on 2024-07-18 03:22_

Support falling back to a global name lookup if a name isn't defined in the local scope, in the cases where that is correct according to Python semantics.

In class scopes, a name lookup checks the local namespace first, and if the name isn't found there, looks it up in globals.

In function scopes (and type parameter scopes, which are function-like), if a name has any definitions in the local scope, it is a local, and accessing it when none of those definitions have executed yet just results in an `UnboundLocalError`, it does not fall back to a global. If the name does not have any definitions in the local scope, then it is an implicit global.

Public symbol type lookups never include such a fall back. For example, if a name is not defined in a class scope, it is not available as a member on that class, even if a name lookup within the class scope would have fallen back to a global lookup.

This PR makes the `@override` lint rule work again.

Not yet included/supported in this PR:

* Support for free variables / closures: a free symbol in a nested function-like scope referring to a symbol in an outer function-like scope.
* Support for `global` and `nonlocal` statements, which force a symbol to be treated as global or nonlocal even if it has definitions in the local scope.
* Module-global lookups should fall back to builtins if the name isn't found in the module scope.

I would like to expose nicer APIs for the various kinds of symbols (explicit global, implicit global, free, etc), but this will also wait for a later PR, when more kinds of symbols are supported.


---

_Label `red-knot` added by @carljm on 2024-07-18 03:22_

---

_Review requested from @MichaReiser by @carljm on 2024-07-18 03:22_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-18 03:22_

---

_Comment by @codspeed-hq[bot] on 2024-07-18 03:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/scope-lookups)

### Merging #12374 will **degrade performances by 10.44%**

<sub>Comparing <code>cjm/scope-lookups</code> (b2b4bd7) with <code>main</code> (811f78d)</sub>



### Summary

`❌ 3 (👁 3)` regressions
`✅ 30` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/scope-lookups` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[without_parse]` | 234.7 µs | 262.1 µs | -10.44% |
| 👁 | `red_knot_check_file[incremental]` | 89.2 µs | 95.5 µs | -6.58% |
| 👁 | `red_knot_check_file[cold]` | 318.8 µs | 345.4 µs | -7.7% |


---

_Comment by @github-actions[bot] on 2024-07-18 03:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-07-18 03:39_

As expected, this gives back the wins (and a bit more) that we saw previously when switching to per-definition inference and removing this lookup to other scopes. Not only are we now doing extra work on names that are global references (and actually also wrongly on names that are arguments, because we currently don't create definitions for arguments), but that means we actually now recognize the decorator and run the lint rule, so we're just doing a lot more work in the benchmark than before.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:111 on 2024-07-18 06:20_

I would not have expected `ClassTypeParameters` to be in here. A comment might be useful here.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:30 on 2024-07-18 06:21_

```suggestion
        use_def.public_may_be_unbound(symbol).then_some(Type::Unbound),
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:697 on 2024-07-18 06:25_

I think it would help readability if we move some declarations closer to the blocks that use them rather than defining all of them upfront. 


```rust
let file_scope_id = self.scope.file_scope_id(self.db);
let use_def = self.index.use_def_map(file_scope_id);
let use_id = name.scoped_use_id(self.db, self.scope);
let definitions = use_def.use_definitions(use_id);
let may_be_unbound = use_def.use_may_be_unbound(use_id);


let unbound_ty = if may_be_unbound {
		let symbols = self.index.symbol_table(file_scope_id);
    // SAFETY: the symbol table always creates a symbol for every Name node.
    let symbol = symbols.symbol_by_name(id).unwrap();

    if !symbol.is_defined() || !self.scope.is_function_like(self.db) {
        // implicit global
        Some(module_global_symbol_ty_by_name(self.db, self.file, id))
    } else {
        Some(Type::Unbound)
    }
} else {
    None
};

---

_@MichaReiser approved on 2024-07-18 06:27_

---

_@AlexWaygood reviewed on 2024-07-18 12:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:111 on 2024-07-18 12:08_

This is because the actual values of type parameters (whether they're type parameters for a class or type parameters for a function) are lazily evaluated (so that you can have forward references in them). The way this is implemented internally in Python is just by creating a function that is executed when you try to access the value for the first time. All of which is to say that although the user probably doesn't think of themselves as creating a function when they use type parameters (either for a class for for a function), the scoping rules for type parameter scopes are very analogous to function scopes. Similarly, we'll probably return `true` for this function in the future if `self.node(db)` is a comprehension or generator expression of some kind, once we support those.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:73 on 2024-07-18 12:15_

If you use markdown headings, VSCode is able to render some beautiful docs when I hover over the `definitions_ty` symbol in another file:

```suggestion
/// # Panics:
///
/// If called with zero definitions and no `unbound_ty`. This is a logic error,
```

![image](https://github.com/user-attachments/assets/ffa945ea-f277-46ad-b0dc-4131abf4e8ab)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:62 on 2024-07-18 12:15_

```suggestion
/// Infer the combined type of an array of [`Definition`]s, plus one optional "unbound type".
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:68 on 2024-07-18 12:15_

```suggestion
/// the given [`Definition`]s. If this isn't possible, then it will be `None`. If it is possible,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:684 on 2024-07-18 12:18_

```suggestion
                let symbol = symbols.symbol_by_name(id)
                    .expect("Expected the symbol table to always crate a symbol for every Name node");
```

You should also remove the `allow(unused)` suppression in `symbols.rs`:

https://github.com/astral-sh/ruff/blob/648cca199bbf0d2b4b04414404bca866fe5b52c9/crates/red_knot_python_semantic/src/semantic_index/symbol.rs#L195-L200

---

_@AlexWaygood reviewed on 2024-07-18 12:21_

---

_@carljm reviewed on 2024-07-18 16:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:111 on 2024-07-18 16:14_

Yes, these scopes behave like function scopes in terms of name resolution. See e.g. https://github.com/python/cpython/blob/main/Python/symtable.c#L545-L552 in CPython. Will add a comment.

---

_Merged by @carljm on 2024-07-18 17:50_

---

_Closed by @carljm on 2024-07-18 17:50_

---

_Branch deleted on 2024-07-18 17:50_

---
