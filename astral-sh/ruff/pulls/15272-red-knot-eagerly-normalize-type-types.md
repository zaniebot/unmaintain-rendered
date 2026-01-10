```yaml
number: 15272
title: "[red-knot] Eagerly normalize `type[]` types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/normalize-subclass-of
created_at: 2025-01-05T16:41:04Z
updated_at: 2025-01-07T12:55:33Z
url: https://github.com/astral-sh/ruff/pull/15272
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Eagerly normalize `type[]` types

---

_Pull request opened by @AlexWaygood on 2025-01-05 16:41_

## Summary

This PR refactors `SubclassOfType` so that certain `type[]` types are eagerly normalized in our model when they are constructed:
- `type[object]` is eagerly normalized to `Type::Instance(<builtins.type>)` rather than `Type::SubclassOf(<builtins.object>)`
- `type[<final class>]` is eagerly normalized to `Type::ClassLiteral(<final class>)` rather than `Type::SubclassOf(<final class>)`

This already allows us to remove several special cases that we carve out for `type[object]`, in `Type::is_equivalent_to()` and `Type::is_subtype_of()`. The logic applied in these special cases just now falls out naturally from our handling of instance types. The approach will also make it easier to apply knowledge of `@final` classes to `Type::is_equivalent_to()`, `Type::is_subtype_of()` and `Type::is_disjoint_from()`. Without this refactor, we'd need to carefully remember to check whether the wrapped class was `@final` in every branch for `Type::SubclassOf`; with this change, however, it's no longer necessary to do so. Several TODO comments are therefore also removed as part of this PR.

## Details of implementation

The `SubclassOfType` struct is moved to a new `types` submodule, so that it is impossible to construct instances of the struct from other modules without using the "constructor" function that eagerly normalizes types. The "constructor" function is called `SubclassOfType::from`, and is analogous to our existing methods `UnionType::from_elements` (which can return any variant of `Type`, not just `Type::Union`) and `TupleType::from_elements` (which can return `Type::Never` as well as `Type::Tuple()`). I wasn't really sure what the best name was for the "constructor" function, though -- I initially used `SubclassOfType::new()`, but Clippy (reasonably) complained that `new()` methods are meant to return `Self`. Suggestions are welcome!

## Test Plan

- Existing mdtests tweaked
- New mdtests added
- New unit test added for `Type::is_singleton()`, ensuring that `type[<final class>]` is understood to be a singleton type
- `QUICKCHECK_TESTS=200000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` reveals no new property test failures


---

_Label `red-knot` added by @AlexWaygood on 2025-01-05 16:41_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-05 16:41_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-05 16:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-05 16:41_

---

_Comment by @github-actions[bot] on 2025-01-05 16:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-05 16:52_

---

_Comment by @InSyncWithFoo on 2025-01-05 22:16_

What does this PR say about the problem discovered in #15271?

Currently `report_invalid_assignment()` is defined as:

```rust
pub(super) fn report_invalid_assignment(..., declared_ty: Type, ...) {
    match declared_ty {
        Type::ClassLiteral(ClassLiteralType { class }) => {
            context.report_lint(&INVALID_ASSIGNMENT, node, format_args!(
                    "Implicit shadowing of class `{}`; annotate to make it explicit if this is intentional",
                    class.name(context.db())));
        }
        // ...
    }
}
```

...which seems to rely on the fact that a class definition always results in a `ClassLiteral` type being `declared_ty`.

```py
from types import EllipsisType

class SomeClass: ...
#     ^^^^^^^^^
# `declared_ty`: `ClassLiteral(SomeClass)`

#                                   vvvvvvvvv `inferred_ty`: `InstanceOf(Type)` (irrelevant)
some_variable: type[EllipsisType] = type(...)
#              ^^^^^^^^^^^^^^^^^^
# `declared_ty`: previously `SubclassOf(EllipsisType)`, now `ClassLiteral(EllipsisType)`
```

No diagnostics should be emitted here, as `some_variable` is by no mean a redefinition.

---

_Comment by @AlexWaygood on 2025-01-05 22:59_

> What does this PR say about the problem discovered in #15271?

Yeah, I'll investigate that a bit more tomorrow. My suspicion is that it's a pre-existing issue exposed by this PR, rather than a _regression_ caused by this PR, _per se_. But anyway, it's late here, so a full investigation shall have to wait :-)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:165 on 2025-01-05 23:40_

I think this is probably not acceptable? That is, `Literal[X]` for classes `X` is already a notation we are inventing; I don't think we can round-trip explicit user annotations of `type[X]` as `Literal[X]` in our type display, even when they are equivalent.

We could always display `Literal[X]` as `type[X]` in general. The downside here is that then we are conflating two types that are meaningfully different in how we handle them internally, with identical text representations.

I'm not sure what the alternative is, though.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:894 on 2025-01-06 00:01_

I think this is not correct, and it reveals the downside of the approach chosen in this PR.

Delegating subtyping of literal final classes to simply `type` is too much loss of precision, and it will result in weird/wrong behaviors. For example:

```py
from typing import final

class A: pass

@final
class B(A): pass

a: type[A] = B
```

With the current code in this PR, this assignment wrongly generates an error,  because our test of whether `Literal[B] <: type[A]` (which should be) falls back to testing if `type <: type[A]` (which is not).

With the approach in this PR, I think to preserve the needed precision here, we have to change all the below cases for `Type::SubclassOf(X)` so they are instead explicitly `Type::SubclassOf(X) | Type::ClassLiteral(X)`, and remove this generalized fallback entirely.

It is attractive that with this PR we can avoid special cases for `Type::SubclassOf` with a final class. But on the flip side, it means that we need to write explicit handling for `Type::ClassLiteral` where otherwise it could have delegated to `Type::SubclassOf`. (This may still be a good trade-off!)

(Let's add a test for the above example. It could just be a new unit test for `is_subtype_of`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:918 on 2025-01-06 00:05_

Why is this change necessary / an improvement?

---

_@carljm reviewed on 2025-01-06 00:10_

Haven't finished reviewing, but have to head out, so submitting what I have so far.

---

_@AlexWaygood reviewed on 2025-01-06 00:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:918 on 2025-01-06 00:16_

The existing `match` branch no longer works with this PR because the inner data wrapped by `SubclassOfType` is now private, so we can't simply unpack it as part of the `match` branch. The inner wrapped data must be private if we want to enforce that the eagerly normalising constructor is always used.

But this branch doesn't have to use a nested `match`; it could also be something like `subclass_of_ty.subclass_of().into_class().is_some_and(...)`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:165 on 2025-01-06 13:51_

A few points.

Firstly, we're far from unique in inventing our own notation for type display in some cases and using it in the output of `reveal_type`. E.g. take a look at the mypy output [here](https://mypy-play.net/?mypy=latest&python=3.12&gist=298fa4178255da17b792b3954bf583e4). There have been a couple of complaints about some of the stranger symbols in mypy's `reveal_type` output being confusing, but I don't think it's really been a major issue at all. I can only remember [one issue](https://github.com/python/mypy/issues/11519) about it, and it's not highly upvoted.

Secondly, we're also far from unique in eagerly simplifying types that we see in user annotations and using the simplified types in our type display. We already do this elsewhere when we simplify unions (removing duplicate elements and elements that are subtypes of each other); [so does pyright, to some extent](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgCYAiSAxjADRQByAhhGQQK4IA2xAUL1S5MAzsKgAxMGAAUMTjwDaqOphQwAugEoAXFAB0B-gA8oAXglTp0gIy0ATJs28QxAG7EmXAPrwExaUZOAkKiUABCTCDSzKykHNzEOrxQKVBGusrJqXC6wjAg-HBm4ZE29ABEwFLlTi7unj4k0nBBgiJiEQBe0kR%2BFNQwSalpGWpZKTlQeQXObh7evv5d0gDe5UblugAsdhVwm1Dl%2BwC%2BjvykxMBQwAGj%2BAA%2BUABGUlxQj8pDqXXzjX4BTiAA).

Notwithstanding those two points, I have been thinking for a while that our current display for class-literal and function-literal types is a little confusing for users. It looks _too close_ to something that would be valid in a type annotation in user code, and users will probably think that we're claiming that `Literal[SomeClass]` is valid in a user type annotation. It'll be confusing for them when we then reject them using `Literal[SomeClass]` in their own code.

Prior to this PR, I was wondering about `Literal[<class 'SomeClass'>]` and `Literal[<function 'some_function'>]` as an alternative display for class-literal and function-literal types. But your comments here make me wonder if we could also consider `type[SomeClass(final=True)]` or `type[SomeClass(@final=True)]` for class-literal types

---

_@AlexWaygood reviewed on 2025-01-06 13:51_

---

_@AlexWaygood reviewed on 2025-01-06 13:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:894 on 2025-01-06 13:54_

Argh, great catch! Fixing this could indeed be annoying enough that we decide not to pursue the approach here. Let's see.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:894 on 2025-01-06 14:22_

Actually, fixing this bug only required adding one extra branch to `Type::is_subtype_of()`. I think that still makes the tradeoff worthwhile.

I added a unit test for the bug in https://github.com/astral-sh/ruff/pull/15272/commits/72f8cece68cbd5d8c3b139a52c89abbb44a9c3b4, and confirmed that it failed on this PR branch prior to https://github.com/astral-sh/ruff/pull/15272/commits/72f8cece68cbd5d8c3b139a52c89abbb44a9c3b4!

---

_@AlexWaygood reviewed on 2025-01-06 14:22_

---

_@carljm reviewed on 2025-01-06 18:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:165 on 2025-01-06 18:00_

Yes, I agree that both displaying our own notation for some types, and simplifying user-provided types, are things we can do. It just feels a bit like crossing another line to _combine_ those two things in such a way that we eagerly replace a spellable user-provided type with a non-spellable type display. But maybe this isn't actually a problem in practice. I'm OK with deferring that question from this PR, and at some point following up more holistically on type display. There are a lot of interesting questions here, including how we balance conciseness/readability vs clarity (to first time readers without needing to refer to docs) in the notations we choose. I won't get too deep into the details here, I'll just say that a) I kind of like the idea of representing `ClassLiteral` types in some way using `type[]` notation, since they are similar, and b) I'm not sure we should use the word "final" in that display, because I think it's too confusing in cases where we have a `ClassLiteral` type but for a non-final class.

---

_@carljm reviewed on 2025-01-06 18:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:894 on 2025-01-06 18:01_

Great, will take a look at the rest of the PR now!

---

_Comment by @carljm on 2025-01-06 18:13_

> a pre-existing issue exposed by this PR

Yes, it looks that way to me (though I haven't investigated it thoroughly). It looks like we probably need to actually check the reaching definitions, rather than just the declared type, in deciding whether to emit the special class-shadowing diagnostic. (Or we could just abandon that special case and just decide that the regular invalid-assignment diagnostic is adequate.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:920 on 2025-01-06 18:16_

Is it worth pulling this into an `SubclassOfType::is_instance_of` method?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1394 on 2025-01-06 18:22_

I think we might thank ourselves for this in the future?
```suggestion
            // We eagerly transform `SubclassOf` to `ClassLiteral` for final types, so `SubclassOf` is never a singleton.
            Type::SubclassOf(..) => false,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/subclass_of.rs`:3 on 2025-01-06 18:26_

This comment sort of implies a type as being made up of types, rather than objects:
```suggestion
/// A type that represents `type[C]`, i.e. the class object `C` and class objects that are subclasses of `C`.
```

---

_@carljm approved on 2025-01-06 18:28_

Looks good! I think it makes sense to go with this direction.

---

_@AlexWaygood reviewed on 2025-01-07 11:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:920 on 2025-01-07 11:42_

I'm reluctant to add a `SubclassOfType::is_instance_of` method _exactly_, because I think there's an important-yet-subtle distinction that needs to be made here: the subtype-of relationship is a relationship between _types_ (is this instance type a subtype of the other instance type? is this subclass-of type a subtype of that instance type?) but the subclass-of and instance-of relationships are relationships between _runtime objects_ rather than _types_ (is this class object an instance of that class object? is this instance an instance of that class object? is this class object a subclass of that class object?). So we can ask the question "are all _inhabitants of_ <some `SubclassOfType`> instances of <class `X`>?", but I don't think we should allow ourselves to ask the question "is <some `SubclassOfType`> an instance of <class `X`>?"; it blurs the distinction between the concepts in an unfortunate way.

I suppose I already blurred the line recently by adding the `KnownInstanceType::is_instance_of()` method:

https://github.com/astral-sh/ruff/blob/0dc00e63f444985fc55c327332602c876a2ffd60/crates/red_knot_python_semantic/src/types.rs#L2769-L2771

it felt okay because this type in particular only has exactly one inhabitant at runtime, so in this particular case the distinction between "the type" and "all inhabitants of the type" is really a distinction without a difference. Maybe it was still a mistake, though; maybe we should get rid of that method.

I'd be open to adding APIs such as `SubclassOfType::inhabitants_are_instances_of()` and `InstanceType::inhabitants_are_instances_of()`, but the longer names make it slightly more unwieldy. For now I'll leave it, but I'm happy to tackle it in a follow-up if you think it would be worth it!

---

_Merged by @AlexWaygood on 2025-01-07 12:53_

---

_Closed by @AlexWaygood on 2025-01-07 12:53_

---

_Branch deleted on 2025-01-07 12:53_

---
