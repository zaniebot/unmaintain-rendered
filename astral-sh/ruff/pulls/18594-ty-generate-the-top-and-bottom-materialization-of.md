```yaml
number: 18594
title: "[ty] Generate the top and bottom materialization of a type"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/top-materialization
created_at: 2025-06-09T14:40:45Z
updated_at: 2025-06-17T22:35:28Z
url: https://github.com/astral-sh/ruff/pull/18594
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Generate the top and bottom materialization of a type

---

_Pull request opened by @dhruvmanila on 2025-06-09 14:40_

## Summary

This is to support https://github.com/astral-sh/ruff/pull/18607.

This PR adds support for generating the top materialization (or upper bound materialization) and the bottom materialization (or lower bound materialization) of a type. This is the most general and the most specific form of the type which is fully static, respectively.
    
More concretely, `T'`, the top materialization of `T`, is the type `T` with all occurrences
of dynamic type (`Any`, `Unknown`, `@Todo`) replaced as follows:

- In covariant position, it's replaced with `object`
- In contravariant position, it's replaced with `Never`
- In invariant position, it's replaced with an unresolved type variable

(For an invariant position, it should actually be replaced with an existential type, but this is not currently representable in our type system, so we use an unresolved type variable for now instead.)

The bottom materialization is implemented in the same way, except we start out in "contravariant" position.

## Test Plan

Add test cases for various types.


---

_Label `ty` added by @dhruvmanila on 2025-06-09 14:40_

---

_Comment by @github-actions[bot] on 2025-06-09 14:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:641 on 2025-06-10 04:37_

I think this is probably correct and I can remove the TODO.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:678 on 2025-06-10 04:52_

The `BoundSuper` type has the following fields:
* pivot class which can be dynamic but neither `super(Any, ...)` or `super(..., Any)` is a valid call, but it is representable in our type system as this field can be `ClassBase::Dynamic`
* owner kind which can be dynamic as well via `SuperOwnerKind::Dynamic`

The thing I'm unsure of is whether to generate the top materialization for this type or let it pass through as is (which is what this PR does).

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:684 on 2025-06-10 04:53_

I'm exactly sure how to test this but this is implemented by materializing the type of each member of a synthesized protocol kind. cc @AlexWaygood can you help me with this part?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:6102 on 2025-06-10 04:58_

For a type variable, it's only the bounds and constraints that participate in determining whether it's fully static type or not which is why this also uses the same approach.

I've one doubt thought -- should the bounds and constraints inherit the variance from the type var or the surrounding context here? i.e., should we use the `variance` parameter or `self.variance`?

---

_@dhruvmanila reviewed on 2025-06-10 04:59_

---

_Marked ready for review by @dhruvmanila on 2025-06-10 10:03_

---

_Review requested from @carljm by @dhruvmanila on 2025-06-10 10:03_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-06-10 10:03_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-06-10 10:03_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-10 10:03_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-06-10 10:03_

---

_@AlexWaygood reviewed on 2025-06-10 15:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:684 on 2025-06-10 15:49_

This seems okay for now! Something I'm working on this week is implementing proper subtyping logic for protocol instances. That involves taking account of the fact that read-only property members, and method members, on protocols act covariantly; write-only property members act contravariantly; and read/write attribute members on protocols act invariantly.

Because this is all unimplemented currently, I don't think there's any way you can test it properly. But it might make sense to add a TODO to the code here to make sure we add some tests once the complete assignability/subtyping logic is implemented for protocols.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/top_materialization.md`:14 on 2025-06-10 19:31_

```suggestion
TODO For an invariant position, e.g. `list[Any]`, it should be replaced with an existential type
representing "all lists, containing any type". We currently represent this by replacing `Any`
in invariant position with an unresolved type variable.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/top_materialization.md`:36 on 2025-06-10 19:31_

```suggestion
The top (and only) materialization of any fully static type is just itself.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/top_materialization.md`:50 on 2025-06-10 19:38_

```suggestion
We currently treat function literals as fully static types, so they remain unchanged even
though the signature might have `Any` in it. (TODO this is probably not right.)
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/top_materialization.md`:216 on 2025-06-10 19:41_

Nit: I might put this section much further up (like above tuples), because many of the other sections rely on using callable arguments to create a contravariant context.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:630 on 2025-06-10 19:44_

```suggestion
    /// TODO For an invariant position, e.g. `list[Any]`, it should be replaced with an existential
    /// type representing "all lists, containing any type." We currently represent this by
    /// replacing `Any` in invariant position with an unresolved type variable.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:634 on 2025-06-10 19:47_

We should also replace Todo types here

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:678 on 2025-06-10 19:53_

I think this is fine for `BoundSuper`, because it has no subtype relationships that depend on its pivot class or owner.

(This is not true for `FunctionLiteral`, which has a subtype relationship with callable types that depend on its argument and return types, which is why I think the `FunctionLiteral` case needs to be a TODO)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:691 on 2025-06-10 19:54_

Can't we use `UnionType::map` here?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-10 19:57_

This hardcoding doesn't seem right -- it should flip to the opposite of `variance`, right? Consider a negation type used as a `Callable` argument...

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6102 on 2025-06-10 20:03_

I think what you have here is correct. The variance of the typevar itself is relevant to subtyping of the type which is generic over this typevar; it's not relevant to use of the typevar itself.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:368 on 2025-06-10 20:07_

I don't think it's correct to not pass in the outer variance to `Parameter::top_materialization`, or to hardcode contravariance below. Rather, the contravariance of parameters needs to "flip the polarity" of the outer variance. If the outer variance is covariant, it's flipped to contravariant, and vice versa. (If the outer variance is invariant, it stays invariant.)

To test this, I think you could use e.g. the case of a callable type which has a parameter that is itself a callable type.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/subclass_of.rs`:76 on 2025-06-10 20:09_

Doesn't this still need to have a variance parameter? Because the subclass-of type might be itself in a contravariant position, in which case we should be materializing to `Never` instead.

---

_@carljm reviewed on 2025-06-10 20:14_

This is looking good! I think the main thing is we need to ensure nesting is right, by always flipping the polarity of the "outer" variance when we hit a contravariant context, not hardcoding variance.

Another observation here is that the implementation of `top_materialization` and `bottom_materialization` should be identical -- the only difference is whether we start with a variance of "Covariant" (top materialization) or start with a variance of "Contravariant" (bottom materialization). It might make sense to rename the method added in the current PR to just `materialize`, and then have `Type::top_materialization` and `Type::bottom_materialization` just be thin wrappers that call `materialize` with the right initial variance.

---

_@dhruvmanila reviewed on 2025-06-11 03:03_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/type_properties/top_materialization.md`:216 on 2025-06-11 03:03_

Yeah, that makes sense.

---

_@dhruvmanila reviewed on 2025-06-11 03:05_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:691 on 2025-06-11 03:05_

We can, just wasn't aware of it, thank you!

---

_@dhruvmanila reviewed on 2025-06-11 03:31_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-11 03:31_

When should this flip happen? Only when the variance position is contravariant? Or, should it also happen for covariant position? I'm asking because flipping would result in the following example not working:

Without flipping, the top materialization would be:

```py
from ty_extensions import top_materialization, static_assert, is_assignable_to, Not
from typing import Any, Callable

# revealed: (object, Never, /) -> object
reveal_type(top_materialization(Callable[[Not[tuple[Any, int]], Any], Any]))
```

**Note:** I'm not using `Not[Any]` because that's equivalent to `Any`

Because the `tuple` is in a contravariant position due to `Not` and the variance is passed through the tuple which results in `tuple[Never, int]` which is simplified to `Never`, and thus `Not[Never]` would become `object`.

And, that top materialization is then assignable to the original gradual type:
```py
static_assert(
    is_assignable_to(
        Callable[[object, Never], object], Callable[[Not[tuple[Any, int]], Any], Any]
    )
)
```

But, if we incorporated the flipping logic, the `Not` would mean the contravariant position of the parameter would be flipped to covariant. Now, if we pass this as is in the `tuple`, this would result into the top materialized type not being assignable to the gradual type:
```py
static_assert(
    is_assignable_to(
        Callable[[Not[tuple[object, int]], Never], object],
        Callable[[Not[tuple[Any, int]], Any], Any],
    )
)
```

But, if we did flip is back again because `tuple` is in a covariant position (so the covariant becomes contravariant again), it would result in the correct top materialization that was without the flipping logic.

---

_@dhruvmanila reviewed on 2025-06-11 03:41_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:684 on 2025-06-11 03:41_

Sounds good, thanks! I'll add the TODO.

---

_@carljm reviewed on 2025-06-11 04:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-11 04:32_

On my phone right now, but I think the latter is the correct top materialization, and it should be assignable to the original type (you can see just looking at it that all the Any can materialize in a way that makes it equivalent); if we are not considering it assignable that's a bug somewhere in the assignability logic. 

---

_@carljm reviewed on 2025-06-11 04:36_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-11 04:36_

Thw top materialization should always be the largest possible type we can materialize to. That means making the parameters of a Callable type as small as possible. Which means making the type inside a Not inside the parameter as large as possible. So that inner type should be `tuple[object, int]`, wrapped in a Not, wrapped in Callable. 

"Flipping" means that when we enter a covariant context, the variance stays whatever it was, when we enter a contravariant context, its polarity flips from whatever it was to the opposite: covariant becomes contravariant, contravariant becomes covariant again.

---

_@dhruvmanila reviewed on 2025-06-11 04:43_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-11 04:43_

Understood, thanks! I'll look into the assignability issue, but quickly playing around with it, it seems to be related to negating tuples:

```py
from ty_extensions import static_assert, is_assignable_to, Not
from typing import Any, Never

static_assert(is_assignable_to(tuple[object, int], tuple[Any, int]))
# This is False but should be True
static_assert(is_assignable_to(Not[tuple[Any, int]], Not[tuple[object, int]]))
```

---

_@carljm reviewed on 2025-06-11 04:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-11 04:43_

We should probably add a property test that the top (and bottom) materialization of every type is assignable to that type. 

---

_@dhruvmanila reviewed on 2025-06-11 04:44_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-11 04:44_

I have the property test in my todo list :)

But, I think that won't work completely because of the todo around invariant position?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/subclass_of.rs`:76 on 2025-06-11 05:53_

Can you give me an example? I'm not sure I follow here.

Do you mean that the top materialization of `(type[Any], /) -> type[Any]` should be `(type[Never], / -> type[object]` and not `(type[object], /) -> type[object]`?

---

_@dhruvmanila reviewed on 2025-06-11 05:53_

---

_@dhruvmanila reviewed on 2025-06-11 08:02_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:678 on 2025-06-11 08:02_

> (This is not true for `FunctionLiteral`, which has a subtype relationship with callable types that depend on its argument and return types, which is why I think the `FunctionLiteral` case needs to be a TODO)

I see, I think it would be the same for `BoundMethod` as well then.

---

_Renamed from "[ty] Generate the top materialization of a type" to "[ty] Generate the top and bottom materialization of a type" by @dhruvmanila on 2025-06-11 08:53_

---

_@carljm reviewed on 2025-06-11 13:42_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/subclass_of.rs`:76 on 2025-06-11 13:42_

Yes, exactly!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:368 on 2025-06-11 13:57_

I did this and added tests as well.

---

_@dhruvmanila reviewed on 2025-06-11 13:57_

---

_Comment by @dhruvmanila on 2025-06-11 14:00_

### Post review changes

* Add `top_materialization`, `bottom_materialization`, and a common `materialize` methods on `Type`
* Add `TypeVarVariance::flip` method that flips the polarity of the variance
* Apply the flip in negation type and callable parameter type
* Update `SubclassOf` to use variance and return `Never` for when contravariant and `type` for other variance
* Expanded the test suite to check `bottom_materialization` as well
* Add property tests, it's failing and I've a couple of questions regarding that I intend to ask in our 1:1

**Edit:**
* Change `SubclassOf` to use `T: type` when it's used in an invariant position

---

_@AlexWaygood reviewed on 2025-06-11 14:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:368 on 2025-06-11 14:01_

> "flip the polarity"

you mean [reverse the polarity](https://www.youtube.com/watch?v=QDaCMhKPGys&ab_channel=DWSupercuts), surely? ;)

---

_Review requested from @carljm by @dhruvmanila on 2025-06-11 16:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:308 on 2025-06-11 16:49_

I suppose we could test that it is a subtype of `type[object]` to validate this?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:332 on 2025-06-11 16:49_

Here also I think we could use subtype checks to more clearly confirm what the materialized bounds/constraints are?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:177 on 2025-06-11 17:47_

I think any time we have a `materialize` method that doesn't accept `variance` as a parameter, it means we have a bug where we won't handle nested types correctly. And I think this is no exception. (I'll comment in more detail below.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:234 on 2025-06-11 17:48_

And same here, we need to be passing variance through from the outer context.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:84 on 2025-06-11 17:48_

And same here, need to be passing through the outer variance

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:324 on 2025-06-11 17:49_

I think we should have a TODO comment here

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:367 on 2025-06-11 17:52_

This method need to accept `variance`, and then possibly flip the outer variance according to `typevar.variance(db)`, not just hardcode directly to `typevar.variance(db)`. Otherwise we get a case like this wrong:

```py
from typing import TypeVar, Any, reveal_type, Generic, Callable

U = TypeVar("U", covariant=True)

class C(Generic[U]):
    pass

from ty_extensions import top_materialization

# should reveal `(C[Never], /) -> None`
# currently reveals `(C[object], /) -> None`
reveal_type(top_materialization(Callable[[C[Any]], None]))
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1344 on 2025-06-11 17:53_

Probably doesn't matter, but we could do this flip just once on line 368 instead of separately for each parameter?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/subclass_of.rs`:90 on 2025-06-11 17:55_

On second look, I think we should use a name other than `T` for these synthesized typevars; `T` is too commonly used in user code and too likely to cause confusion.

I think we could use the name `type[All]` here?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:649 on 2025-06-11 17:56_

Similar to below, I think we should use a more distinctive name than `T` here. I think `All` is a reasonable choice, since this typevar really should represent "all possible types".

---

_@carljm reviewed on 2025-06-11 17:56_

---

_@dhruvmanila reviewed on 2025-06-12 04:36_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:308 on 2025-06-12 04:36_

Discussed on Discord and decided to avoid doing this. For context:

---

Regarding this suggestion for checking the subtyping with regards to `list[type[Any]] -> list[T] (where T: type)`, the subtyping check is giving false that `list[T]` is not a subtype of `list[type[object]]`:

```py
from typing import Any, Generic, TypeVar
from ty_extensions import is_subtype_of, static_assert

T_all = TypeVar("T_all", bound=type)

static_assert(is_subtype_of(list[T_all], list[type[object]]))  # false
static_assert(is_subtype_of(T_all, type[object]))  # true
```

I think that's because the subtype relation between two specialization for an invariant position checks whether the [two types are equivalent](https://github.com/astral-sh/ruff/blob/deee6d9ad7383f5446227c4914d3834c9472084d/crates/ty_python_semantic/src/types/generics.rs#L401-L402) and here it isn't.

---

_@dhruvmanila reviewed on 2025-06-12 04:42_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/generics.rs`:367 on 2025-06-12 04:42_

Good catch, I've updated it. Also, added test cases with all the three variance in a generic context as a callable parameter.

---

_@dhruvmanila reviewed on 2025-06-12 04:42_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/subclass_of.rs`:90 on 2025-06-12 04:42_

I've used `T_all`

---

_@dhruvmanila reviewed on 2025-06-12 04:42_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:649 on 2025-06-12 04:42_

I've used `T_all`

---

_@dhruvmanila reviewed on 2025-06-12 04:44_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/subclass_of.rs`:76 on 2025-06-12 04:44_

Updated this. The materialization happens like so:
* For covariant, `type[Any]` -> `type[object]` (which is `type`)
* For contravariant, `type[Any]` -> `type[Never]` (which is `Never`)
* For invariant, `type[Any]` -> `type[T] (T: type)`

---

_@dhruvmanila reviewed on 2025-06-12 04:56_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:703 on 2025-06-12 04:56_

I added the property tests to flaky because they fail most of the time as it reveals bugs around our subtying/assignability implementation. I'll open separate issues for the things that I've found so far running them.

---

_@dhruvmanila reviewed on 2025-06-12 04:58_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:332 on 2025-06-12 04:58_

Discussed this on Discord but it isn't that useful to check whether the top materialization is a subtype of `object` because all static types are a subtype of `object`. On the other hand, it would be useful to check whether the bottom materialization is a subtype of `Never` i.e., is the bottom materialization of `T: Any` a subtype of `Never` but that's not working due to https://github.com/astral-sh/ty/issues/638.

---

_@dhruvmanila reviewed on 2025-06-12 05:08_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:332 on 2025-06-12 05:08_

I've added the subtyping check for bottom materialization with one being a TODO related to the linked issue.

---

_@carljm approved on 2025-06-12 05:17_

Looks great!

---

_Merged by @dhruvmanila on 2025-06-12 06:36_

---

_Closed by @dhruvmanila on 2025-06-12 06:36_

---

_Branch deleted on 2025-06-12 06:36_

---

_@jelle-openai reviewed on 2025-06-17 22:22_

---

_Review comment by @jelle-openai on `crates/ty_python_semantic/src/types.rs`:695 on 2025-06-17 22:22_

Just snooping around here but is this missing a `Type::SubclassOf` in the return value?

---

_@AlexWaygood reviewed on 2025-06-17 22:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:695 on 2025-06-17 22:27_

nah, the function here delegates to `SubclassOfType::materialize()`, which does work to figure out which `Type` variant it needs to return (it won't always be a `Type::SubclassOf` type due to some eager normalizations we do):

https://github.com/astral-sh/ruff/blob/a2cd6df429a3e76880a48a5eb8816a86c36927ed/crates/ty_python_semantic/src/types/subclass_of.rs#L80-L104

---

_@carljm reviewed on 2025-06-17 22:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:695 on 2025-06-17 22:35_

No, but this is subtle and probably should have a comment. `SubclassOfType::materialize` returns a `Type` for the overall materialization of the subclass-of type, not just for the inner type. The reason for this is that we don't currently have a direct representation for `type[T]` (that is, subclass-of a typevar), but the materialization of `type[Any]` in an invariant position should be `type[T]` where `T` has no upper bound or constraints, and it turns out that this is equivalent to just `T'` where `T': type` -- so that's how we represent this `type[T]` we need to synthesize.

~~I think I did just spot another bug in looking at `SubclassOfType::materialize`, though. That method currently acts as if `type[...]` itself were bivariant (imposed no variance constraints). But in fact it is covariant. So both the `Contravariant` and `Invariant` branches there should be returning the type variable.~~ EDIT: never mind, this is wrong -- I was thinking in terms of collecting constraints in variance inference, but here we are just flipping variance, not collecting constraints. So `type[]` being covariant just means it doesn't change the outer variance context.

---
