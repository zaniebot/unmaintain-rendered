```yaml
number: 12269
title: "[red-knot] per-definition inference, use-def maps"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/defs
created_at: 2024-07-10T06:55:21Z
updated_at: 2024-07-18T16:39:48Z
url: https://github.com/astral-sh/ruff/pull/12269
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] per-definition inference, use-def maps

---

_Pull request opened by @carljm on 2024-07-10 06:55_

Implements definition-level type inference, with basic control flow (only if statements and if expressions so far) in Salsa.

There are a couple key ideas here:

1) We can do type inference queries at any of three region granularities: an entire scope, a single definition, or a single expression. These are represented by the `InferenceRegion` enum, and the entry points are the salsa queries `infer_scope_types`, `infer_definition_types`, and `infer_expression_types`. Generally per-scope will be used for scopes that we are directly checking and per-definition will be used anytime we are looking up symbol types from another module/scope. Per-expression should be uncommon: used only for the RHS of an unpacking or multi-target assignment (to avoid re-inferring the RHS once per symbol defined in the assignment) and for test nodes in type narrowing (e.g. the `test` of an `If` node). All three queries return a `TypeInference` with a map of types for all definitions and expressions within their region. If you do e.g. scope-level inference, when it hits a definition, or an independently-inferable expression, it should use the relevant query (which may already be cached) to get all types within the smaller region. This avoids double-inferring smaller regions, even though larger regions encompass smaller ones.

2) Instead of building a control-flow graph and lazily traversing it to find definitions which reach a use of a name (which is O(n^2) in the worst case), instead semantic indexing builds a use-def map, where every use of a name knows which definitions can reach that use. We also no longer track all definitions of a symbol in the symbol itself; instead the use-def map also records which defs remain visible at the end of the scope, and considers these the publicly-visible definitions of the symbol (see below).

Major items left as TODOs in this PR, to be done in follow-up PRs:

1) Free/global references aren't supported yet (only lookup based on definitions in current scope), which means the override-check example doesn't currently work. This is the first thing I'll fix as follow-up to this PR.

2) Control flow outside of if statements and expressions.

3) Type narrowing.

There are also some smaller relevant changes here:

1) Eliminate `Option` in the return type of member lookups; instead always return `Type::Unbound` for a name we can't find. Also use `Type::Unbound` for modules we can't resolve (not 100% sure about this one yet.)

2) Eliminate the use of the terms "public" and "root" to refer to module-global scope or symbols. Instead consistently use the term "module-global". It's longer, but it's the clearest, and the most consistent with typical Python terminology. In particular I don't like "public" for this use because it has other implications around author intent (is an underscore-prefixed module-global symbol "public"?). And "root" is just not commonly used for this in Python.

3) Eliminate the `PublicSymbol` Salsa ingredient. Many non-module-global symbols can also be seen from other scopes (e.g. by a free var in a nested scope, or by class attribute access), and thus need to have a "public type" (that is, the type not as seen from a particular use in the control flow of the same scope, but the type as seen from some other scope.) So all symbols need to have a "public type" (here I want to keep the use of the term "public", unless someone has a better term to suggest -- since it's "public type of a symbol" and not "public symbol" the confusion with e.g. initial underscores is less of an issue.) At least initially, I would like to try not having special handling for module-global symbols vs other symbols.

4) Switch to using "definitions that reach end of scope" rather than "all definitions" in determining the public type of a symbol. I'm convinced that in general this is the right way to go. We may want to refine this further in future for some free-variable cases, but it can be changed purely by making changes to the building of the use-def map (the `public_definitions` index in it), without affecting any other code. One consequence of combining this with no control-flow support (just last-definition-wins) is that some inference tests now give more wrong-looking results; I left TODO comments on these tests to fix them when control flow is added.

And some potential areas for consideration in the future:

1) Should `symbol_ty` be a Salsa query? This would require making all symbols a Salsa ingredient, and tracking even more dependencies. But it would save some repeated reconstruction of unions, for symbols with multiple public definitions. For now I'm not making it a query, but open to changing this in future with actual perf evidence that it's better.

---

_Label `red-knot` added by @carljm on 2024-07-10 06:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:32 on 2024-07-10 07:33_

Would it make sense to instead have an `IndexVec` from `ScopedExpressionId` to `ScopedUseId` or are we using a map because only very few expression ids have `ScoipedUseId`? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:22 on 2024-07-10 07:36_

I think we also want the `ScopedExpressionId` as part of the `id`. Otherwise all expressions from the same scope have the same id.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:85 on 2024-07-10 07:37_

Can we use `derive(PartialEq, Eq)`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:22 on 2024-07-10 07:38_

```suggestion
    let use_def = use_def_map(db, scope);
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:30 on 2024-07-10 07:39_

```suggestion
   let first = def_types.next().unwrap_or(Type::Unbound);
```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:15 on 2024-07-10 07:40_

Is idea here that we don't cache the public symbol type anymore because we already cache the definition types?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:474 on 2024-07-10 07:42_

Pfff... :P

---

_@MichaReiser reviewed on 2024-07-10 07:42_

I don't fully understand all of it yet, but I like what I'm seeing. 

---

_@carljm reviewed on 2024-07-10 07:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:32 on 2024-07-10 07:45_

Yes, I did it this way because it's quite sparse; basically only `Name` expressions are uses.

---

_@carljm reviewed on 2024-07-10 07:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:85 on 2024-07-10 07:46_

Oh yeah, I took this from the symbol table, but there we manually implement PartialEq because we want to exclude a field from it. Will fix.

---

_@carljm reviewed on 2024-07-10 07:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:22 on 2024-07-10 07:57_

Hmm. Is this also a problem with our current representation of `Definition`? https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/semantic_index/definition.rs#L12-L21

There can be multiple definitions of the same symbol in the same scope, but the id of definition contains only file, scope, and symbol.

I'm a little fuzzy on the implications of creating multiple tracked structs that share an id, or whether we should really be using id in these structs at all. The description of it at https://salsa-rs.github.io/salsa/overview.html#id-fields is all about tracking identity across re-orderings, but I think any reordering of Definitions or Expressions in a scope is going to change symbol IDs and expression IDs anyway, so I'm not sure if there is any useful "identity" that we are tracking here at all.

---

_@carljm reviewed on 2024-07-10 07:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:30 on 2024-07-10 07:59_

Is this better even though it means we'll make an entirely unnecessary second call to `def_types.next()` that we already know will return `None`? It seems like we may as well short-circuit here and just return Unbound directly.

---

_@MichaReiser reviewed on 2024-07-10 08:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:22 on 2024-07-10 08:05_

It depends on the query that creates the structure. `Definitions` are created as part of the `semantic_index` query. Adding or removing any definition would offset all definitions by one. The worst-case outcome is that all queries taking a definition as an argument now return a different result (worst-case invalidation). 

This is mitigated by including the `scope` and `symbol` in the ID. The scope ensures that adding a definition to an outer (or even inner) scope doesn't change the IDs of the current scope. Whether we need `symbol_id` and `file_id` is unclear to me. We could probably do without them. 

The fact that `symbol_id` isn't unique doesn't matter. If two structs have the same ID, Salsa relies on order (at least that's what I understand). 

I would need to have a closer look at `Expression` to understand what makes sense here. So yes, maybe scope itself is sufficient, because we then only depend on the order of expressions per scope. But we might be able to do better by including an additional id so that adding/removing expressions doesn't invalidate all expressions in a scope

---

_@carljm reviewed on 2024-07-10 08:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:15 on 2024-07-10 08:06_

Yeah... the thing is that all symbols (not just root-scope symbols) have a "public type" (i.e. the type as seen from another scope) -- for non-root-scope symbols they can be seen by a nested scope, for class-scope symbols they can also be seen by other scopes via class attribute access -- and I want to handle all of them the same way. And I'm not sure we want every symbol to be an ingredient.

But this will be fairly easy to change in the future, I think; it can be an empirical question whether performance is better if we make all symbols an ingredient and cache public type of all symbols, or don't make them an ingredient and sometimes re-create the same union.

---

_@carljm reviewed on 2024-07-11 01:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:22 on 2024-07-11 01:48_

Makes sense. The thing I don't understand about adding a `ScopedExpressionId` as part of the id here is that, given how we number expressions in-order within a scope, adding/removing expressions is going to result in changing the `ScopedExpressionId` of all the subsequent expressions anyway, right? So it's not clear to me how matching up by `ScopedExpressionId` is actually better (within a scope) than matching up by order of expressions; it seems like they are basically equivalent. In fact it seems like not having the `ScopedExpressionId` might be better, since only a few expressions are interned; without the `ScopedExpressionId` adding/removing non-interned expressions won't invalidate, but if we included the `ScopedExpressionId` then it would invalidate.

---

_Comment by @github-actions[bot] on 2024-07-11 20:23_

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

_Comment by @codspeed-hq[bot] on 2024-07-12 05:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/defs)

### Merging #12269 will **improve performances by 6.95%**

<sub>Comparing <code>cjm/defs</code> (c7e4ef9) with <code>main</code> (30cef67)</sub>



### Summary

`⚡ 2` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/defs` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[without_parse]` | 238.7 µs | 228.9 µs | +4.28% |
| ⚡ | `red_knot_check_file[incremental]` | 90.2 µs | 84.4 µs | +6.95% |


---

_Marked ready for review by @carljm on 2024-07-12 06:22_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-12 06:22_

---

_Comment by @carljm on 2024-07-12 06:24_

This is ready for review. I've updated the PR description with a fuller narrative of what's in here and why.

Benchmarks seem to say this PR has reclaimed most of the perf that was lost in the Salsa-type-interning PR, but I'm not sure why. It may be due to the removal of the ancestor-scope-walking name lookup code in favor of the use-def map.

---

_@carljm reviewed on 2024-07-12 06:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:15 on 2024-07-12 06:30_

Working on a change to use `Cow` here so that repeated uses with the same set of visible definitions require fewer allocations. But this can also land as a follow-up PR if I don't get it done before review of this PR.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:174 on 2024-07-12 12:20_

I know you just copied what I did, so this is really my mistake. Can't we use `self.use_map.len()` to compute the next id?


```suggestion
        let use_id = NextUseId::from_usize(self.uses_map.len());
        self.uses_map.insert(expr.into(), use_id);
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:97 on 2024-07-12 12:24_

Should we also store the `target_index` here to be consistent with `ImportFromDefinitionNode`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:6 on 2024-07-12 12:26_

I'm a bit surprised to find the `Expression` ingredient in this module. What's the motivation for placing it here rather than e.g. in its own module?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:38 on 2024-07-12 12:27_

Should we just store the relevant node here, considering that we only need a part of it?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:15 on 2024-07-12 12:29_

An alternative could be to have a separate `Vec` into which we write all these sub-vecs and instead just store a `Range` into that vec here (and uses that have the same definitions can just use the same range). But yes, I think it is worthwhile to spend some time investing if we can reduce the allocations (and clones)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:33 on 2024-07-12 12:32_

Without having looked at the code. Should `infer_region` consume `self` and directly return the result or should it/is it possible to infer multiple regions at once?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:35 on 2024-07-12 12:33_

One of the main concerns that you expressed is that it's now "complicated" to know which of the `infer_` methods to call. Could we add some documentation that gives guidance on which method to use? Like use `infer_expression_types` when. If you want x, use `infer_definition_types`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:595 on 2024-07-12 12:36_

Is this allow still necessary. It seems `self` is used in the method body

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:263 on 2024-07-12 12:42_

I don't fully understand how the different scopes are merged, or how we avoid performing the same type inference multiple times. 

Let's say we call type inferences on module. The builder runs over each statement, calls `infer_statement` and we ultimately arrive here, performing the inference. 

Now, some other code calls `infer_definition_ty(function_stmt)`. I would expect that this re-runs the type-inference for the entire function because the module-level type inference never called the salsa query. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:84 on 2024-07-12 12:45_

I don't think we can use an `IndexVec` here any longer because each region only stores a very small subset of the expressions. I'm not quiet sure why the tests are passing but I would expect that all expression ids are off. 

---

_@MichaReiser reviewed on 2024-07-12 12:46_

I like what I'm seeing here. 

My only concern is that I don't understand how the different type inference scopes are merged. See my inline comment. 

It will be interesting to see what the cost of storing the `definition -> Ty` and `expression -> Ty` hash maps on every level. That's a lot of hash maps and a lot of redundant information that we need to keep around.

---

_@carljm reviewed on 2024-07-12 15:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:97 on 2024-07-12 15:51_

The reason I didn't do that here is that assignments are more complicated than imports, when considering unpacking assignment as well as multiple targets. For example:

`(x, (y, z)) = a = foo()`

But I guess we could still number these targets left to right, ignoring the nesting (`x` would be 0, `y` would be 1, `z` would be 2, `a` would be 3), and as long as the visit order of semantic indexing is the same as the visit order of inference, that should work too; it just felt less clear.

I think it might be best to leave this alone for now and consider it as an option when we implement unpacking and multi-target assignment statements. I'll add a TODO comment.

---

_@carljm reviewed on 2024-07-12 15:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:6 on 2024-07-12 15:52_

Laziness :) I'll move it to its own module.

---

_@carljm reviewed on 2024-07-12 15:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:33 on 2024-07-12 15:54_

Yeah, I think you're right; `infer_region()` and `finish()` should be the same method. Will fix.

---

_@carljm reviewed on 2024-07-12 15:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:84 on 2024-07-12 15:57_

Yes, I think you're right. Tests are passing because at this stage tests are only actually exercising per-definition types; without type narrowing, nothing is currently using the stored per-expression types. I'll either add some unit tests or figure out what feature I can add to this PR that would actually exercise per-expression types in an integration test.

---

_@carljm reviewed on 2024-07-12 15:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:263 on 2024-07-12 15:58_

Yes, you're totally right. The call above to `self.infer_function_definition` needs to go through the query instead, and then we need to merge the types from that query result. I just forgot to do this, and then no tests caught it. I'll add a test that would catch it, and fix it.

---

_@carljm reviewed on 2024-07-12 16:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:38 on 2024-07-12 16:02_

I have a comment just above about this. In type inference we will need to recognize specific cases where we need to use the per-expression query, and so I thought it was clearer to explicitly store those specific cases, rather than generically store any expression.

Also, if I generically store any expression, then I'll need like a 15-variant enum here, for all the possible expression types. That seems more verbose and actually less clear about the "cases" we really deal with here.

I don't see a big downside to the current approach; following one box pointer doesn't seem like a big cost in the context of everything else?

---

_Comment by @carljm on 2024-07-12 16:07_

> It will be interesting to see what the cost of storing the definition -> Ty and expression -> Ty hash maps on every level. That's a lot of hash maps and a lot of redundant information that we need to keep around.

Yeah :/ That's the part of this approach that I'm least happy about.

We could reduce the number of hashmaps, and the redundancy, by using some kind of chaining (where an outer `TypeInference` will re-request the sub-query -- should be cached -- when needed to get sub-results instead of actually merging them into its own results), but that means more cost on lookup. I'm not sure how the tradeoffs will shake out.

I'll think more about this.

---

_Comment by @carljm on 2024-07-12 16:07_

Thanks for the great review!

---

_@MichaReiser reviewed on 2024-07-12 16:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:38 on 2024-07-12 16:12_

> I don't see a big downside to the current approach; following one box pointer doesn't seem like a big cost in the context of everything else?

No, it's not a big cost. It's just unclear to me how we avoid the 15 variants even if we hold on to the parent node, because you would still need different kinds to know which different sub-node to take. Anyway. I feel comfortable moving forward with this. It's just a design decision that I don't fully understand. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:38 on 2024-07-12 17:31_

Understood that this isn't a blocking issue, but following up anyway to see if we can clear up our common understanding of this design choice, in case I'm missing something:

> It's just unclear to me how we avoid the 15 variants even if we hold on to the parent node, because you would still need different kinds to know which different sub-node to take.

So the "15 variants" I'm talking about here is like one variant for `ExprName`, one for `ExprBinOp`, etc, for all the different Expr variants. Just because we don't have an `AstNodeRef` equivalent of `ExpressionRef`, as far as I know.

If we hold the parent, we don't need those 15 variants, we just need one variant for each possible type of parent node that requires an independently-inferable expression, which will be more like 3 or 4 variants: one for StmtAssign, and one for each kind of branch statement that can contain a test expression, so `If`, `While`... not sure if there are any others? For each of those variants, there's only one sub-expression we could possibly mean (the RHS `value` for StmtAssign, the `test` for the others). And we don't need to enumerate all possible expression types anywhere, since type inference already handles that match over Expr variants.

So this design is many fewer variants, and the variants we have are more semantically meaningful: rather than just being a bunch of boilerplate to handle all possible expression types, it actually enumerates each specific semantic context in which we need an `Expression` ingredient at all.

Let me know if that still isn't making sense, or where I'm missing something!

---

_@carljm reviewed on 2024-07-12 17:31_

---

_@MichaReiser reviewed on 2024-07-12 21:03_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:38 on 2024-07-12 21:03_

That makes sense. Thanks for explaining. I don't think the `Expr` limitation still applies to `AstNodeRef`.

An `AstNodeRef` can store a reference to an `Expr`. Ultimately, `AstNodeRef` allows you to reference any reference with the same lifetime (a reference that is reachable from any AST node) as the parsed tree. There's no `where` clause restricting the type. The only constraint is that the thing stored on `AstNodeRef` is part of the AST.

---

_@carljm reviewed on 2024-07-12 23:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:38 on 2024-07-12 23:21_

Oh! In that case I will look to simplify this; we may not need an enum here at all.

---

_@carljm reviewed on 2024-07-12 23:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:595 on 2024-07-12 23:32_

Would love to have the nightly feature `expect` instead of `allow` to force cleaning these up.

---

_Comment by @MichaReiser on 2024-07-13 07:22_

> We could reduce the number of hashmaps, and the redundancy, by using some kind of chaining (where an outer TypeInference will re-request the sub-query -- should be cached -- when needed to get sub-results instead of actually merging them into its own results), but that means more cost on lookup. I'm not sure how the tradeoffs will shake out.

Here's an idea that's not in the salsa spirit but upholds the important constraints that matter to salsa. 

* Add an `expression_types` field to `SemanticIndex` which is an `IndexVec<ScopeId, ExpressionTypes>`. Adding it to `SemanticIndex` causes it to get wiped every time the AST changes. 
* `ExpressionTypes` internally stores a `RwLock<IndexVec<ScopedExpressionId, Option<Ty>>>` (or `Mutex`, see below) and the `semantic_index` query either initialize it as empty, or with the right size, but all values set to `None`
* All type inference stores the inferred expression types in `ExpressionTypes` (and not locally).
* `ExpressionTypes` allows lookups and writes by `ScopedExpressionId` (which you can get from any expression by calling `scoped_ast_id()`). It uses internal mutability for writing. A `RwLock` does allow for race conditions where two threads both test if an expression has been inferred, before they start inferring that expression (and then writing it back). But I think this kind of race is okay. Both queries will infer the same type.

This approach would mitigate most concerns. The only extra cost is that we need to test for every expression if it has already been inferred. 

I'm unclear on how it would work with any fixpoint iteration. How would we know which expression types need to be invalidated? Or would we know if we run a fixpoint and could then forcefully re-infer all expression types?

Regarding the review: Please re-request review when you think I should have another look

---

_Review requested from @MichaReiser by @carljm on 2024-07-16 04:19_

---

_Comment by @carljm on 2024-07-16 04:27_

Ok, I think this is ready for review again.

The use-def map building is now better (uses a single vec of definitions and ranges, instead of vecs of vecs) but there are probably ways it could be still better; open to comments. The main complexity is that avoiding vecs-of-vecs means sticking to a single range for the "active" defs of a given symbol, which means we always have to keep all the active defs for a given symbol adjacent. Sometimes at control-flow merge points this requires copying the same definition IDs from earlier to later in the all-definitions vector.

I ended up adding basic control flow (just for if statements and expressions), because it makes a big difference in how the range-based use-def map building works.

> Here's an idea that's not in the salsa spirit but upholds the important constraints that matter to salsa. ...

That's a neat idea. I would like to hold off on it though; it's purely a performance optimization that shouldn't make a difference to the API for type inference. So I would rather leave it until we are at a point where we can evaluate the performance impact on real code. For now I think we can move forward with some duplicated stored types and keep things more salsa-native. It's just extra id -> id mappings; not a ton of wasted memory.

---

_Review comment by @MichaReiser on `Cargo.toml`:130 on 2024-07-16 06:50_

We have our own `textwrap` in `ruff_python_trivia::textwrap`. Using it avoids adding new dependencies.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:190 on 2024-07-16 06:52_

I think we only have expression ingredients for some nodes. Because of that, should the method return an `Option<Expression<'db>>`. If not, then we should document the cases in which this method panics.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:412 on 2024-07-16 06:54_

Nit: Can we have a more specific assertion than `len`. I always find `len` assertions hard to debug if the test fails. What entry was expected to be in there? Or I now get two, crap, which one is the incorrect entry

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:491 on 2024-07-16 06:57_

I kind of prefer this to be called `root` scope. It's a very common term whereas I never heard of module-global before. It's also unclear to me if the `module-global` scope would be a parent or child of the built-in scope? For `root_scope` it is clear, it's the top-level scope.



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:1 on 2024-07-16 07:02_

Nit: Rename the module to `use_def` to be consistent with the casing. We could even argue that it should be called `use_definitions` because you consistently use `Definition` elsewhere. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:11 on 2024-07-16 07:03_

You could use an `IndexVec` here too, so that `Definitions::definitions` is a `Range<DefinitionId>`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:47 on 2024-07-16 07:03_

```suggestion
            definitions: Range::default(),
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:63 on 2024-07-16 07:08_

```suggestion
    /// builder state: currently visible definitions for each symbol
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:81 on 2024-07-16 07:09_

Nit: I would prefer a consistent use of `def` or `definition`. 
```suggestion
    pub(super) fn record_definition(&mut self, symbol: ScopedSymbolId, definition: Definition<'db>) {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:108 on 2024-07-16 07:11_

When is it possible that `num_symbols` > self.definitions_by_symbol.len()`? Is `Definitions::default` really the right default in that case. 

You can use  `truncate` if it is guaranteed that `num_symbols < self.definitions_by_symbol.len` (and it would probably be worth adding an assertion)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:113 on 2024-07-16 07:12_

```suggestion
            let current = &mut self.definitions_by_symbol[symbol_id];
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:121 on 2024-07-16 07:14_

The parentheses here and in other branches are unnecessary
```suggestion
                current.definitions = current.definitions.start..to_merge.definitions.end;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:111 on 2024-07-16 07:15_

The encapsulation of this method is unclear to me. It seems that we're extending the ranges of `definitions` without adding new items to `all_definitions`. What's guaranteeing that we can later dereference the definitions without having them pushed to `all_definitions`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:100 on 2024-07-16 07:18_

I'm a bit worried about the size of this vector (contains all symbols of a scope) combined with the fact that we call snapshot for every branch. I don't think that's something we have to fix as part of this PR but I'm worried about the IO cost and we probably want something more granular (for example, instead of snapshoting, create a `Branch` that stores the "merged" symbols together with a local lookup table (HashMap)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:78 on 2024-07-16 07:24_

Not for this PR: But I wonder if we could have different backing storages depending on the region. Because the scope-level inference could still use an `IndexVec` (and it probably makes sense for it to do so because there are many expressions, to the point where hasing becomes expensive)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:253 on 2024-07-16 07:27_

I think we need some module level or struct level documentation explaining why there are two `infer_function_definition` methods. The difference in naming is so subtle and we better establish a convention or this is going to get very confusing. Ideally, these methods wouldn't be in the same builder, but I think that's difficult to accomplish. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:295 on 2024-07-16 07:29_

This seems to always be the same pattern. Convert node into definition, infer the definition types, extend. 

I think we can create a helper function for this

```
fn infer_definition(&self, node: impl Into<DefinitionNodeKey>) {
        let definition = self.index.definition(node);
        let result = infer_definition_types(self.db, definition);
        self.extend(result);
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:620 on 2024-07-16 07:32_

We should use the `index` directly here because using the `use_def_map` for better "isolation" doesn't give us anything because this query already depends on the AST and the semantic index.

Using the query here actually results in a negative performance impact because we now pay the cost of another concurrent hash map lookup

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:998 on 2024-07-16 07:35_

Do you intentionally use an extra indent for the inline code? 
```suggestion
            y = 1
            y = 2
            if flag:
                y = 3
            x = y
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:969 on 2024-07-16 07:36_

```suggestion
        db.write_file("src/a.py", "x = y = 1")?;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:762 on 2024-07-16 07:37_

I rather have this block next to the `TestDb`. It's otherwise unclear where the `write_dedented` method is coming from. The implementation isn't local to this test block, it's available in the entire crate (but is private)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1170 on 2024-07-16 07:39_

I would remove the assertion here. The fact that `x_ty_2` resolves to a different type guarantees that the query did run. 

---

_@MichaReiser approved on 2024-07-16 07:40_

The type inference part looks good to me.

I started (for 1.5h) reviewing the use-def implementation but I must admit that I don't have enough context information to efficiently review the changes and the data structures are non-trivial. I would have preferred if adding use-def and then refactoring to a more efficient data structure were its own PR with a summary explaining how the data structures work together and what considerations have been made. But it's a bit too late for that. So I'm just gonna skip this part. We can come back to it later or set up a session where you walk me through the code. 





---

_@AlexWaygood reviewed on 2024-07-16 07:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:491 on 2024-07-16 07:44_

I somewhat strongly prefer module-global. The topmost scope is always called the "module" scope or the "global" scope in a Python context; I'm not sure I've ever heard it referred to as the "root" scope.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:491 on 2024-07-16 07:49_

Root scope is a term commonly used in the static analysis literature. Maybe not in Python. Anyway, I think I prefer `module` or `global` if that's what's used in the Python ecosystem over using both terms together. Using both seems unnecessary complex.

---

_@MichaReiser reviewed on 2024-07-16 07:49_

---

_@MichaReiser reviewed on 2024-07-16 09:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:1 on 2024-07-16 09:45_

I would find some module level documentation useful. What's a use definition map? What kind of lookup operation does it support? How is the data structure model (what's a snapshot?) and what makes it a good data structure for this use case. 

Example:

https://github.com/astral-sh/ruff/blob/6a1e5555377457f4a7c799b72341684a58c0887b/crates/ruff_python_formatter/src/comments/map.rs#L8-L44

---

_@carljm reviewed on 2024-07-16 16:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:190 on 2024-07-16 16:01_

Yeah, good point. I can check how painful it is to return Option here, that's probably the best approach.

---

_@carljm reviewed on 2024-07-16 16:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:491 on 2024-07-16 16:04_

I think this discussion clarifies why "root scope" is a bad term for module-global scope, because it is not actually the root scope in terms of lookup order; builtins scope is the root scope in that sense (the last fallback for name lookups.) All module scopes are "children" of the builtins scope in that sense.

I'm OK with just using "module scope" for this, I think that's clear enough.

---

_@carljm reviewed on 2024-07-16 16:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:1 on 2024-07-16 16:07_

Yes, this is a good idea. As we discussed, I'll do this in a follow-up PR.

---

_@carljm reviewed on 2024-07-16 16:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:1 on 2024-07-16 16:09_

I agree with `use_def` for consistency. I would rather not go use `use_definitions` just because it's so long, I don't want to use that in variable names everywhere we use this, so for consistency it seems nicer to just stay with `use_def` everywhere we refer to it.

---

_@carljm reviewed on 2024-07-16 16:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:11 on 2024-07-16 16:11_

We can't use an `IndexVec` here because this has to list definitions in a specific order, and sometimes with the same definition listed more than once, in order to maintain that at any given point the active definitions for a symbol must all be adjacent in this vector. So it can't be just "all definitions ordered by definition ID". Hopefully this will be clearer in the follow-up PR with the module doc comment.

---

_@carljm reviewed on 2024-07-16 16:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:100 on 2024-07-16 16:14_

I'm definitely open to iterating for better efficiency here, if we can find better options. I don't think I understand exactly what alternative you are proposing, but I don't think we have to sort it out here. I think (given that it's all private implementation detail and doesn't change the external API) I would actually prefer to defer further optimization here until we have enough features that we can run on real-world larger code samples and get higher-confidence performance measurements, so we know that our improvements are actually improvements.

---

_@carljm reviewed on 2024-07-16 16:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:121 on 2024-07-16 16:16_

I personally find it harder to visually parse without the parens (given that we have single dots in the start and end expression, and then double dot in the middle, which is kind of visually subtle), but if this isn't common style, I don't mind removing the parens.

---

_@carljm reviewed on 2024-07-16 16:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:78 on 2024-07-16 16:18_

Yes, I thought about this as well. I think it is definitely possible, and we can experiment with it and see how much difference it makes.

---

_@carljm reviewed on 2024-07-16 16:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:253 on 2024-07-16 16:18_

Yes, I agree. I'll add such documentation in a follow-up PR.

---

_@carljm reviewed on 2024-07-16 16:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:108 on 2024-07-16 16:37_

(Feel free to either read and reply to comments here, or just wait for the follow up PR with module docs, whatever you prefer. I reply here first either way because it helps me to sort out what I need to write in the module docs.)

The purpose of this method is to reset the builder state to some previous snapshot. For instance, when we have `if a: body_code else: else_code`, after we visit `body_code` we have to reset the visible-definitions state to what it was before the `if`, before we visit `else_code`, since either `body_code` or `else_code` can run, but not both, so definitions in `body_code` can never be visible to `else_code`.

But we also have to make sure that every symbol we have seen (even in `body_code`) is included in `symbols_by_definition`, otherwise our symbol IDs in the IndexVec will get out of sync: we might try to record some new symbol in `else_code` with the wrong symbol ID, because we are missing the ones from `body_code`.

Like say we have this:

```
if flag:
    x = 1
else:
    y = 2
```

After visiting `x = 1` we need to visit the else clause with no visible definition for `x`. We reset the state to our snapshot from before the `if`, which doesn't even include `x` in `definitions_by_symbol` yet. But then when we visit `y = 2` we will insert it to the wrong slot in `definitions_by_symbol`, because we are missing `x` -- the symbol IDs don't line up correctly.

Because we only add symbols as we visit code, never remove them, the current `num_symbols` can never be less than the number of symbols in the earlier snapshot we are resetting to, only greater (e.g. in the above example `num_symbols` is 1 -- it includes `x` -- but the snapshot we are resetting to has zero symbols). So the only case that can occur here is that we increase the size of the snapshot (in this case from zero to one), and add a "no definitions" for any missing symbols (in this case `x`). So `Definitions::default()` is correct here, because it represents "no visible definitions" (an empty range, with `may_be_unbound: true`).

In the follow-up PR, in addition to the module doc comment, I'll add assertions and comments throughout this code.

---

_@carljm reviewed on 2024-07-16 16:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:111 on 2024-07-16 16:41_

`all_definitions` never shrinks or is mutated, it only grows. And `FlowSnapshot` can only come from a snapshot of some previous state (barring that it comes from some other builder/scope entirely, but this would be a bug regardless.) So both `state.definitions_by_symbol` and `self.definitions_by_symbol` must refer to indices that are already present in `self.all_definitions`.

---

_@carljm reviewed on 2024-07-16 17:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:190 on 2024-07-16 17:00_

After trying it out, I don't think Option makes sense here because this can only occur due to a bug in our code; the caller doesn't have any better choice than to panic if it gets `None` either. So I added a doc of when this will panic.

---

_@carljm reviewed on 2024-07-16 17:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:491 on 2024-07-16 17:08_

Hmm, now I remember why I went with `module_global` and not just `module` -- because we have cases like `module_global_symbol_ty`, and changing that to `module_symbol_ty` is confusing, because we have some symbols that represent an entire module (like `foo` in `import foo`), and "module symbol" sounds like it could be that, rather than "a module-scope symbol".

I think just "global" could be OK too, as long as we understand that in Python, "global" means "module global" (but in Python that is what it generally means.)

I might leave this change for a follow-up PR, because it's not feeling clear to me.

---

_Comment by @carljm on 2024-07-16 17:42_

Planning to merge this as-is once tests pass. All unresolved comments above will be addressed in follow-up PRs.

---

_Merged by @carljm on 2024-07-16 18:02_

---

_Closed by @carljm on 2024-07-16 18:02_

---

_Branch deleted on 2024-07-16 18:02_

---

_@carljm reviewed on 2024-07-17 00:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:412 on 2024-07-17 00:35_

https://github.com/astral-sh/ruff/pull/12355

---

_@carljm reviewed on 2024-07-17 03:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:253 on 2024-07-17 03:05_

https://github.com/astral-sh/ruff/pull/12356

---

_@carljm reviewed on 2024-07-17 06:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:111 on 2024-07-17 06:35_

(Oh, and in some of these cases we do add new items to `all_definitions`, using `extend_from_within`.)

---

_@carljm reviewed on 2024-07-17 06:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:108 on 2024-07-17 06:51_

Hopefully clearer with the docs in https://github.com/astral-sh/ruff/pull/12357

---

_@carljm reviewed on 2024-07-17 06:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:111 on 2024-07-17 06:51_

Hopefully clearer with the docs in https://github.com/astral-sh/ruff/pull/12357

---

_@carljm reviewed on 2024-07-17 06:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/usedef.rs`:1 on 2024-07-17 06:51_

https://github.com/astral-sh/ruff/pull/12357

---

_@carljm reviewed on 2024-07-18 16:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:491 on 2024-07-18 16:39_

https://github.com/astral-sh/ruff/pull/12374

---
