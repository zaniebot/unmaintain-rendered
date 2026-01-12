```yaml
number: 15194
title: "[red-knot] Support `assert_type`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-assert-type
created_at: 2024-12-30T03:53:20Z
updated_at: 2025-01-10T16:50:07Z
url: https://github.com/astral-sh/ruff/pull/15194
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Support `assert_type`

---

_@InSyncWithFoo_

## Summary

See #15103.

## Test Plan

Markdown tests and unit tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-30 03:53_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-30 03:53_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-30 03:53_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-30 03:53_

---

_Comment by @github-actions[bot] on 2024-12-30 03:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2024-12-30 04:02_

This is largely a proof-of-concept. `is_equals_to()` is not a thing in the specification, and I'm quite sure its name is not the best.

---

_Label `red-knot` added by @MichaReiser on 2024-12-30 08:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-03 13:35_

Can you go into a bit more detail in how this is different from `is_equivalent_to`? The main difference seems to be that this can also handle non-fully-static types and return `true` for something like `Any ~ Any`? If so, could it handle those gradual types (+ intersections/unions/tuples) and then fall back to `is_equivalent_to` for fully static types?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:79 on 2025-01-03 13:39_

It feels like this should be tested elsewhere? Or what exactly is the `assert_type` property that is tested here?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:1 on 2025-01-03 13:42_

Remove?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:1 on 2025-01-03 13:44_

One thing that we should probably test here somewhere (if only to document our behavior) is the handling of `Literal` types. And whether we use the inferred type or the declared type. For example, pyright behaves like this:
```py
from typing import assert_type

x: int = 1

assert_type(x, int)  # "assert_type" mismatch: expected "int" but received "Literal[1]"

def f():
    assert_type(x, int)  # fine (uses declared type)
```

It looks like we would currently have the same behavior?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:33 on 2025-01-03 13:47_

Again, we should specify somewhere what "precisely" or "exactly the same" means. Because it's not "structural equivalence" or `Eq` on `Type` (because `int | str` and `str | int` should match, for example)

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:120 on 2025-01-03 13:49_

:+1: 

---

_@sharkdp reviewed on 2025-01-03 13:50_

Thank you very much for working on this. I added a few initial review comments.

---

_@AlexWaygood reviewed on 2025-01-03 16:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-03 16:20_

I think it's quite possible that a function like this could help fix https://github.com/astral-sh/ruff/issues/14899, FWIW, so it doesn't seem like an unreasonable addition to me. I'm also not sure what name would clearly distinguish it from `Type::is_equivalent_to()`, though 😆

---

_@sharkdp reviewed on 2025-01-03 16:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-03 16:38_

To be clear, I was not suggesting that this function is not useful. I just think there's some overlap with `is_equivalent_to`, and I'd like to understand it and possibly make use of it.

---

_@MichaReiser reviewed on 2025-01-03 17:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:501 on 2025-01-03 17:36_

Should this be an `Error`? I'd expect any assertion that doesn't match the expected type to fail my build. 

---

_@InSyncWithFoo reviewed on 2025-01-03 20:15_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:1 on 2025-01-03 20:15_

We should always use the inferred types in the specific scope/code flow. What Pyright is doing seems to be the most user-friendly way; there is no way to know when functions will be called, but it is fine to narrow or infer a more precise type (as is currently done) for even global-scoped flows.

---

_@InSyncWithFoo reviewed on 2025-01-03 20:17_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:79 on 2025-01-03 20:17_

A bare `Literal` should cause an error and be treated as `Unknown`. `type[Unknown]` should then be treated the same as `type[Any]`.

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-03 20:59_

Another notable exception is `type[Any] != type != type[object]`. I'm not too sure about the former, but I think it is better to be strict.

Also, as it currently is, `int | Unknown | Any != int | Any`. This is supposedly due to `Unknown` not being foldable into `Any`, but `int | Any` is just `Any`; could such union types even be emitted in the first place?

---

_@InSyncWithFoo reviewed on 2025-01-03 20:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-08 01:02_

This relation is defined in the type specification, but it's mentioned as a bit of an aside, and unfortunately not given a distinct name from the equivalence relation on fully static types. See https://typing.readthedocs.io/en/latest/spec/concepts.html#summary-of-type-relations :

> We can also define equivalence on gradual types. Two gradual types A and B are equivalent (that is, the same gradual type, not merely consistent with one another) if and only if all materializations of A are also materializations of B, and all materializations of B are also materializations of A.

I think we should link to that definition in the doc comment here, and name the method `is_gradual_equivalent_to`.

I think it's more useful for this doc comment to give this overall conceptual definition than to outline what that means for every concrete type variant; that's what the code is for. (If anything, the per-type-variant descriptions should be individual comments above each case within the method.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-01-08 01:04_

Todo is supposed to behave like `Any` or `Unknown` in the typing system, so I think our default approach should be that all three are gradually-equivalent to each other. Is there a concrete reason you felt it necessary to make `Todo != Todo`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1468 on 2025-01-08 01:06_

For all fully-static types, I think we could just defer to `is_equivalent_to`? And/or we could have a fast-path at the top of this method for `if self == other { return true; }`, similar to `is_equivalent_to`. Either one would eliminate the need for many of these cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1515 on 2025-01-08 01:09_

Nice; this is a fix we need in `is_equivalent_to` as well. It would be nice if there were a way to do it without allocating so many hashsets...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2015 on 2025-01-08 01:11_

These todos can be solved by rebasing this on top of call-checking, which has landed, and https://github.com/astral-sh/ruff/pull/15103, which hopefully will land soon, and has many examples of similar methods.

The one thing we'll need to do that https://github.com/astral-sh/ruff/pull/15103 doesn't yet do is provide a way to say that some arguments of a function must be interpreted as type expressions, and others as value expressions.

---

_@carljm reviewed on 2025-01-08 01:14_

This looks good, thank you for working on it! I think if we rebase on top of https://github.com/astral-sh/ruff/pull/15103 we can address some of the limitations and get it ready to land.

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-01-08 07:43_

I just thought that it would be rather nonsensical to do something like this:

```py
def _(a: tuple[int, ...]):
	assert_type(a, tuple[str, ...])  # Pass because both are Todos
```

No strong feelings though.

---

_@InSyncWithFoo reviewed on 2025-01-08 07:43_

---

_@MichaReiser reviewed on 2025-01-08 09:35_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1449 on 2025-01-08 09:35_

Do we need to compare the `first_positive` and `second_positive` lengths (and same for negative) before calling zip?

---

_@MichaReiser reviewed on 2025-01-08 09:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 09:37_

What makes this set ordered? This is just a regular hash set, isn't it?

---

_@MichaReiser reviewed on 2025-01-08 09:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1446 on 2025-01-08 09:38_

Why do we need to collect the positive elements into another set when they are already set in `IntersectionType`? Could we instead compute the difference between `a` and `b` and then compare if those elements are equivalent?

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:1446 on 2025-01-08 10:05_

`int & Unknown & ~Any` is supposed to be gradual equivalent to `int & Any & ~Unknown`; I did mention this in [a previous comment](https://github.com/astral-sh/ruff/pull/15194#discussion_r1902163652). Since I'm not sure whether such types can exist, I took the safe route.

---

_@InSyncWithFoo reviewed on 2025-01-08 10:05_

---

_@InSyncWithFoo reviewed on 2025-01-08 10:06_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 10:06_

True. I just thought its name should reflect the fact that the order of iteration is depended upon.

---

_@MichaReiser reviewed on 2025-01-08 10:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 10:08_

The problem is that the order isn't guaranteed. The element ordering may depend on insertion order. The `IntersectionType` `positive` and `negative` sets are ordered (although I don't remember the details of how they're ordered)

---

_@InSyncWithFoo reviewed on 2025-01-08 12:42_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 12:42_

`IntersectionType` uses `OrderedSet`, whose iteration order is the same as insertion order. The comparison in question needs a self-ordered set type.

I have so far been unsuccessful in finding such a type. Any suggestions?

---

_@MichaReiser reviewed on 2025-01-08 12:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 12:55_

You could collect the types into a vec and then order them. I'm just not sure why the order is important. I suspect that we instead have to implement an `O(n^2)` algorithm that, for every type in `a`, searches a type in `b` to which it is equivalent, disregarding ordering entirely. But I'm not sure if doing so is too naive. 

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 13:04_

Indeed, this was done to avoid $$O(n^2)$$ time at the cost of a few $$O(n)$$ iterations and allocations. However, even for a $$O(n^2)$$ algorithm, I suspect $$O(n)$$ space is still needed to store paired indices.

---

_@InSyncWithFoo reviewed on 2025-01-08 13:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-08 15:45_

`int | Any` is not just `Any`; it is a gradual type with a known lower bound. It is an unknown type that must be at least as large as `int`, whereas plain `Any` is an unknown type that could be a smaller type than `int`. It is thus intentional that we don't simplify `int | Any`, and `int | Any` can be treated differently in the type system than plain `Any`. For instance, given an object of type `int | Any`, it is an error to assign that object to a destination expecting e.g. a `str`, whereas `Any` is assignable to `str`. Using terms from the typing spec, `Any` can materialize to `str`, but `int | Any` cannot; at best it can materialize to `int | str`.

The thing that causes more difficulty here is our choice to not simplify `Any | Unknown`. Because both of these simply represent the dynamic/unknown type, just with different origin. From a type system perspective, they should simplify, but we choose to preserve the information that the type may have come from an explicit `Any` annotation or may have come from a lack of annotation. But we should still ideally treat `Any | Unknown` as equivalent to `Any` and equivalent to `Unknown`.

---

_@carljm reviewed on 2025-01-08 15:45_

---

_@carljm reviewed on 2025-01-08 15:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-01-08 15:47_

> type[Any] != type != type[object]

I'm not sure if this is describing current behavior or correct behavior. Correct behavior would be that `type` == `type[object]`, but both are not equivalent to `type[Any]`. (In our internal representation, `type` would be `Type::Instance(<builtins.type>)` and `type[object]` would be `Type::SubclassOf(<builtins.object>)`, but these are equivalent types and we should treat them as such.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-01-08 15:49_

I understand the reasoning, but I think it is OK for that `assert_type` to pass, until we actually are able to properly differentiate those types. It is more important that we consistently treat `Todo` in the type system as the dynamic type, just with more provenance information.

---

_@carljm reviewed on 2025-01-08 15:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 16:43_

After thinking this over more, I agree with Micha, in that I don't think naive `Type` ordering (even if we were to find/create a self-ordering set implementation) will be able to get us the right results here. The problem is that we can have equivalent "atomic" types that are not represented as the same `Type` enum bit pattern. (Examples already discussed include `Any` and `Unknown`, or `Type::Instance(<builtins.type>)` and `Type::SubclassOf(<builtins.object>)`. So our definition of equivalence for unions and intersections needs to recurse into a full understanding of atomic type equivalence, not be built solely on `Type` enum identity.

Given that we already have this as an unsolved problem with the existing `is_equivalent_to` implementation, I'm OK with punting on this for this PR. (In other words, I think we can, and probably should, separate the problems of "implement `assert_type`" and "fully correct gradual type equivalence" into separate PRs.) In which case, I would suggest just removing the set stuff from this PR, and implementing `is_gradual_equivalent_to` for now without any handling for differently-ordered unions and intersections (just like `is_equivalent_to`), and a `TODO` comment for improving that in a separate PR.

---

_@carljm reviewed on 2025-01-08 16:43_

---

_@AlexWaygood reviewed on 2025-01-08 16:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:561 on 2025-01-08 16:45_

I actually briefly tried looking at the problem of equivalence for differently ordered unions over the Christmas break, and, yeah, it is Not Easy

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:49 on 2025-01-08 16:50_

I see this issue (and all other TODOs that say "infer the second argument as a type expression") as fundamental to the nature of `assert_type`, and thus something we should solve before merging this PR. (In contrast to, say, ironing out all the details of `is_gradual_equivalent_to`, which I think can be left as an exercise for future.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1489 on 2025-01-08 16:54_

Since this isn't a complete handling of differently-ordered unions (and it's not even well-specified how correct it is, since `HashSet` doesn't provide any self-ordering guarantees at all), I think we should just remove it for now, compare the elements in whatever we have them, and leave a `TODO` for handling ordering separately.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1511 on 2025-01-08 16:55_

Same as above, let's remove the `OrderedTypeSet` stuff in favor of a `TODO`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2018 on 2025-01-08 16:57_

This I don't think we should leave as a TODO. This PR should implement a finer-grained version of `takes_type_expression_arguments` that allows us to infer the second argument as a type expression. We'll need this for `typing.cast` as well. (One implementation option that seems reasonably straightforward would be to make `takes_type_expression_arguments` a bitmask u32 instead of a boolean.)

---

_@carljm reviewed on 2025-01-08 16:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:116 on 2025-01-10 00:36_

```suggestion
    # TODO: order-independent union handling in type equivalence
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:140 on 2025-01-10 00:36_

```suggestion
        # TODO: order-independent intersection handling in type equivalence

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:105 on 2025-01-10 00:37_

Can we also add a test showing union equivalence when the elements _are_ actually in the same order? (This one should actually pass now.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:125 on 2025-01-10 00:37_

Same as above, let's also add a test for intersection equivalence when ordering is the same? It doesn't have to highlight ordering, just show two identical unions and that `assert_type` considers them equivalent.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1114 on 2025-01-10 00:39_

What about Todo?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1129 on 2025-01-10 00:40_

What about Todo?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1159 on 2025-01-10 00:42_

```suggestion
            // TODO handle equivalent intersections with items in different order
            (Type::Intersection(first), Type::Intersection(second)) => {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1147 on 2025-01-10 00:42_

```suggestion
            // TODO handle equivalent unions with items in different order
            (Type::Union(first), Type::Union(second)) => {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4748 on 2025-01-10 00:45_

also test `Todo` vs `Any` and `Todo` vs `Unknown`, also all equivalent

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2548 on 2025-01-10 00:51_

I don't think overload matching is the feature that will allow removing this; `TypeForm` and contextual type inference might. But since the current approach is not buggy, just hardcoded, I don't think we need a TODO for that. We could make a note of it, but I'd probably put that in the doc comment for `takes_type_expression_arguments`, not here.
```suggestion
```

---

_@carljm reviewed on 2025-01-10 00:51_

Looks great, thank you! A few nits here and there, but basically this looks just right.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1141 on 2025-01-10 12:15_

Calling `.len(db)` and then `.elements(db)` in quick succession like this means that you're doing two lookups in the Salsa database for each `Tuple` type when you only really need to do one. You can optimise it like this:

```suggestion
            (Type::Tuple(left), Type::Tuple(right)) => {
                let left_elements = left.elements(db);
                let right_elements = right.elements(db);
                left_elements.len() == right_elements.len()
                    && iter::zip(left_elements, right_elements).all(equivalent)
            }
```

---

_@AlexWaygood reviewed on 2025-01-10 12:21_

You need to merge/rebase and fix some compilation failures now that https://github.com/astral-sh/ruff/pull/15386 has landed :-)

---

_@carljm approved on 2025-01-10 16:44_

Awesome, thank you for this, and for managing all the rebasing on top of other refactors that landed in the meantime!

---

_Merged by @carljm on 2025-01-10 16:45_

---

_Closed by @carljm on 2025-01-10 16:45_

---

_Branch deleted on 2025-01-10 16:50_

---
