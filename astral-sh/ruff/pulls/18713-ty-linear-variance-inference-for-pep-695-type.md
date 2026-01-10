```yaml
number: 18713
title: "[ty] linear variance inference for PEP-695 type parameters"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: variance-inference
created_at: 2025-06-17T06:16:27Z
updated_at: 2025-09-01T02:46:38Z
url: https://github.com/astral-sh/ruff/pull/18713
synced_at: 2026-01-10T17:46:21Z
```

# [ty] linear variance inference for PEP-695 type parameters

---

_Pull request opened by @ericmarkmartin on 2025-06-17 06:16_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement linear-time variance inference for type variables (https://github.com/astral-sh/ty/issues/488).

Inspired by Martin Huschenbett's [PyCon 2025 Talk](https://www.youtube.com/watch?v=7uixlNTOY4s&t=9705s).

## Test Plan

update tests
add new test case for mutually recursive classes


---

_Label `ty` added by @AlexWaygood on 2025-06-17 08:19_

---

_Comment by @github-actions[bot] on 2025-06-18 03:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:411 on 2025-06-25 01:16_

The last bullet point of the [scoping rules](https://typing.python.org/en/latest/spec/generics.html#scoping-rules-for-type-variables) section of the spec says that the typevar scope of an outer class does not cover an inner class, so it should be an error for `D` to refer to `T`. (We don't currently check this correctly yet.)

The pattern that they suggest for this example would be

```py
class C[T]:
    class D[U]:
        def f(self, value: U) -> None: ...

    def f(self, value: D[T]) -> None: ...
```

I'm not sure if this invalidates your reasoning in the next paragraph.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:838 on 2025-06-25 01:22_

We should see if Martin has a linkable copy of his slides that we can cite as well.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/place.rs`:492 on 2025-06-25 01:23_

nit: you should't need the qualifier, `PartialOrd` is in the prelude

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:286 on 2025-06-25 01:24_

We are (starting to) try to cut back on the size of this file. Could you move the new variance-related stuff into a new `types/variance.rs` submodule?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:329 on 2025-06-25 01:26_

nit: `TypeVarVariance` is a non-data-carrying enum that implements `Eq`, so you can just use an equality comparison here

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6351 on 2025-06-25 01:30_

There's a helper method for printing out a nicer rendering of the type:

```suggestion
        tracing::debug!(
            "Checking variance of '{tvar}' in `{ty}`",
            tvar = type_var.name(db),
            ty = self.display(db),
        );
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6291 on 2025-06-25 01:32_

My hunch is that we _don't_ want the alternative you suggest in the TODO, since this method should materialize the containing type as it is, and not perform other operations on it.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6297 on 2025-06-25 01:35_

I don't think we want `track_caller` here. That's intended more for e.g. assertion helpers, where locations within the function aren't useful to include in a panic message or stack trace

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6313 on 2025-06-25 01:37_

add a `use` statement for `NodeWithScopeKind` up top, this is an awful lot of redundant qualifiers

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6328 on 2025-06-25 01:38_

Make this a `panic!` instead of an `unreachable!`. The speed difference will not be noticeable, and the extra safety of a panic will be important

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6448 on 2025-06-25 01:42_

Should we change `TypeVarVariance` to be a bitset? Co- and contravariant would be the individual bits; bivariant would be both set; invariant would be both unset.  Then this function would just be bitwise-AND.

(I'm not convinced by this suggestion — I worry it would be more clever than useful.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6600 on 2025-06-25 01:46_

The short-circuiting behavior here isn't semantically important, is it? Will we be folding over large enough iterators for it to be important from a performance standpoint? I feel like a simple non-short-circuiting `fold` will be much easier to understand.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:18 on 2025-06-25 01:48_

Is the `ide_support` import spurious?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:232 on 2025-06-25 01:48_

Our house style would be to call `.chain` on the first iterator directly

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2118 on 2025-06-25 01:52_

This is a minor style nit, but we (or at least I!) try to avoid too much abbreviation in variable names. So I would prefer `spec` → `specialization`; `ctx` → `generic_context`; `tvar` → `typevar`; etc here and elsewhere

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2334 on 2025-06-25 01:53_

The usage here suggests that `all_declarations_and_bindings` belongs somewhere other than `ide_support`, though that might be out of scope for this PR, especially if it's already being used elsewhere for type inference purposes

---

_@dcreager reviewed on 2025-06-25 01:55_

Some initial comments

---

_@ericmarkmartin reviewed on 2025-06-28 23:24_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types.rs`:286 on 2025-06-28 23:24_

Would you move all the trait impls too?

---

_@ericmarkmartin reviewed on 2025-06-28 23:25_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types.rs`:6291 on 2025-06-28 23:25_

Okay, that's what I figured, just wanted to double-check

---

_@ericmarkmartin reviewed on 2025-06-28 23:46_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types.rs`:6600 on 2025-06-28 23:46_

It's definitely not semantically important, just a performance optimization.

I do actually think this one is worth it: suppose we have a class where there's an early invariant occurrence (this is actually relatively common, as any mutable instance member would be). The short-circuit here saves us traversing the entire rest of the class members. If the class references other classes, or even a mutual recursive class, this could save us a lot of work (especially since in the latter case, we're not truly linear due to how salsa handles Fixpoint iterations).

We actually do something similar in [reachability analysis][reachability-analysis] where we short-circuit with the same trick.

[reachability-analysis]: https://github.com/astral-sh/ruff/blob/9218bf72ad84f56f48c3cca985be8565ae1600e7/crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs#L628

---

_@ericmarkmartin reviewed on 2025-06-29 15:34_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types/class.rs`:2118 on 2025-06-29 15:34_

Do you have opinions about `typevar` vs `type_var`?

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types/class.rs`:18 on 2025-06-30 03:38_

No, it's used in the below `all_declarations_and_bindings`

---

_@ericmarkmartin reviewed on 2025-06-30 03:38_

---

_Renamed from "[ty] linear variance inference" to "[ty] linear variance inference for PEP-695 type parameters" by @AlexWaygood on 2025-07-09 08:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:374 on 2025-07-22 23:48_

I would find this explanation of the example a bit clearer if it acknowledged the existence of `Y` (the type parameter in `D`). It seems to entirely conflate that type parameter with `X`

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5564 on 2025-07-23 00:14_

I think the PEP / spec suggests an equivalence of `Self` to an invariant typevar, and I'm not sure it would ever make a difference. I don't think we need to trigger variance inference on `Self`. It doesn't seem to impact any tests if I do this:
```suggestion
                        Some(TypeVarVariance::Invariant),
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6310 on 2025-07-23 00:40_

Do we have tests for these cases? As an experiment, I changed `Contravariant` to `Covariant` for intersection negative contributions, and it didn't seem to change any test results.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7150 on 2025-07-23 00:43_

Eventually we should support also legacy typevars with `infer_variance=True` kwarg... but I think this method wouldn't really be used for those? Because in that case there is no variance for the `TypeVarInstance` per se, only for its use in a particular class.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7169 on 2025-07-23 00:50_

I don't think variance is meaningful for either type alias type parameters or function type parameters, because unlike generic classes, neither of those can have persistent generic instances which participate in subtyping. So I think that we could safely just return invariant here for those two, without doing any further work.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6564 on 2025-07-23 00:53_

This is only possible for synthesized typevars; I think returning Invariant is fine there; if we care about the variance of a synthesized typevar, we'll specify it explicitly.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/variance.rs`:72 on 2025-07-23 01:16_

```suggestion
    // instead have it evaluate to bivariant. This is a valid choice, as
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/protocol_class.rs`:246 on 2025-07-23 01:36_

I think we might need to switch on `member.kind` here? Some protocol member kinds (e.g. regular attributes) will be invariant. (I'm not sure if those are currently represented as `ProtocolMemberKind::Other`, or aren't represented yet; @AlexWaygood would know. It may be this is just a TODO for when further protocol member kinds are added.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:418 on 2025-07-23 01:37_

Where are we using this? Could we add an accessor method instead?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:40 on 2025-07-23 01:42_

General note: it's great that we are eliminating all these TODOs, and that we have the mutually-recursive case from Martin, but those two together cover pretty much the bare-bones minimal cases (plus mutual recursion). There's a lot of logic included in this PR that doesn't seem to be covered by any test. Ideally we'd have at least one test for every case of variance inference we have dedicated handling for in the PR. If you don't have time to add those tests, let us know and we can try to find someone on the team to do it?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:349 on 2025-07-23 01:49_

What if `generic_type_var` is a legacy type var with explicitly specified variance? Should `Type::variance_of` itself short-circuit and return the explicit variance in this case?

(Possibly related: at some point we'll probably also want to issue a diagnostic if explicitly-specified-variance type vars are used in a position that contradicts their explicit variance; not sure if it will make sense to reuse some of this logic for that diagnostic as well.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2481 on 2025-07-23 01:51_

It seems like `type_var_in_generic_context` would be a better name for this?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2496 on 2025-07-23 02:00_

I'm pretty sure once we get all the details right here (including checking qualifiers), we'll no longer be sharing this function with `ide_support`, we'll need our own version of it dedicated to this purpose.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2499 on 2025-07-23 02:06_

I think properties are another case where we should visit them in a covariant context, not invariant.

Also `Final` annotated members can be visited covariantly.

And `ClassVar` members are a little odd -- they shouldn't include the typevar directly, but they could potentially be a callable type with the typevar in the signature. This can be a TODO maybe.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2518 on 2025-07-23 02:17_

I don't think we need to consider classmethods here -- they cannot define instance attributes that could use the typevar.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6362 on 2025-07-23 02:19_

I think this question applies to `FunctionLiteral` above as well (or even more -- since we are pulling member types directly out of the class scope I think it would be pretty difficult for us to even encounter a BoundMethod type). I _think_ it should be safe to ignore the type of `self`, but I'm not sure if we _need_ to. In most cases it won't be explicitly annotated. Probably fine to leave this as a TODO.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2496 on 2025-07-23 02:22_

I think we need to ignore `__init__` and `__new__` methods here. It looks like currently in this branch, if `__init__` accepts an argument `val: T` we take that as making the class contravariant in `T`, but that's not right. Constructors aren't instance methods. (We should have a test for this.)

---

_@carljm reviewed on 2025-07-23 02:26_

This is looking good! I think we can leave a lot of stuff as TODOs for follow-up PRs, just want to make sure we clearly record all the TODOs. I would really like to have more thorough tests here.

Regarding the failing test, I suspect what is happening is that we now see that type as bivariant in `T` (because `T` is unused), and that means that every specialization of `C` is equivalent to every other specialization, so the `__init__` overloads no longer discriminate the instantiation argument, the overloads just all match, resulting in `Unknown`. 

If I change the class like this (to make it covariant in `T`), the test passes:

```py
class C[T]:
    @overload
    def __init__(self: C[str], x: str) -> None: ...
    @overload
    def __init__(self: C[bytes], x: bytes) -> None: ...
    @overload
    def __init__(self: C[int], x: bytes) -> None: ...
    @overload
    def __init__(self, x: int) -> None: ...
    def __init__(self, x: str | bytes | int) -> None:
        self.x = x

    def method(self) -> T:
        return self.x
```

---

_Comment by @carljm on 2025-07-24 00:14_

I'm kind of shocked that there is zero ecosystem impact reported here? Is PEP 695 uptake still that low? Or maybe because we previously assumed "invariant" for PEP 695 typevars, this doesn't actually result in any new diagnostics? (Also maybe due to some issues pointed out above, e.g. the `__init__` thing, in practice we are still inferring ~all PEP 695 typevars in the ecosystem as invariant?)

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types.rs`:7150 on 2025-07-24 01:29_

In some ways we're well set up for this because we also have a form of variance inferrence that is for a variable _in_ a type, but over there our heuristic for checking if the type variable was defined in the class scope is gonna bite us...perhaps we can instead have an invariant that we never attempt to do inference in a class that's not relevant?

---

_@ericmarkmartin reviewed on 2025-07-24 01:29_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types.rs`:7169 on 2025-07-24 05:59_

Updated to return invariant for function type parameters, but it seems like we probably do want to support inference for type alias parameters

---

_@ericmarkmartin reviewed on 2025-07-24 05:59_

---

_@carljm reviewed on 2025-07-24 23:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7150 on 2025-07-24 23:20_

What is that heuristic? Isn't it checking if the typevar is in the class' generic context? I think that might be fine -- we also add legacy typevars mentioned in a class' bases (e.g. where it inherits `Generic`) into its generic context.

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types.rs`:7150 on 2025-07-30 02:23_

Oh good point, I totally forgot we changed that

---

_@ericmarkmartin reviewed on 2025-07-30 02:23_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:7150 on 2025-08-05 17:37_

> Eventually we should support also legacy typevars with `infer_variance=True` kwarg... but I think this method wouldn't really be used for those? Because in that case there is no variance for the `TypeVarInstance` per se, only for its use in a particular class.

We can now track different uses of a legacy typevar as of #19604. The `binding_context` of the `TypeVarInstance` will be `None` for each reference to the typevar outside of a generic context, and will have different non-`None` values for each use in a generic context. So while still likely out of scope for this PR, I think we can now also use this method for legacy `infer_variance=True`.

---

_@dcreager reviewed on 2025-08-05 17:39_

---

_Comment by @github-actions[bot] on 2025-08-15 03:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-20 00:39:59.104775161 +0000
+++ new-output.txt	2025-08-20 00:40:01.565795171 +0000
@@ -580,21 +580,15 @@
 generics_variance_inference.py:19:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T3@ClassA`
 generics_variance_inference.py:24:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int, int, int]`
 generics_variance_inference.py:25:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int | float, int | float, int]`
-generics_variance_inference.py:26:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int | float, int, int | float]`
 generics_variance_inference.py:28:5: error[invalid-assignment] Object of type `ClassA[int, int | float, int | float]` is not assignable to `ClassA[int, int, int]`
-generics_variance_inference.py:29:5: error[invalid-assignment] Object of type `ClassA[int, int | float, int | float]` is not assignable to `ClassA[int, int, int | float]`
 generics_variance_inference.py:33:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@ShouldBeCovariant1`
 generics_variance_inference.py:36:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T@ShouldBeCovariant1]`
-generics_variance_inference.py:40:1: error[invalid-assignment] Object of type `ShouldBeCovariant1[int]` is not assignable to `ShouldBeCovariant1[int | float]`
 generics_variance_inference.py:41:1: error[invalid-assignment] Object of type `ShouldBeCovariant1[int | float]` is not assignable to `ShouldBeCovariant1[int]`
-generics_variance_inference.py:48:1: error[invalid-assignment] Object of type `ShouldBeCovariant2[int]` is not assignable to `ShouldBeCovariant2[int | float]`
 generics_variance_inference.py:49:1: error[invalid-assignment] Object of type `ShouldBeCovariant2[int | float]` is not assignable to `ShouldBeCovariant2[int]`
 generics_variance_inference.py:53:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ShouldBeCovariant2[T@ShouldBeCovariant3]`
-generics_variance_inference.py:57:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int]` is not assignable to `ShouldBeCovariant3[int | float]`
 generics_variance_inference.py:58:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int | float]` is not assignable to `ShouldBeCovariant3[int]`
 generics_variance_inference.py:66:1: error[invalid-assignment] Object of type `ShouldBeCovariant4[int]` is not assignable to `ShouldBeCovariant4[int | float]`
 generics_variance_inference.py:67:1: error[invalid-assignment] Object of type `ShouldBeCovariant4[int | float]` is not assignable to `ShouldBeCovariant4[int]`
-generics_variance_inference.py:79:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int]` is not assignable to `ShouldBeCovariant5[int | float]`
 generics_variance_inference.py:80:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int | float]` is not assignable to `ShouldBeCovariant5[int]`
 generics_variance_inference.py:96:1: error[invalid-assignment] Object of type `ShouldBeInvariant1[int]` is not assignable to `ShouldBeInvariant1[int | float]`
 generics_variance_inference.py:97:1: error[invalid-assignment] Object of type `ShouldBeInvariant1[int | float]` is not assignable to `ShouldBeInvariant1[int]`
@@ -607,12 +601,9 @@
 generics_variance_inference.py:130:1: error[invalid-assignment] Object of type `ShouldBeInvariant4[int]` is not assignable to `ShouldBeInvariant4[int | float]`
 generics_variance_inference.py:138:1: error[invalid-assignment] Object of type `ShouldBeInvariant5[int]` is not assignable to `ShouldBeInvariant5[int | float]`
 generics_variance_inference.py:149:1: error[invalid-assignment] Object of type `ShouldBeContravariant1[int]` is not assignable to `ShouldBeContravariant1[int | float]`
-generics_variance_inference.py:150:1: error[invalid-assignment] Object of type `ShouldBeContravariant1[int | float]` is not assignable to `ShouldBeContravariant1[int]`
 generics_variance_inference.py:169:1: error[invalid-assignment] Object of type `ShouldBeInvariant6[int | float]` is not assignable to `ShouldBeInvariant6[int]`
 generics_variance_inference.py:170:1: error[invalid-assignment] Object of type `ShouldBeInvariant6[int]` is not assignable to `ShouldBeInvariant6[int | float]`
 generics_variance_inference.py:181:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int | float]` is not assignable to `ShouldBeCovariant6[int]`
-generics_variance_inference.py:182:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int]` is not assignable to `ShouldBeCovariant6[int | float]`
-generics_variance_inference.py:193:1: error[invalid-assignment] Object of type `ShouldBeContravariant2[int | float]` is not assignable to `ShouldBeContravariant2[int]`
 generics_variance_inference.py:194:1: error[invalid-assignment] Object of type `ShouldBeContravariant2[int]` is not assignable to `ShouldBeContravariant2[int | float]`
 literals_interactions.py:15:5: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
 literals_interactions.py:16:5: error[index-out-of-bounds] Index -5 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
@@ -859,5 +850,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 860 diagnostics
+Found 851 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Marked ready for review by @ericmarkmartin on 2025-08-15 04:19_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-08-15 04:19_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-08-15 04:19_

---

_@JelleZijlstra reviewed on 2025-08-15 05:15_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md`:240 on 2025-08-15 05:15_

Do you mean bivariant? I'd expect the `x: T` to actually make it invariant.

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:397 on 2025-08-15 05:16_

```suggestion
Normal attributes are mutable, and so are invariant (see [inv]).
```

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:413 on 2025-08-15 05:16_

... unfinished sentences?

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:397 on 2025-08-15 05:17_

Perhaps pedantically, I don't think you can say that the attribute itself is invariant; rather, their presence leads us to infer that the enclosing generic class in invariant over this TypeVar.

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:416 on 2025-08-15 05:18_

```suggestion
Immutable attributes can't be written to, and so don't have the same behavior as mutable ones.
```
Who said that invariance is a problem?

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:512 on 2025-08-15 05:19_

D doesn't have an `x` attribute

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:465 on 2025-08-15 05:19_

```suggestion
    def y(self, value: U): ...
```

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:520 on 2025-08-15 05:19_

```suggestion
This holds likewise for dataclasses with synthesized `__init__`:
```

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/class.rs`:3154 on 2025-08-15 05:25_

```suggestion
                        // we want to be robust to new qualifiers
```

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/variance.rs`:20 on 2025-08-15 05:27_

is this a term of art?

---

_@JelleZijlstra reviewed on 2025-08-15 05:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md`:240 on 2025-08-15 19:55_

```suggestion
We have to add `x: T` to the classes to ensure they're not bivariant in `T` (__new__ and __init__
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:371 on 2025-08-15 21:04_

> `X` has one bivariant occurrence

It's not clear to me where this bivariant occurrence is? Is it the occurrence in the return type of `f`, because we are currently assuming that `D` is bivariant in `Y`? If so, that seems like the key point here, but it isn't really mentioned at all.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:375 on 2025-08-15 21:05_

I'm not sure this description currently captures the cyclic nature of this resolution? It's presented as if we can independently conclude that `X` is contravariant in `C`, and then based on that, conclude that `Y` is contravariant in `D`. But that entirely glosses over the interesting part (the cycle), and the fact that we at some point have to make a co-inductive assumption when we hit a cycle.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:362 on 2025-08-15 21:08_

I think that the more common usage would be to say that "`C` is contravariant in `X`" rather than the other way around.

Also, we shouldn't ignore `Y` here.
```suggestion
`C` is contravariant in `X`, and `D` in `Y`:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:397 on 2025-08-15 21:14_

I think there might be a sense in which you could talk about the variance of an attribute itself, if you are talking about how the type of the attribute is allowed to vary in subclasses (if it has a concrete type, not a TypeVar)? But I agree that here we could use more precise language:
```suggestion
Normal attributes are mutable, and so make the enclosing class invariant in this typevar (see [inv]).
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:416 on 2025-08-15 21:17_

I'd probably go even a bit more explicit here

```suggestion
Immutable attributes can't be written to, and thus constrain the typevar to covariance, not
invariance:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:448 on 2025-08-15 21:29_

```suggestion
Properties constrain to covariance if they are get-only and invariant if they are get-set:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:512 on 2025-08-15 21:31_

I'm having trouble understanding what this sentence is trying to say; where did `int` come from?

Not sure this sentence is even needed, we could just join the two code snippets.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:603 on 2025-08-15 21:37_

```suggestion
Implicit attributes work like normal ones:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:614 on 2025-08-15 21:42_

A dataclass with no fields typed as `T` won't include `T` anywhere in the `__init__` signature, so I'm not sure what this example actually demonstrates.

Really, the example above with a frozen dataclass already demonstrates this better: that one will include `T` in the synthesized `__init__`, and if we considered that `__init__` then we'd still infer it as invariant, even though the attribute is read-only.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:838 on 2025-08-15 21:51_

I've asked Martin and can add the link here if/when he replies, but I don't think this is a blocker

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:752 on 2025-08-15 21:52_

Not clear to me why we need this example here (in addition to the explicit subtype/assignable checks) but not in any of the above cases.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/variance.rs`:20 on 2025-08-15 22:00_

Yes: https://en.wikipedia.org/wiki/Infimum_and_supremum

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/variance.rs`:97 on 2025-08-15 22:05_

I might be missing something here, but how is this the infimum? Isn't it the supremum of all variances we've seen so far in this iterator?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6998 on 2025-08-15 22:08_

```suggestion
    /// The explicitly specified variance of the TypeVar
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3074 on 2025-08-15 22:25_

I think this won't find subclasses of classed defined with `NamedTuple` -- we'll probably want a better way to identify `NamedTuple` fields than this. (But this could be a follow-up TODO.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:349 on 2025-08-15 22:28_

Ignoring the second part for now (I just filed https://github.com/astral-sh/ty/issues/1017 to track that for later), it still seems to me there might be something we could do differently here if `generic_type_var` has explicit variance? But it might be just an optimization to do less work in that case, and it might only make a difference if the explicit-variance legacy typevar is being used in a way that doesn't actually match its explicit variance. So this can probably just be a TODO for later follow-up.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3114 on 2025-08-15 22:29_

Pretty sure we do not, we only need to consider symbols

---

_@carljm reviewed on 2025-08-15 22:32_

Didn't quite get the whole way through reviewing this, but to me it's looking pretty much land-ready so far with some minor adjustments. I would probably just make the adjustments myself and land it now, but I'm out of time for today. Will try to get back to it as soon as I can; would love to get it landed before it accumulates more conflicts.

---

_Comment by @carljm on 2025-08-15 22:33_

Looks like there are also a couple clippy / `cargo doc` issues we need to address, and then we also need to review the conformance-suite diff for correctness.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3145 on 2025-08-16 15:12_

No tests fail if I remove this line. I don't think there is actually any way for the signatures of synthesized ordering methods, `__eq__`, `__repr__`, or `__hash__` to ever contain a typevar, so I don't think we can write a test that would require this line. Given that, I'm inclined to remove this extra logic and replace it with a comment that dataclasses may have some additional synthesized methods which we don't consider since they won't affect variance.

---

_@carljm reviewed on 2025-08-16 15:19_

---

_Comment by @carljm on 2025-08-16 15:27_

Finished my code review, and I reviewed the conformance suite impact. Mostly the result is good (we stop emitting wrong diagnostics where previously we assumed invariance and now we infer correct variance.)

On [these lines](https://github.com/python/typing/blob/main/conformance/tests/generics_variance_inference.py#L169-L194), which are testing variance in derived classes, we now miss all the errors; it seems like we are inferring all these as bivariant now somehow? Will explore this a bit more.

ETA: Oh, I think this is the issue I mentioned inline in my code review, where when we encounter another generic class while inferring variance, we are ignoring explicit variance.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3074 on 2025-08-16 16:36_

I realized that we don't _want_ to match `NamedTuple` subclasses here, because we are only considering own-attributes; we do variance inference on base classes separately. Only own-attributes of actual named tuples are immutable; attributes tacked on in a subclass are mutable. Adding some tests for these cases.

But I also realized there is an easier way to do this, using `CodeGeneratorKind::matches`.

---

_@carljm reviewed on 2025-08-16 16:36_

---

_@carljm reviewed on 2025-08-16 16:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3145 on 2025-08-16 16:59_

I realized in making this change that there is (in newer Python versions) the `__replace__` method, which breaks this covariance, as discussed in https://discuss.python.org/t/make-replace-stop-interfering-with-variance-inference/96092/16

Added some tests and handling for this.

---

_@AlexWaygood reviewed on 2025-08-16 17:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 17:01_

it surprises me that we have to special-case NamedTuples here. Our NamedTuple handling is such that if you access an attribute on a NamedTuple class object using `Type::member()`, you'll get a `Type::PropertyInstance()` back that has no setter. In general we should surely consider any attribute on any class to be covariant if `Type::member()` on the class object returns a `Type::PropertyInstance()` with no setter. So this indicates to me that perhaps the generalised logic for non-namedtuples maybe isn't generalised enough?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 17:02_

I'd guess this is because the code is using the low-level `all_end_of_scope_symbol_declarations()` and `all_end_of_scope_symbol_bindings()` APIs rather than the high-level `Type::member()` API. The low-level APIs will just look directly at the annotation on the `NamedTuple` class, leading us to infer a mutable attribute annotation; the high-level `Type::member()` API will return a `Type::PropertyInstance` type.

---

_@AlexWaygood reviewed on 2025-08-16 17:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:349 on 2025-08-16 17:06_

I realized (thanks to the conformance test suite) that this is important for correctness, because explicit (legacy) variance can also enforce a stricter variance than we would otherwise infer here. Adding some tests and looking at fixing this.

---

_@carljm reviewed on 2025-08-16 17:06_

---

_@carljm reviewed on 2025-08-16 17:17_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 17:17_

The logic here doesn't access members (other than implicit instance members) individually using the full individual member lookup methods. To do that, we'd first have to query names of all "own members" using the use-def map (and throwing away qualifier information), then re-lookup those same attributes one by one (effectively re-doing the initial use-def map queries).

We could choose to do this, if we judge that the gain in simplicity / robustness outweighs the loss in efficiency. But it's not clear to me that that's the case. The logic here for namedtuples and dataclasses is fairly simple, and I think it's correct.

---

_@AlexWaygood reviewed on 2025-08-16 17:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 17:33_

My feeling is that using the high-level API is likely to be much more robust here in the long term. We already synthesize attributes on a variety of types -- as well as dataclasses and namedtuples, we also synthesize methods on other tuple types (currently, `__new__`, `__bool__`, `__len__` and `__getitem__`) and `TypedDict` types. (`TypedDict`s are currently always mutable IIRC, but we'd need to remember to update this logic in the future if something like the `TypedMapping` that's previously been proposed was ever accepted.) In the future, I could easily see us synthesizing more methods for `attrs` classes and Django models.

I don't want to block this PR if you feel strongly, but this method is also cached, so I would lean towards doing the more robust thing here unless and until we see this method show up on profiles as a hot routine that's slowing us down.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 17:34_

for full correctness, this should probably also check for function-like `CallableType`s

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3215 on 2025-08-16 17:35_

shouldn't this also check that the `setter` of the `PropertyInstanceType` is `None`?

---

_Comment by @carljm on 2025-08-16 17:35_

With the fix I just pushed, the conformance suite changes all look correct now.

---

_@AlexWaygood reviewed on 2025-08-16 17:35_

---

_@carljm reviewed on 2025-08-16 17:42_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 17:42_

If we want this method to be robust against new synthesized attributes/methods and not require special-casing types that provide them, it will take more than just using the high-level member-lookup API that we already have; it will require new "get me all member names" API that doesn't currently exist, that uses a shared source of truth for what synthesized members can exist on a given type. I think such API would be very useful (it could also fix the fact that I think currently our autocomplete doesn't consider synthesized members), but it's a non-trivial project. IMO this PR has already been sitting long enough and is already complex enough; I don't want it to be blocked on that project as well. And I don't think there is enough benefit to justify going only halfway, using the high-level member-lookup API when we'd still have to special-case dataclasses and namedtuples in order to even consider what synthesized methods might exist.

I've already added a TODO comment for this improvement and can open an issue for it as well.

---

_@carljm reviewed on 2025-08-16 17:46_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3215 on 2025-08-16 17:46_

Yes, this is on my list to look into -- there are tests that show this is working, but I'm not sure why...

---

_@carljm reviewed on 2025-08-16 17:47_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 17:47_

I don't think that it should? A function literal type represents an actual method -- we treat those as immutable, but only as a side effect of the fact that function literals are singleton types, so there's nothing else we'd ever allow you to assign to a declared function literal. Any other callable-type attribute is mutable and thus I think shouldn't be treated as covariant.

---

_@carljm reviewed on 2025-08-16 17:50_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3215 on 2025-08-16 17:50_

Oh never mind, I know why. Effectively this line is just saying "we don't consider that the property instance itself could be replaced on an instance" -- this is equivalent to the case for function literals above, we're basically just saying "properties, like methods, are implicitly `ClassVar`".

But we still visit the property instance type itself in variance inference below, so we'll still observe the signatures of its getter and setter methods (just like we'll observe the signature of a method), and we'll see that the setter (if it exists) has the type variable in contravariant position.

I can add some comments to clarify what's going on here.

---

_Comment by @carljm on 2025-08-16 17:54_

Ok, I pushed some things here, but didn't address all the comments yet. Out of time for now. I don't have any more changes locally, so @ericmarkmartin if you have time/inclination, feel free to push changes addressing additional comments! I'll come back to this next week (but I'll comment before I make any further changes.)

---

_@carljm reviewed on 2025-08-16 17:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 17:57_

When I come back to this next week, I'll re-consider the "going halfway" option for this PR.

This thread also made me realize that we might need to add explicit consideration of the other synthesized `NamedTuple` methods that come from `NamedTupleFallback`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 18:01_

So we're saying that this class is covariant:

```py
class Foo[T]:
    def __or__(self, other: Foo[T]) -> Foo[T]: ...
    __ror__ = __or__
```

But this one is not?

```py
from typing import Callable

class Foo[T]:
    __or__: Callable[[Foo[T], Foo[T]], Foo[T]] = lambda self, other: self
```

FWIW for the purposes of protocol interfaces, we currently treat any function-like callable that is bound on the class as a method member, just like function-literal types that are bound on the class

---

_@AlexWaygood reviewed on 2025-08-16 18:01_

---

_@carljm reviewed on 2025-08-16 18:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 18:03_

I think what we are kind of missing here is a clear mental model (as well as clear documentation, and ideally a single source of truth) for which "declared" class members (that aren't explicitly declared with `ClassVar`) we implicitly treat as class-only variables (which also means we can treat them covariantly here.)

The current answer here is "methods and properties", which I think is roughly reasonable, but it would be good if this were more explicit and centralized.

---

_@AlexWaygood reviewed on 2025-08-16 18:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 18:03_

> it will require new "get me all member names" API that doesn't currently exist, that uses a shared source of truth for what synthesized members can exist on a given type

I thought we _did_ already have that, and it was called `types::ide_support::all_members()` 😄

If that routine doesn't currently include certain attributes/methods synthesized on NamedTuples or dataclasses, that seems like a pre-existing bug in that routine

---

_@carljm reviewed on 2025-08-16 18:12_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 18:12_

> But this one is not?

We would currently allow `__or__` to be mutated on an instance of the second `Foo`, as long as you assign a compatible callable type. So it shouldn't be covariant, unless we establish some principle by which that annotated `__or__` can be considered class-only, despite not being annotated with `ClassVar`. I'm not sure "callable types are always implicitly `ClassVar`" is the right rule here.

If `__or__` were explicitly annotated with `ClassVar`, then that second `Foo` would be covariant.

---

_@carljm reviewed on 2025-08-16 18:14_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 18:14_

Also, the first `Foo` there is not covariant either, because the type of `__ror__` is `Unknown | FunctionLiteral(...)` (it has no declared type), and we don't special-case that union here.

---

_@carljm reviewed on 2025-08-16 18:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 18:23_

`ide_support::all_members` isn't usable here because it includes superclass members also, here we only want to include own-members.

Definitely possible that the right answer here involves increased code sharing between the class-types branch of `all_members` and this code, but it's definitely more complicated than "just use `all_members`". I still think that's a big enough project that it would be better to make it a TODO and separate PR.

---

_@AlexWaygood reviewed on 2025-08-16 18:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 18:23_

Thank you for the corrections. Something like this might be more interesting as a talking point:

```py
from typing import Final

class Foo[T]:
    def __or__(self, other: Foo[T]) -> Foo[T]: ...
    __ror__: Final = __or__
```

Vs

```py
from typing import Callable, ClassVar

class Foo[T]:
    __or__: ClassVar[Callable[[Foo[T], Foo[T]], Foo[T]]] = lambda self, other: self
```

Though that second one might not be legal, because there are rules against using type variables inside `ClassVar`s? Can't remember the exact details.

Anyway, my point is that it seems somewhat arbitrary to distinguish between functions that are defined using `def`, and other callables that we also know are instances of `FunctionType` (they wouldn't be marked as function-like callables if they weren't), also have method-binding behaviour when accessed on instances, and are also bound on the class object itself. At runtime, the two are identical in their behaviour if you're defining a method on the class.

I agree that we don't have a particularly consistent mental model about this right now, though, and it's hardly high-priority. I'm definitely fine with deferring this discussion to a later date and landing the PR with this logic as-is!

---

_@AlexWaygood reviewed on 2025-08-16 18:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3157 on 2025-08-16 18:24_

Fair enough.

---

_@carljm reviewed on 2025-08-16 18:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3212 on 2025-08-16 18:27_

Just a note that I think the rule against using type variables inside a ClassVar should not apply to typevars used in the signature of a callable type. If the spec doesn't have that carve-out, it probably should.

---

_@ericmarkmartin reviewed on 2025-08-16 21:28_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types/variance.rs`:97 on 2025-08-16 21:28_

Ope--I either forgot to change this after I flipped the lattice (initially i had it upside-down) or I'm just cursed to never remember join vs meet correctly.

---

_@ericmarkmartin reviewed on 2025-08-17 02:47_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:512 on 2025-08-17 02:47_

I think this was me trying to communicate a point you made in discord---we want to access members without going through the descriptor protocol.

The int thing is a goof on my part. I was trying to come up with an example where the return type of the getter would be deceiving versus that of the descriptor variance-wise, and was trying to imagine a descriptor whose getter would return `int` even though it stored a `T`, but I couldn't really do it, and never cleaned this up--sorry!

---

_Comment by @ericmarkmartin on 2025-08-17 02:57_

I think I've addressed the last few comments that you didn't resolve yourself, thanks!

I tried to push out the code quickly the other night because I'm away for a little bit now and didn't want to delay the PR more, and I appreciate the thorough review. Fortunately the WiFi up here is actually quite good 😃.

I'm noticing that with the bound typevars, you write `typevar` and not `type_var`. Would you like me to update my code likewise? 

---

_Comment by @carljm on 2025-08-19 01:32_

> I'm noticing that with the bound typevars, you write `typevar` and not `type_var`. Would you like me to update my code likewise?

I don't have strong feelings between the two, but consistency is good. Maybe a mild preference for avoiding unnecessary underscores.

---

_Comment by @carljm on 2025-08-19 22:43_

I'm about to rebase this and make some more updates to it, hopefully resulting in a merge. (Just commenting in advance so as to avoid simultaneous duplication of work.)

---

_@carljm reviewed on 2025-08-19 23:22_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6448 on 2025-08-19 23:22_

Just scanning this PR for unresolved comments and ran across this. I agree this would be conceptually neat; I also agree that I struggle to see practical benefit. Will leave it as an enum for now.

---

_Merged by @carljm on 2025-08-20 00:54_

---

_Closed by @carljm on 2025-08-20 00:54_

---

_Comment by @carljm on 2025-08-20 00:54_

Merged! Thank you @ericmarkmartin for all your excellent work here

---

_Branch deleted on 2025-09-01 02:46_

---
