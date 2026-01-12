```yaml
number: 13335
title: "[red-knot] use declared types in inference/checking"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/declared-types-infer
created_at: 2024-09-12T03:19:20Z
updated_at: 2024-09-17T15:11:08Z
url: https://github.com/astral-sh/ruff/pull/13335
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] use declared types in inference/checking

---

_@carljm_

Use declared types in inference and checking. This means several things:

* Imports prefer declarations over inference, when declarations are available.
* When we encounter a binding, we check that the bound value's inferred type is assignable to the live declarations of the bound symbol, if any.
* When we encounter a declaration, we check that the declared type is assignable from the inferred type of the symbol from previous bindings, if any.
* When we encounter a binding+declaration, we check that the inferred type of the bound value is assignable to the declared type.


---

_Label `red-knot` added by @carljm on 2024-09-12 03:19_

---

_Marked ready for review by @carljm on 2024-09-12 03:25_

---

_Review requested from @MichaReiser by @carljm on 2024-09-12 03:25_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-12 03:25_

---

_Comment by @carljm on 2024-09-12 03:27_

Pretty happy that the inference side of this doesn't regress performance at all, per CodSpeed! That's definitely thanks to @MichaReiser's suggestion to use just a single tracked struct and a single query for inferring definition types (whether binding or declaration). So we don't add too much Salsa overhead here, in terms of new tracked structs or queries.

---

_Comment by @github-actions[bot] on 2024-09-12 03:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4795 on 2024-09-12 10:04_

I'm not sure this should be changing as part of this PR. It's a class definition, and it's still only defined in the `sys.version_info >= (3, 11)` branch in typeshed: https://github.com/python/typeshed/blob/97ce20034352caff50d818e4b32cc28338a08d19/stdlib/builtins.pyi#L2025-L2032. Since we don't support `sys.version_info` branches yet, we should still be inferring that it could be `Unbound` if the implicit `else` branch is taken (i.e., `sys.version_info < (3, 11)`). And the publicly visible type of the `Unbound` variable from the perspective of other modules is `Unknown`.

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:26 on 2024-09-12 10:04_

🎉

---

_@AlexWaygood reviewed on 2024-09-12 10:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_model.rs`:163 on 2024-09-12 18:06_

This is interesting. What made you decide for using the inferred type over the declared type? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:166 on 2024-09-12 18:08_

Can we make this function private or is this a common operaiton when doing type inference?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:178 on 2024-09-12 18:12_

I'm surprised that adding an extra hash map doesn't lead to a more significant perf regeression. Lucky us. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:496 on 2024-09-12 18:14_

Nit: Maybe just use the term `Value` instead of `Object` because not all users know that all python values inherit from object.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:785 on 2024-09-12 18:20_

Should both of these be `annotated_ty` or should one be `Type::Unknown` because it is the inferred default value type?

---

_@MichaReiser approved on 2024-09-12 18:22_

This is great!

---

_@AlexWaygood reviewed on 2024-09-12 18:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:496 on 2024-09-12 18:25_

I prefer "object", because it's consistent with the terminology users see all the time in Python's tracebacks at runtime. I don't see it as referring to the fact that all Python values inherit from `builtins.object`; I think it's referring to the more fundamental semantic that all values in Python are, conceptually, objects

---

_@carljm reviewed on 2024-09-13 02:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_model.rs`:163 on 2024-09-13 02:47_

Hmm, good question. It wasn't really a thought-through decision, I just didn't have any intent to make a change to this part of the code at all -- I was just updating the wording to clarify what it actually does.

I think the main reason to use inferred type here is that all of the definitions we currently implement this for always have an inferred type, and for the ones that have a declared type (functions, classes, parameters), it's always identical to the inferred type anyway. So using declared type here would never give a different result, except in the cases where it would give no result at all.

The only node which can have a different declared vs inferred type is AnnAssign, and we don't implement `HasTy` for it, partly for that very reason.

---

_@carljm reviewed on 2024-09-13 02:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:166 on 2024-09-13 02:49_

No, I doubt we'll need it anywhere else. I can probably just inline and eliminate it; it only has one callsite. I only kept it for parallel with `bindings_ty`, but I don't think that's necessary.

---

_@carljm reviewed on 2024-09-13 02:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:785 on 2024-09-13 02:56_

The default value type doesn't have any influence on either the declared or the inferred type for a parameter. Consider this function:

```
def f(x: int = 1): ...
```

The declared type for `x` is `int`. The inferred type for the default parameter is `Literal[1]`. But that does not mean that the initial inferred type for `x` in the function's internal scope can be `Literal[1]`! The default value isn't the only possible bound value; callers are allowed to pass any value of type `int` for `x`. So both the declared and the inferred type for `x` is `int`. The only thing we will do with the default value (we don't do it yet) is verify that `Literal[1]` is assignable to `int`, and emit a diagnostic if not.

---

_@carljm reviewed on 2024-09-13 03:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4795 on 2024-09-13 03:05_

When we import something which has type declarations (and remember, function and class definitions are also declarations), we don't do type inference or look at bindings at all, we just look at the declared type(s).

If a declaration is present in one path and not in another, the current behavior is that we don't care about that; we just union the live declarations, and we never include Unbound. Unbound is a statement about runtime bindings, not a statement about declarations. I guess we could have an equivalent possibly-undeclared type (or piece of metadata in the use-def map), but I'm not sure what we would do with this, other than maybe emit some kind of diagnostic "you are importing something that is not declared in all paths," or similar. I still think we'd want to infer the imported type the way we do now -- I'm not sure that it's an improvement to union the imported type with `Unknown` just because the name wasn't declared in all paths.

---

_@carljm reviewed on 2024-09-13 03:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4795 on 2024-09-13 03:39_

Open to considering proposed alternative behaviors; just making sure we're clear on what is currently implemented and why. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4795 on 2024-09-13 19:46_

We discussed, and settled on a few modifications to the semantics, which I've now implemented in this PR, and I describe in another comment below. Thanks for prompting a re-think here!


---

_@carljm reviewed on 2024-09-13 19:46_

---

_Comment by @carljm on 2024-09-13 19:50_

I pushed some updates to the semantics implemented here:
* We now infer `Unknown` as part of the declared type of a symbol that isn't declared in all branches.
* We emit a conflicting-declarations diagnostic (if you attempt to assign to a name that has conflicting live declarations) even if one of the "conflicting declarations" is Unknown; this highlights subtle cases where a conditional declaration leaves the variable effectively undeclared at top level (in that anything can be assigned to it.)
* Even if declarations conflict, we still validate the assignment against the union of the declared types.

This gives declared types similar semantics to inferred types (multiple declarations in different paths are unioned), and means we always maintain the invariant `inferred_type <: declared_type`. We also emit a diagnostic on conflicting declarations, to avoid confusion, but this diagnostic could be disabled (if some users prefer it) without impacting soundness.

---

_Review requested from @MichaReiser by @carljm on 2024-09-13 23:28_

---

_Comment by @carljm on 2024-09-13 23:29_

Re-requesting review since this changed significantly; you can probably just review the changes in the last commit, or last two commits.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:440 on 2024-09-16 10:04_

Are these methods used outside this file? If not, can we make them private?

I generally prefer standalone functions or newtype wrapper around adding test specific methods to a type. They polute the type's namespace (I e.g. don't know if rust analyzer filters the methods out when outside a test). 



---

_@MichaReiser reviewed on 2024-09-16 10:04_

---

_@MichaReiser reviewed on 2024-09-16 10:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:348 on 2024-09-16 10:07_

Can't this be
```suggestion
            may_be_undeclared: declarations.maybe_undeclared,
```

You may need to flip the order of `inner` and `may_be_undeclared` for the borrow checker.

---

_@MichaReiser reviewed on 2024-09-16 10:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:271 on 2024-09-16 10:10_

I'm not sure how many more times we should write this before introducing a helper similar to `f.debug_list`.

* Create a `FormatterJoinExtension` trait with a single method `join(&mut self, separator: &str) -> Join`
* Create a `Join` struct with:
  * `entry(&mut self, item: &dyn std::fmt::Display)`
  * `entries<I, F>(&mut self, items: I) where I: IntoIterator<Item = F>, F: std::fmt::Display`
  * `finish(self) -> Result<()>`

It can be then used like this

```rust
f.join(", ") 
    .entries(self.types.0.iter())
    .finish()
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:510 on 2024-09-16 10:11_

Delete?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:128 on 2024-09-16 10:13_

Can this function be private?
```suggestion
fn bindings_ty<'db>(
```


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:177 on 2024-09-16 10:14_

What now, may or must?

```suggestion
/// error, as any symbol with zero live declarations clearly must be undeclared.
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:208 on 2024-09-16 10:17_

I guess it's kind of difficult with the current builder API but isn't `!first.is_equivalent_to` the same as testing if the item was added to the builder? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:209 on 2024-09-16 10:18_


```suggestion
            TypeList([].into())
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:209 on 2024-09-16 10:20_

I would have to do some more research but I think an empty `Box<[]>` always allocates (unlike an empty Vec). We may want to use an `Option<Box<[]>>` for `conflicting` for that reason.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:223 on 2024-09-16 10:21_

Nit: The declaration of these types is somewhat out of order considering that all other type declarations come after `Type`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:222 on 2024-09-16 10:23_

We can implement `Display` for `Box<[Type]>` if the `Display` type is the only reason for defining `TypeList`. 

Should `TupleType` and `FunctionType::decorators` use `TypeList`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:209 on 2024-09-16 10:26_

An other alternative is that this function returns a `Result<Type, (Type, Box<[Type])>>` which woudl also force callers to explicitly handle the error case. 

---

_@MichaReiser approved on 2024-09-16 10:26_

---

_@carljm reviewed on 2024-09-16 15:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:128 on 2024-09-16 15:04_

Yes, and it looks like the same is true for a number of other functions in here. I've made all the ones private that are only used in inference.

---

_@carljm reviewed on 2024-09-16 15:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:208 on 2024-09-16 15:52_

Right now the builder doesn't do any de-duplication. We could easily add de-duplication based on equivalence, but that would just be a temporary placeholder, as really it should be based on subtyping, e.g. if `class A(B): ...` then adding `A` to a union that has `B` in it already should not add anything to the union. But `A` and `B` are still conflicting type declarations. So ultimately this check should not be the same as union de-duplication.

---

_@carljm reviewed on 2024-09-16 16:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:348 on 2024-09-16 16:07_

Good call! Nah, borrow checker has no issues here, this was just a result of rewriting the code too many times :) The original version didn't have `may_be_undeclared` stored on the `DeclarationsIterator` at all.

---

_@MichaReiser reviewed on 2024-09-16 16:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:208 on 2024-09-16 16:12_

The other alternative is to use `itertools::join`. But that has the downside that it allocates a string (but that's probably fine here)

---

_@carljm reviewed on 2024-09-16 16:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:209 on 2024-09-16 16:21_

Yeah, verified that `Box<[]>` still allocates, so I'll rework this to use `Option`.

Not sure about using `Result` -- it feels odd to me to duplicate the actual return value in both variants, and it's also not clear that all callers _should_ be forced to explicitly handle the error case. In the case of resolving an import (`symbol_ty_by_id`) we want to ignore conflicting declarations and just use the unioned result.


---

_@carljm reviewed on 2024-09-17 00:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:271 on 2024-09-17 00:43_

I added this, and converted everywhere we had the "join" pattern in `types/display.rs` to use it.

---

_@carljm reviewed on 2024-09-17 00:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:208 on 2024-09-17 00:44_

> The other alternative is to use `itertools::join`. But that has the downside that it allocates a string (but that's probably fine here)

I think this comment was meant for a different thread? I went ahead and implemented `FormatterJoinExtension` trait rather than using `itertools::join`.

---

_@carljm reviewed on 2024-09-17 00:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:209 on 2024-09-17 00:44_

In the end I went with a `Result` -- even where we want to ignore conflicting types, it's probably better to be explicit about it.

---

_@carljm reviewed on 2024-09-17 00:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:223 on 2024-09-17 00:49_

I wanted to put them near `declarations_ty`, which is the only place they are used. But in the latest version this is reduced to just a type alias.

---

_@carljm reviewed on 2024-09-17 00:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:222 on 2024-09-17 00:51_

We can't implement `Display` for `Box<[Type]>` directly, because we need a `db`. But I added a `DisplayTypeArray` intermediate struct and a `TypeArrayDisplay` trait with a `display(&self, db) -> DisplayTypeArray` method, and then we can implement that trait for `Box<[Type]>`. I think it works out pretty well, looking forward to your review :)

---

_Comment by @carljm on 2024-09-17 00:54_

Updated again with a bunch of type-display cleanup, per comments.

Since I've again made a lot of changes in these revisions, and I'm working in very Rusty territory that isn't super familiar, I'm again re-requesting review before I land, to catch anything silly I've done. (Just the last three commits.)

---

_Review requested from @MichaReiser by @carljm on 2024-09-17 00:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:13 on 2024-09-17 08:16_

I would keep it `pub` as it is also useful when using `HasTy`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:103 on 2024-09-17 08:19_

Is it necessary that we use two lifetimes here considering that all our types in fact do have a `'db` lifetime. 

My preference is only to use as many lifetimes as necessary. Sometimes, it's necessary to use two to make the borrow checker happy (because it is important that some values have different lifetimes), but often, it isn't, and we can use a single lifetime.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:114 on 2024-09-17 08:19_

The fact that we use `'_` makes me think that two lifetimes are indeed unnecessary.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:120 on 2024-09-17 08:20_

I intentionally used an order map here to preserve the ordering. What's the reason for changing to an fx hash map (and rewriting the formatting here?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:142 on 2024-09-17 08:22_

Took me a while to understand why we need `DisplayUnionElement`. I would probably rewrite this to using a `for` loop and calling `f.entry(&ty)` or `f.entry(&DisplayLiteralGroup(literals, self.db))`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:235 on 2024-09-17 08:23_

Nit: I would prefer to keep a for loop here and call `f.entry(&ty)` in the for loop body. It avoids splitting the implementation across multiple types

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:335 on 2024-09-17 08:24_

I would move this into `ruff_db` and make it public. This seems generally useful

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:5394 on 2024-09-17 08:25_

Is it intentional that the types are no longer quoted?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:168 on 2024-09-17 08:26_

I thought you don't like `Result` :laughing: 

---

_@MichaReiser approved on 2024-09-17 08:27_

I should probably have been more explicit that refactoring `Display` wasn't something that we had to do as part of this PR. A separate PR for it would probably have been better (because I think there are other places where we now could use the new API). 

It overall looks good to me. I would have preferred a more imperative formatting in some places using `f.entry` because I think it leads to simpler code but I don't feel strongly about it.

The one change that I don't understand is switching from `OrderMap` to an `FxHashMap` in the union display implementation. The use of an ordered map was intentional to preserve the source order when displaying a union (as much as possible). Preserving the source order in types is something that I understood is important. If not, then we should reconsider whether we want to use `OrderMap` in unions and intersection and instead replace it with another map/set that implements `Hash`.

---

_Comment by @carljm on 2024-09-17 13:56_

> I think there are other places where we now could use the new API

I updated all the ones I could find in red-knot (in `types/display.rs`). Do you mean other places outside of red-knot?

Either way, if you point them out I'm happy to update them to use this, in a separate PR.

> The one change that I don't understand is switching from `OrderMap` to an `FxHashMap` in the union display implementation. The use of an ordered map was intentional to preserve the source order when displaying a union (as much as possible). Preserving the source order in types is something that I understood is important. If not, then we should reconsider whether we want to use `OrderMap` in unions and intersection and instead replace it with another map/set that implements `Hash`.

I removed the use of `OrderMap` because it didn't serve any purpose; it didn't change the ordering of the output in any way (note that no tests changed in their ordering of unions). The algorithm is 1) collect all literal types into a map of vecs keyed by literal kind, and then 2) iterate again through each individual element of the union, and display each entire literal group (removing it from the map, by key) whenever we hit the first element of that kind. We never iterate over the map. The ordering of elements within a literal group is maintained by the vec, and the ordering of literal groups within the union is determined by iteration through the individual elements.

So this change just simplifies the code (and I assume a `HashMap` is a bit more efficient than an `OrderMap`?) without changing the output of the algorithm at all.

---

_@carljm reviewed on 2024-09-17 13:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:168 on 2024-09-17 13:57_

I changed my mind -- I discussed this in a comment above.

---

_@carljm reviewed on 2024-09-17 13:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5394 on 2024-09-17 13:57_

Yeah, the more I looked at it the more it just seemed like visual noise.

---

_@carljm reviewed on 2024-09-17 14:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:120 on 2024-09-17 14:00_

The ordered properties of the OrderMap were not used at all in the algorithm here. We don't ever iterate over the map; we pop specific keys out of it based on when we run into the first literal of that kind in the overall union.

---

_@carljm reviewed on 2024-09-17 14:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:103 on 2024-09-17 14:25_

I think I ran into a different case where we were using `'db` lifetime for `self` where it was wrong and caused a problem, and then got over-aggressive about splitting up lifetimes 😆 I'll change this one back. I feel a bit mixed about it, because in principle the split feels more correct, but you're right that all of our type objects are always db lifetime anyway, so we really don't need to split here.

---

_@carljm reviewed on 2024-09-17 14:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:142 on 2024-09-17 14:57_

That makes sense. I think I kind of forgot about the `.entry` option when working on this, and assumed I needed to get things into an "iterator of items" shape. Oops.

I do like extracting the formatting of a literal group into its own type, so will probably keep that; it looks like that is your suggestion as well. Done and pushed.

---

_@carljm reviewed on 2024-09-17 15:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:235 on 2024-09-17 15:00_

Looks like this doesn't work here; since the `Join` holds a mutable reference to the formatter, you can't `f.write_str(...)` while doing a join, so there's no way to add the `~` for negated types. So we still need `DisplayMaybeNegatedType`, which means it's nicer to just use the iterator and `.entries()`.

I guess the conclusion between this and `DisplayUnionType` is that for loop and `.entry` is good when you're doing conditional stuff; that way you can avoid an extra enum. But it's still a requirement to use `Join` that all the actual writing is done within a per-entry `&dyn Display`; you can't do extra writes to the formatter while joining.

---

_Comment by @carljm on 2024-09-17 15:00_

Thanks for the great review, again! Will land this once signal is green.

---

_Merged by @carljm on 2024-09-17 15:11_

---

_Closed by @carljm on 2024-09-17 15:11_

---

_Branch deleted on 2024-09-17 15:11_

---
