```yaml
number: 20076
title: "[ty] Introduce a representation for the top/bottom materialization of an invariant generic"
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - ty
assignees: []
merged: true
base: main
head: materialization-relations
created_at: 2025-08-25T03:59:17Z
updated_at: 2025-08-28T00:53:58Z
url: https://github.com/astral-sh/ruff/pull/20076
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Introduce a representation for the top/bottom materialization of an invariant generic

---

_@JelleZijlstra_

Part of #994. This adds a new field to the Specialization struct to record when we're dealing with the top or bottom materialization of an invariant generic. It also implements subtyping and assignability for these objects.

Next planned steps after this is done are to implement other operations on top/bottom materializations; probably attribute access is an important one.

---

_Comment by @github-actions[bot] on 2025-08-25 04:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-25 04:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @JelleZijlstra on 2025-08-25 04:14_

The mypy-primer report suggests this breaks some aspect of overload resolution; [the code](https://github.com/optuna/optuna/blob/233a12906ed3632c12bdc30e732f209612945730/tests/samplers_tests/test_partial_fixed.py#L84) in optuna calls [suggest_categorical](https://github.com/optuna/optuna/blob/233a12906ed3632c12bdc30e732f209612945730/optuna/trial/_trial.py#L332) which is overloaded. It makes sense overloads would be affected since overload resolution already uses the top materialization. I'll have to look harder to see what's going wrong.

---

_Comment by @JelleZijlstra on 2025-08-25 04:17_

Oh maybe it's because ty still doesn't infer precise types for list displays?

---

_Comment by @JelleZijlstra on 2025-08-25 04:41_

The overload issue in mypy-primer can be reduced to this:

```python
from typing import overload, Sequence
from typing_extensions import reveal_type

@overload
def f(x: Sequence[None]) -> None: ...
@overload
def f(x: Sequence[int]) -> int: ...
def f(x: Sequence[None] | Sequence[int]) -> None | int: raise NotImplementedError

def caller() -> None:
    reveal_type(f([1, 2, 3]))
```

On my PR this outputs None; previously it was Unknown.

---

_Comment by @JelleZijlstra on 2025-08-25 04:44_

And that's because now `static_assert(is_assignable_to(Top[list[Any]], Sequence[None]))` succeeds, which is wrong. Probably something to do with how assignability interacts with looking at subclasses; I'll have to look into it tomorrow.

---

_Label `ty` added by @AlexWaygood on 2025-08-25 06:44_

---

_Marked ready for review by @JelleZijlstra on 2025-08-25 14:31_

---

_Review requested from @carljm by @JelleZijlstra on 2025-08-25 14:31_

---

_Review requested from @AlexWaygood by @JelleZijlstra on 2025-08-25 14:31_

---

_Review requested from @sharkdp by @JelleZijlstra on 2025-08-25 14:31_

---

_Review requested from @dcreager by @JelleZijlstra on 2025-08-25 14:31_

---

_Converted to draft by @JelleZijlstra on 2025-08-25 14:31_

---

_@JelleZijlstra reviewed on 2025-08-26 00:10_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-26 00:10_

This feels not great and I tried to implement it in `apply_specialization` instead, but couldn't get that to work for a reason I don't fully understand.

---

_Marked ready for review by @JelleZijlstra on 2025-08-26 02:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:210 on 2025-08-27 17:08_

naming nit: `Type` is already so overloaded in a type-checker codebase that we try pretty hard to avoid overloading it even more, so we avoid using it to mean generally "kind" or "variety" (not a representation of a type-system type). Maybe `MaterializationKind` here?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:521 on 2025-08-27 17:21_

Not convinced there's a TODO here, but I could be wrong? The invariance of a generic property instance with setter (if it is part of a generic class/protocol) derives from the use of its type variable in both covariant (return type of getter) and contravariant (argument type of setter) positions. Since in this case we materialize both the getter and the setter independently, we will get the right (structural) result from either materialization here: e.g. a property that returns `object` but accepts `Never`, or vice versa.

(I don't think we should ever materialize a property instance with un-substituted free typevars from a containing generic protocol; we should always apply a specialization first. This part may be a TODO?)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:520 on 2025-08-27 17:23_

Similarly suggest `materialization_kind` over `materialization_type` throughout

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:893 on 2025-08-27 17:29_

👍 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:515 on 2025-08-27 17:33_

Why do we need to store this separately in the `DisplayGenericAlias` when it's available from the `specialization` itself, which we already have? (And the `DisplayGenericAlias` also has access to the db.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:411 on 2025-08-27 17:39_

This is a field of a `Specialization`, so clearly it applies to a `Specialization`; prefixing the field name with `specialization_` thus seems redundant. (I think clippy has a warning for this, not sure under what circumstances it chooses to fire.)

I would find `materialization_kind` or just `materialization` to be a clearer name here. It is only set if this specialization represents a materialized generic type.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:693 on 2025-08-27 17:43_

This does look a bit involved. I also suspect this method might go away in the new constraint solver @dcreager is working on, so probably a good choice to defer considering it if we can.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6671 on 2025-08-27 17:46_

Related to another naming comment: `get_materialization_kind`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:641 on 2025-08-27 18:03_

It's a bit obscured by the current choice of match patterns, but I think the current form of this match simplifies to "if there is an existing materialization, always prefer it" (makes sense, an existing materialization is fully static already, new materialization can't affect it), "otherwise, use new materialization if any."

All mdtests pass if we replace with this:

```py
        let new_specialization_type = self.specialization_type(db).or(applied_specialization_type);
```

Notably, the `has_invariant_dynamic_typevar` variable is not actually relevant to the currently implemented logic.

That said, I think the currently-implemented logic could be improved; it can result in us keeping around a materialization that is unnecessary, because all typevars have been successfully materialized. That would suggest this instead:

```suggestion
        let new_specialization_type = if has_dynamic_invariant_typevar { 
            self.specialization_type(db).or(applied_specialization_type)
        } else {
            None
        };
```

Which also passes all tests. Not sure if the difference between this and the above can be detected in a test or not.

I think there's some reason to consider whether `Specialization` should have a `Vec<Option<MaterializationKind>>` (one per typevar) rather than an `Option<MaterializationKind>`. It would simplify this logic somewhat (each typevar would retain its `MaterializationKind` only if needed, rather than "retain the materialization if any typevar needs it"). But it may not be worth the extra memory used. Maybe depends whether it would also simplify other handling (e.g. for subtyping). (ETA: after looking at the subtyping logic, I think tracking materialization-kind per-typevar might optimize subtype/assignability checking, because it would reduce the cases where we have to re-materialize already-materialized types in a specialization, just because some _other_ typevar in the same specialization is dynamic-and-invariant. Whether this is in practice a good tradeoff for the extra memory is not clear. Something to explore in a future PR, I think.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-27 18:33_

I suspect the reason it didn't work is because `apply_specialization` is not called recursively, only `apply_type_mapping_impl` is? I think this would be difficult to change, so I suspect this is the way it needs to be done.

Maybe this would feel clearer if instead there were a top-level conditional here "are we applying a specialization?" (or it could be "are we applying a specialization with a materialization?" which I think is less conceptually clear but practically equivalent), and then bifurcate into the previous simple logic (if not) or the more complex logic accounting for specializations if so. (The latter could even be extracted into its own method.) This would duplicate the core "map over types" -- not really sure if it would feel like an improvement without trying it.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:486 on 2025-08-27 18:37_

Should this doc comment mention the "in invariant position" part?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:540 on 2025-08-27 18:39_

Neat implementation!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:449 on 2025-08-27 18:44_

```suggestion
        // `Derived` is a subtype of `Base` if the range of materializations covered by `Derived`
        // is a subset of the range covered by `Base`.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:451 on 2025-08-27 18:44_

These two lines also seem out of place here; there's no handling for `None` materialization anywhere in this routine.
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:447 on 2025-08-27 18:45_

This seems out of place in a routine that only handles subtyping?
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:476 on 2025-08-27 18:52_

The wording of this comment suggests that `derived_top.is_equivalent_to(db, base_bottom)` would be an adequate implementation here, but it's not. I think the issue with the wording is that the referents of "both" are not clear. It is not adequate that the "top materialization" and the "bottom materialization" previously mentioned are the same fully static type -- the original (unmaterialized) type must be fully static.
```suggestion
        // A top materialization is a subtype of a bottom materialization only if both original
        // un-materialized types are  the same fully static type.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:465 on 2025-08-27 19:01_

I think the referent of "it's" in the first phrase here is not clear. It is not the case that "the bottom materialization" mentioned previously must be "a subtype of some part of the range of materializations covered by top." (That would be a simpler implementation, which uses only `derived_bottom` and not `derived_top`.)

I think what is implemented here (and described after the "i.e.") is "some materialization of derived must be a subtype of some materialization of base."

Also, "top and derived of base" should be "top and bottom of base".

```suggestion
        // The bottom materialization of `Derived` is a subtype of the top materialization of `Base` if some
        // materialization of `Derived` is a subtype of some materialization of `Base`, i.e. either the top or
        // bottom of `Derived` is between the top and bottom of `Base`, or both materialization of `Base`
        // are between the top and bottom of `Derived`.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:726 on 2025-08-27 19:08_

I'm not sure what this TODO refers to. I think what was previously handled by `variance_with_polarity` here (that is, respecting the passed in polarity of the materialization desired) is now correctly handled by the use of `materialization_type` and `materialization_type.flip()` in the covariant/contravariant branches below.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:846 on 2025-08-27 19:13_

For this to be correct, it is important that we do ensure that we never preserve a materialization in the `Specialization` unless there is some invariant typevar in it with a non-fully-static type. As discussed above, I don't think `apply_type_mapping_impl` currently does that. (This might suggest a possible test to expose that issue?)

---

_@carljm reviewed on 2025-08-27 19:13_

Fantastic!

---

_@AlexWaygood reviewed on 2025-08-27 19:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6671 on 2025-08-27 19:27_

I think if Micha weren't on holiday he'd tell us that Rust naming conventions are such that you rarely prefix methods with `get_` 😄 maybe just `materialization_type`?

---

_@carljm reviewed on 2025-08-27 19:30_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6671 on 2025-08-27 19:30_

> maybe just `materialization_type`

yes, except `materialization_kind` :)

---

_@JelleZijlstra reviewed on 2025-08-27 21:46_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-27 21:46_

What I tried was this:

```
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -5971,7 +5971,11 @@ impl<'db> Type<'db> {
         db: &'db dyn Db,
         specialization: Specialization<'db>,
     ) -> Type<'db> {
-        self.apply_type_mapping(db, &TypeMapping::Specialization(specialization))
+        let new_specialization = self.apply_type_mapping(db, &TypeMapping::Specialization(specialization));
+        match specialization.specialization_type(db) {
+            None => new_specialization,
+            Some(materialization_type) => new_specialization.materialize(db, materialization_type)
+        }
     }
 
     fn apply_type_mapping<'a>(
```

Which should apply recursively since `materialize` is recursive.

---

_@JelleZijlstra reviewed on 2025-08-27 21:46_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:210 on 2025-08-27 21:46_

Sure, I can switch to kind.

---

_@JelleZijlstra reviewed on 2025-08-27 21:48_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:521 on 2025-08-27 21:48_

Oh yes, that makes sense. 

---

_@JelleZijlstra reviewed on 2025-08-27 21:53_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:411 on 2025-08-27 21:53_

I consistently used `specialization_type` for a variable of type `Option<MaterializationType>`, on the theory that None doesn't represent a specific materialization. But maybe that's too subtle a difference.

---

_@carljm reviewed on 2025-08-27 21:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:411 on 2025-08-27 21:57_

In that case it could be `maybe_materialization` -- but I don't think that's necessary. I think it is common to have a variable of `Option` type named as if it is `Some`, where the semantics of `None` are implied by absence. I think that applies pretty well here -- if `materialization` or `materialization_kind` is `None`, then this doesn't represent any materialization.

---

_@JelleZijlstra reviewed on 2025-08-27 21:59_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:641 on 2025-08-27 21:59_

I think the variant you propose (dropping use of `has_dynamic_invariant_typevar`) would have the result that we'd set the materialization_type (or kind, I'll change that) on the synthesized base class, so we'd represent the base class of `Top[list[Any]]` as `Top[Sequence[object]]` instead of `Sequence[object]`. But that may not actually have any visible effect, so maybe it's fine? I imagine it's worse for caching though.

Regarding using a `Vec<Option<MaterializationKind>>`: I think conceptually you could have a different materialization kind per TypeVar. For example, a `dict[Any, Any]` with Top applied to its first TypeVar but not its second would represents all dicts of any key type, mapped to an unknown value type. But we don't have a use case for such a type, or a way to represent it using the `Top[]` syntax, so I think it makes more sense to stick with a single flag for the whole specialization.

It may be worth it to cache the `materialize` method so we can return quickly when materializing a type that's already fully static.

---

_@JelleZijlstra reviewed on 2025-08-27 22:00_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:451 on 2025-08-27 22:00_

Leftover from before I split the two functions :)

---

_@JelleZijlstra reviewed on 2025-08-27 22:02_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:726 on 2025-08-27 22:02_

The old code used polarity and I didn't fully understand what it meant. Perhaps we can just drop the `variance_with_polarity` method.

---

_@JelleZijlstra reviewed on 2025-08-27 22:02_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:846 on 2025-08-27 22:02_

I think it currently does, through `has_dynamic_invariant_typevar`.

---

_@carljm reviewed on 2025-08-27 22:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-27 22:03_

Ah, I see, interesting. Yes, I see how that would be a lot nicer if it works; would eliminate the significant redundancy we currently have between `apply_type_mapping_impl` and `materialize` implementations.

It's not immediately clear to me why that wouldn't work. I think it's worth putting a TODO here to investigate this further, but it doesn't have to block this PR. What was the failure symptom? Was it widespread or just one or two specific cases?

If you have the failing version and want to push it up as a draft stacked PR, I can take a look and see if anything is apparent.

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:465 on 2025-08-27 22:08_

I had this rewritten comment locally, which I think is clearer:

```
        // The bottom materialization of `Derived` is a subtype of the top materialization
        // of `Base` if there is some type that is both within the
        // range of types covered by derived and within the range covered by base, because if such a type
        // exists, it's a subtype of `Top[base]` and a supertype of `Bottom[derived]`.
```

(After I had written the current version of the PR, I thought I could simplify it to something like `(Top[Derived] - Bottom[Derived]) & (Top[Base] - Bottom[Base]) != Never`. But that's wrong, because we need to consider sets of *types* here, not sets of *values*, which is what standard intersection logic would do.)

---

_@JelleZijlstra reviewed on 2025-08-27 22:08_

---

_@carljm reviewed on 2025-08-27 22:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:641 on 2025-08-27 22:08_

> I think the variant you propose (dropping use of `has_dynamic_invariant_typevar`) would have the result that we'd set the materialization_type (or kind, I'll change that) on the synthesized base class, so we'd represent the base class of `Top[list[Any]]` as `Top[Sequence[object]]` instead of `Sequence[object]`. But that may not actually have any visible effect, so maybe it's fine?

I mentioned in another comment that I think it might be detectable in unusual cases via the current implementation of equivalence (which assumes that differing materializations can't be equivalent).

But to be clear, I'm not proposing the implementation without `has_dynamic_invariant_typevar`, just a) observing that it passes all tests, and b) asserting that I think the current `match` is actually equivalent to it, if you trace through all the cases. I don't think its result ever depends on `has_dynamic_invariant_typevar`.

The implementation I actually propose is a simpler if/else that does respect `has_dynamic_invariant_typevar`, ensuring we never store a materialization at all if its `false`, which I think is the desired behavior?

Makes sense re single vs multiple materialization kinds; caching is probably a better answer if re-materializing fully static types is a problem in practice.

---

_@JelleZijlstra reviewed on 2025-08-27 22:17_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-27 22:17_

#20121

---

_@carljm reviewed on 2025-08-27 22:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:726 on 2025-08-27 22:20_

Yes, it looks like in this PR that method is now redundant; it can just be renamed `variance` and assume covariant context.

The meaning of "polarity" in that method name is just that you could get the variance of a bound typevar given some outer variance context (e.g. it might be in covariant context, which would "flip" the result). But that's not an operation we need (at that level) anymore.

The word "polarity" is still used in a `with_polarity` method that's heavily used in PEP 695 variance inference, so if it's confusing maybe we should change that too. But not in this PR :)

---

_@carljm reviewed on 2025-08-27 22:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:846 on 2025-08-27 22:23_

I don't think so (I think the current `match` there never actually depends on `has_dynamic_invariant_typevar`), but I could be analyzing it wrong.

---

_@JelleZijlstra reviewed on 2025-08-27 22:24_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:893 on 2025-08-27 22:24_

I'm actually not sure how top/bottom materializations of a TypeIs should work, but I think that can be a problem for another day.

---

_@JelleZijlstra reviewed on 2025-08-27 22:35_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:641 on 2025-08-27 22:35_

The result does currently depend on `has_dynamic_invariant_typevar`: if `applied_materialization_kind` is Some but `has_dynamic_invariant_typevar` is False, we end up with None. But if `has_dynamic_invariant_typevar` is True in that case, we use the applied materialization.

You're right that your proposed implementation is equivalent to what I currently have but simpler; I'll change that.

We're still in the situation then where no tests fail if we remove the `has_dynamic_invariant_typevar` branching. I think that's because currently, this logic primarily gets exercised when specializing base classes in an MRO, and that logic only gets used in contexts where it doesn't matter if we put in an unnecessary top/bottom. As you observed, it matters for equivalence, but I am not sure you can construct two types that are equivalent except for this issue. Maybe you can think of a situation where this would matter?

I'll add comments regarding the invariant that `materialization_kind` should be set only if there are dynamic, invariant type variables.


---

_@carljm reviewed on 2025-08-27 22:39_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:641 on 2025-08-27 22:39_

> The result does currently depend on `has_dynamic_invariant_typevar`: if `applied_materialization_kind` is Some but `has_dynamic_invariant_typevar` is False, we end up with None. But if `has_dynamic_invariant_typevar` is True in that case, we use the applied materialization.

Ah yes, true, I missed that,  but only if the existing materialization is `None` -- otherwise we'd end up with the existing materialization. (I think the first arm should probably have been `(existing, false, _) => None` instead of `(existing, false, _) => existing`? But that probably only applies in much rarer cases. And it doesn't matter anyway, if we'll switch to the simpler version.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:641 on 2025-08-27 22:43_

I don't think we need to spend more time trying to come up with a test case that fails without this.

---

_@carljm reviewed on 2025-08-27 22:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-28 00:12_

It has something to do with how bases are specialized. Given this test code:

```py
def _(y: Top[InvariantChild[Any]]) -> None:
    reveal_type(y.__mro__)
```

In the working branch, this reveals `tuple[<class 'Top[InvariantChild[Any]]'>, <class 'CovariantBase[object]'>, typing.Generic, <class 'object'>]`, which is expected: the covariant base class materializes the typevar to `object`.

In the non-working branch, this reveals `tuple[<class 'Top[InvariantChild[Any]]'>, <class 'CovariantBase[object]'>, typing.Generic, <class 'object'>, <class 'CovariantBase[Any]'>]` -- for some reason we have both `CovariantBase[object]` and `CovariantBase[Any]` still in the MRO.

---

_@carljm reviewed on 2025-08-28 00:12_

---

_@carljm reviewed on 2025-08-28 00:44_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:592 on 2025-08-28 00:44_

I'm out of time to look into this for now; I think we should go ahead and land this with a TODO comment. I'll add the comment and then merge.

---

_@carljm approved on 2025-08-28 00:49_

---

_Merged by @carljm on 2025-08-28 00:53_

---

_Closed by @carljm on 2025-08-28 00:53_

---
