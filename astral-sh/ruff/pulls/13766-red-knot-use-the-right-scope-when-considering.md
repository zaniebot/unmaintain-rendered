```yaml
number: 13766
title: "[red-knot] Use the right scope when considering class bases"
type: pull_request
state: merged
author: rtpg
labels:
  - ty
assignees: []
merged: true
base: main
head: class-type-params
created_at: 2024-10-16T01:57:02Z
updated_at: 2024-10-17T22:38:44Z
url: https://github.com/astral-sh/ruff/pull/13766
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Use the right scope when considering class bases

---

_@rtpg_

Summary
---------

PEP 695 Generics introduce a scope inside a class statement's arguments and keywords.

```
class C[T](A[T]):  # the T in A[T] is not from the global scope but from a type-param-specfic scope
   ...
```

When doing inference on the class bases, we currently have been doing base class expression lookups in the global scope. Not an issue without generics (since a scope is only created when generics are present).

This change instead makes sure to stop the global scope inference from going into expressions within this sub-scope. Since there is a separate scope, `check_file` and friends will trigger inference on these expressions still.

Another change as a part of this is making sure that `ClassType` looks up its bases in the right scope. I do not believe the way I do the lookup in this change is the most precise way, and would appreciate comments on that fragment.

Test Plan
----------
`cargo test --package red_knot_python_semantic generics` will run the markdown test that previously would panic due to scope lookup issues

---

_Review requested from @carljm by @rtpg on 2024-10-16 01:57_

---

_Review requested from @MichaReiser by @rtpg on 2024-10-16 01:57_

---

_Review requested from @AlexWaygood by @rtpg on 2024-10-16 01:57_

---

_Renamed from "Use the right scope when considering class bases" to "[red-knot] Use the right scope when considering class bases" by @rtpg on 2024-10-16 01:57_

---

_@rtpg reviewed on 2024-10-16 01:59_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:41 on 2024-10-16 01:59_

This is very helpful because debug output on `ExpressionNodeKey` includes the range, which is usually enough in context to identify what text fragment is causing issues.

I put this in under the idea that this doesn't cost much but I might be wrong

---

_@rtpg reviewed on 2024-10-16 02:02_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 02:02_

It's unclear to me what the cost of the `semantic_index` lookup would be in practice. I just want to know what scope I need to be looking at. While `ClassType` does have `body_scope`, that is a separate scope from the type parameter scope.

Would it make sense to add `bases_scope` to `ClassType` here to avoid needing this index build up?

---

_@rtpg reviewed on 2024-10-16 02:04_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1442 on 2024-10-16 02:04_

the mapper is pulled out here because if we build up separate closures in different `if` branches, we hit the "no two closures are alike" issue. Hence the `type_scope_info` `Option` song and dance. 

---

_Comment by @github-actions[bot] on 2024-10-16 02:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-10-16 06:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:43 on 2024-10-16 06:41_

I like this. You could do

```suggestion
        let key = ;
        match self.expressions_map.get(&key.into()).unwrap_or_else(|| {
	        panic!("Could not find expression ID for {key:?}");
        }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 06:46_

I would expect that we could reuse the `self.file` and `self.index` because all the expressions in the class's base should be in the same file and from the same index. 

However, we probably don't want to call `infer_scope_types` because inferring the class's bases then suddenly becomes dependent of **all** types in that other scope, resulting in poor incremental caching. 

I'm not quiet sure but maybe `definition_expression_ty` is a better fit?  

---

_@MichaReiser reviewed on 2024-10-16 06:46_

---

_@carljm reviewed on 2024-10-16 12:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 12:56_

I don't think we can use `definition_expression_ty` because, in a type params scope, the class bases aren't "part of a definition" (the class definition is in the outer scope.)

We could mark the class bases as a "standalone expression" (or set of them) so we can query for their types directly, but I don't think this is worth the extra Salsa tracked structs. The "type params scope" is automatically generated and implicit and contains nothing but definitions for type parameters, and the class bases/keywords. There isn't "all types in that scope" to be worried about, because there are no other types in that scope. So I think `infer_scope_types` is the best option here.

---

_@carljm reviewed on 2024-10-16 13:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 13:02_

> I would expect that we could reuse the self.file and self.index

We aren't in `TypeInferenceBuilder` here, so those aren't available.

---

_@carljm reviewed on 2024-10-16 13:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 13:10_

I think perhaps the simpler way to get the type params scope is that it must always be the parent scope of the body scope (for a class with type params.) We should be able to add a `ScopeId::parent` method (it would need to use the symbol table, but not the semantic index.)

I'm also not opposed to adding `bases_scope` (or optional `type_params_scope`) to `ClassType`.

---

_@carljm reviewed on 2024-10-16 13:12_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1437 on 2024-10-16 13:12_

Is there a reason to use `has_type_params` and then `type_scope_info.unwrap()` rather than just `if let Some(...) = type_scope_info {` ? They rely on the same invariants, but the latter seems simpler.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:24 on 2024-10-16 13:13_

I would like to also add a test that in a stub file (`pyi`) we can have a generic class that refers to itself in its own bases. We have a similar test already for non-generic classes.

---

_@carljm requested changes on 2024-10-16 13:14_

This looks really good, thank you!! Comments aren't anything major, but there are a few things to address, so I'll request changes for now.

---

_@rtpg reviewed on 2024-10-16 13:16_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1437 on 2024-10-16 13:16_

Hmm... your suggestion is cleaner and this code isn't big, but I dislike the hiding of the "the branching is due to type parameter-ness".

I believe my concerns can be resolved with a more well thought out variable name for the scope info. Will ruminate on it overnight 

---

_@MichaReiser reviewed on 2024-10-16 13:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 13:24_

You can also use `index.scopes_by_node` when the query already depends on the index anyway (which `parent` would as well?)

---

_@carljm reviewed on 2024-10-16 15:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-16 15:47_

`ScopeId::parent` would rely on just the symbol table, which is part of the index, but in principle could be backdated independently if it didn't change? Unlikely though; I don't think depending on the index here is a problem.

---

_@rtpg reviewed on 2024-10-17 00:39_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/resources/mdtest/generics.md`:24 on 2024-10-17 00:39_

Added

---

_@rtpg reviewed on 2024-10-17 00:40_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1437 on 2024-10-17 00:40_

Changed this up to `bases_scope_info`. It's still an `Option` (instead of doing something like checking if `bases_scope` is equal to some other scope to determine specialized-ness), but good enough for me to feel comfortable with the cleaner `if let` solution.

---

_@rtpg reviewed on 2024-10-17 00:45_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1428 on 2024-10-17 00:45_

Ended up storing the bases scope in `ClassType` (which gets built up in the inference builder so we have access to the index right there). Definitely feels right after having written it up.

---

_@rtpg reviewed on 2024-10-17 00:46_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:43 on 2024-10-17 00:46_

Went with this, thanks for the suggestion!

---

_@rtpg reviewed on 2024-10-17 00:49_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:43 on 2024-10-17 00:49_

Great, went with that

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1433 on 2024-10-17 05:43_

Nit: We could consider to just call `base_expr.ty(...)` over repeating the entire ceremony of storing the base specialized scope, fingering the scope, and calling `expression_ty`. However, it would be slightly less efficient.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1439 on 2024-10-17 05:44_

Nit
```suggestion
        class_stmt_node.bases().iter().map(move |base_expr: &ast::Expr| {
            if let Some((bases_scope, inferences)) = bases_scope_info {
                // when we have a specialized scope, we'll look up the inference
                // within that scope
                inferences.expression_ty(base_expr.scoped_ast_id(db, bases_scope))
            } else {
                // Otherwise, we can do the lookup based on the definition scope
                definition_expression_ty(db, definition, base_expr)
            }
        })
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:880 on 2024-10-17 05:47_

Nit: I suggest making this a method on `ClassType`. It doesn't seem worth storing (it's very cheap to recompute). Unless we think that it helps with reducing the blast radius of file changes (e.g. so that calling `(class A).bases` where `class A` is defined in `foo.py`  from `bar.py` doesn't get invalidated when `foo.py changes). However, I do think that we might want to start caching `bases` once we add MRO resolution (which seems expensive)

---

_@MichaReiser approved on 2024-10-17 05:48_

---

_@rtpg reviewed on 2024-10-17 07:23_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types.rs`:1433 on 2024-10-17 07:23_

Going down that path let me remove all the base scope song and dance, at (by my read) not much cost. The resulting `if` statement is probably very hard to look at though now, since there's no longer quite a clear reason for _why_ I am calling `.ty` (even with the comment). But simpler is simpler, thanks for the suggestion

---

_@rtpg reviewed on 2024-10-17 10:51_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/infer.rs`:880 on 2024-10-17 10:51_

Thanks to your suggestion on using `ty` I don't even need this anymore, fortunately.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1419 on 2024-10-17 22:21_

We could create the `SemanticModel` only within the branch below where we'll actually need it.

---

_@carljm approved on 2024-10-17 22:21_

Looks great, thank you!

Since I have only one comment and it's minor, I'll push that change and go ahead and land this.

---

_Merged by @carljm on 2024-10-17 22:29_

---

_Closed by @carljm on 2024-10-17 22:29_

---
