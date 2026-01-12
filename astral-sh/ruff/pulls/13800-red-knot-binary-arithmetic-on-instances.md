```yaml
number: 13800
title: "[red-knot] binary arithmetic on instances"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: binary-arithmetic
created_at: 2024-10-17T18:33:41Z
updated_at: 2024-10-26T02:59:41Z
url: https://github.com/astral-sh/ruff/pull/13800
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] binary arithmetic on instances

---

_@carljm_

This implements the (subtle!) algorithm that Python uses for binary arithmetic operations between two instances. The full algorithm is described well in [this blog post](https://snarky.ca/unravelling-binary-arithmetic-operations-in-python/) by Brett Cannon; the official Python docs for this are [here](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types).

There are still many TODOs in the code and tests. The main remaining tasks in this area are:
- There are many type errors we do not currently catch, and some types that we do not yet infer correctly, because we would need to validate the types of arguments passed to functions.
- We do not yet have any capability to traverse a class's MRO to understand if one class is a subclass of the other

Closes #13590

---

_Label `red-knot` added by @carljm on 2024-10-17 18:33_

---

_Comment by @AlexWaygood on 2024-10-18 16:46_

Okay, this should now be passing all tests!

---

_Marked ready for review by @AlexWaygood on 2024-10-18 16:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-18 16:46_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-10-18 16:46_

---

_@AlexWaygood reviewed on 2024-10-18 16:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2520 on 2024-10-18 16:54_

@carljm -- currently I just returned an `Option`, as it's simpler and we don't have a branch dealing with `Union`s yet. When we implement `Union`s I figured we could refactor it to return a `Result` rather than an `Option`, but this seemed more minimal and easier to understand for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:183 on 2024-10-18 16:54_

Is the intention just that this is an implicit assertion that no error is raised? Should there be a `reveal_type` here?

I assume this will be a TODO since we don't do argument checking yet?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:443 on 2024-10-18 16:58_

There are unit tests for this method below; can we add a unit test for this case? (Will be easiest if we can find some stdlib types with an inheritance relationship.)

---

_Comment by @github-actions[bot] on 2024-10-18 17:04_

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

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2651 on 2024-10-18 17:17_

May as well do this for `LiteralString` as well?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 17:25_

I don't think we should use `is_assignable_to` here, or even `is_subtype_of`; I think we should go straight to an `is_subclass_of` method implemented directly on `ClassType`.

Functionally it shouldn't matter currently, though it will be more efficient to bypass unnecessary layers of abstraction. But in the future with generic classes, I _think_ this decision should be based purely on the subclass relationship of the classes, without considering generic parameters at all. E.g. if class `Bar[T]` inherits class `Foo[T]` and overrides `__radd__`, then I think we should prioritize `Bar.__radd__` even if we have `Bar[X]` and `Foo[Y]`, where `Bar[X]` is not a subtype of `Foo[Y]` (for variance reasons or because X and Y have no subtype relationship), because the runtime won't be considering generic parameters (they don't even exist at runtime.)

Of course our eventual determination of which method is _supported_ will depend on the type annotations on the dunder methods, which may involve `T`, and that could influence our eventual determination of the result of the operation. But I don't think `T` should influence the decision of which dunder method to try first. Which means I think this should be based purely on the subclass relationship of two classes, without considering anything else handled in `is_assignable_to` or `is_subtype_of`.

---

_@carljm reviewed on 2024-10-18 17:36_

Looks great! Just a few comments. I would approve-and-comment but I can't approve "my" pull request 😆 

---

_@AlexWaygood reviewed on 2024-10-18 17:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 17:38_

what if a class has `Any`/`Unknown` in its MRO

---

_@AlexWaygood reviewed on 2024-10-18 17:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 17:39_

> what if a class has `Any`/`Unknown` in its MRO

(I should add a test for this. Meant to do so; forgot!)

---

_@AlexWaygood reviewed on 2024-10-18 17:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 17:46_

> But in the future with generic classes, I _think_ this decision should be based purely on the subclass relationship of the classes, without considering generic parameters at all. E.g. if class `Bar[T]` inherits class `Foo[T]` and overrides `__radd__`, then I think we should prioritize `Bar.__radd__` even if we have `Bar[X]` and `Foo[Y]`, where `Bar[X]` is not a subtype of `Foo[Y]` (for variance reasons or because X and Y have no subtype relationship), because the runtime won't be considering generic parameters (they don't even exist at runtime.)

Hmm, this is a really good point, though. My current approach is definitely wrong, whatever the case.

---

_@carljm reviewed on 2024-10-18 17:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 17:47_

I think that the hypothetical `ClassType::is_subclass_of` should handle the special case of `Any` or `Unknown` in the MRO. (And unions too, if we support them in bases, though this might be handled via checking in multiple MROs instead.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 17:53_

Mypy and pyright both reveal `int` here:

```py
from typing import Any

class X:
    def __add__(self, other: object) -> int:
        return 42

class Y(Any):
    pass

reveal_type(X() + Y())
```

I don't actually know what the right answer is... do we need to consider `Y` as a class that "could inherit from `X` and override `__radd__`"? I think we _do_, because `Any` represents an unknown set of objects, and that unknown set of objects could include the `X` class. So a better answer than what mypy/pyright give might be `int | Any` (accounting for the fact that it _could_ be a class that inherits from `X`, but it also _could not_)?

---

_@AlexWaygood reviewed on 2024-10-18 17:53_

---

_@AlexWaygood reviewed on 2024-10-18 17:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:183 on 2024-10-18 17:56_

Oops, there should be a `reveal_type` here!

---

_@carljm reviewed on 2024-10-18 18:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2666 on 2024-10-18 18:07_

> Any represents an unknown set of objects, and that unknown set of objects could include the X class

I think it's even slightly more subtle than that; the relevant case here is that Any represents some unknown third class Z which inherits from X and overrides `__radd__` (since we can see that Y itself does not override `__radd__`).

I agree that `int | Any` is the right answer here. It would be `int & Any` if `X` also defined `__radd__` to return `int`.

Getting to this right answer will depend on several features, one of which is `is_subclass_of` supporting finding `Any` in the MRO, and the other is `class_member` and `inherited_class_member` supporting `Any` in the MRO.

---

_Comment by @AlexWaygood on 2024-10-18 18:50_

Thanks @carljm! Pushed a commit addressing your review -- the only things you might want to glance at again are my implementation for `is_subclass_of()` and the test snippet I added for inheriting from `Unknown`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1444 on 2024-10-18 19:33_

Sorry I wasn't more clear about this: I think this method should take `other: ClassType` instead of `other: Type`, and attempt to only answer the much narrower question, "given two ClassType, does the one inherit the other". This would avoid a lot of the TODO questions below; I think more abstracted methods calling this should be responsible for handling e.g. unions and intersections. (For example, that is already the case in this PR, and would also be the case when e.g. `is_subtype_of` wants to make use of this method -- which it probably should already in this PR.)

That just leaves the question of how to handle `Any` in the MRO. I think for now (until we have a proper MRO and a clarified data model for what types can occur in the MRO) this method can just return `true` if it finds `Any/Unknown/Todo` or a matching `ClassType` in bases, otherwise `false`.

I think a doc comment clarifying the semantics of this method would be useful.
```suggestion
    /// Does this class inherit the given `other` class?
    pub fn is_subclass_of(self, db: &'db dyn Db, other: ClassType) -> bool {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2520 on 2024-10-18 19:35_

I would probably have just gone for `Result` from the start, if we know that's where we'll end up, but I don't feel strongly, or that you need to change it. We can definitely just do it in the PR that adds union or tuple support.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:445 on 2024-10-18 19:35_

Why not use `is_subclass_of` here?

(I guess maybe this seemed less like an obvious choice when `is_subclass_of` took an arbitrary `Type`.)

---

_@carljm reviewed on 2024-10-18 19:36_

---

_@carljm reviewed on 2024-10-18 22:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1444 on 2024-10-18 22:23_

Since this PR is a joint work anyway, I went ahead and pushed what I was thinking here, so the review ball is now in your court :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1596 on 2024-10-18 22:33_

I know you don't like default cases in match statements, but since this is all subject to change soon anyway when we add proper MRO support, I didn't feel it was worth listing out every `Type` variant here.

---

_@carljm reviewed on 2024-10-18 22:33_

---

_@carljm reviewed on 2024-10-18 22:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1595 on 2024-10-18 22:35_

This means that if we want binary operations to actually consider the possibility that an `Any` inheriting type might be a subtype when found on the RHS, we'll have to do something a bit more complicated there than just `is_subclass_of`; there might need to be another `may_be_subclass_of` method that does respect `Any/Unknown`?

To be honest, though, I feel like the behavior of binary operations when the RHS inherits `Any` is an edge case that may never practically matter to anyone, so I don't think we should spend more time on it now.

---

_Review requested from @AlexWaygood by @carljm on 2024-10-18 22:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:179 on 2024-10-19 15:02_

```suggestion
without raising an exception -- it will simply return `NotImplemented`,
allowing the runtime to try the `__radd__` method of the right-hand operand
as well.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:85 on 2024-10-19 15:04_

```suggestion
## Returning a different type
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:121 on 2024-10-19 15:05_

```suggestion
reveal_type(A() + B())  # revealed:  int

# Edge case: C is a subtype of C, *but* if the two sides are of *equal* types,
# the lhs *still* takes precedence
class C:
    def __add__(self, other: C) -> int: return 42
    def __radd__(self, other: C) -> str: return "foo"

reveal_type(C() + C())  # revealed: int
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:173 on 2024-10-19 15:06_

```suggestion
returns `NotImplemented` to signal failure.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:225 on 2024-10-19 15:07_

```suggestion
# TODO should be complex, need to check arg type and fall back to `rhs.__radd__`
reveal_type(3.14 + 3j)  # revealed: float

# TODO should be float, need to check arg type and fall back to `rhs.__radd__`
reveal_type(42 + 4.2)  # revealed: int

# TODO should be complex, need to check arg type and fall back to `rhs.__radd__`
reveal_type(3 + 3j)  # revealed: int
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:239 on 2024-10-19 15:07_

```suggestion
# TODO should be float, need to check arg type and fall back to `rhs.__radd__`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:310 on 2024-10-19 15:08_

```suggestion
### Dunder as instance attribute
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:312 on 2024-10-19 15:08_

```suggestion
The magic method must exist on the class, not just on the instance:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1595 on 2024-10-19 15:10_

> To be honest, though, I feel like the behavior of binary operations when the RHS inherits `Any` is an edge case that may never practically matter to anyone, so I don't think we should spend more time on it now.

Agreed, I'm definitely fine with what we have for now, the TODO comments are pretty clear!

---

_@AlexWaygood approved on 2024-10-19 15:13_

Looks great! A few minor nits, mostly regarding wording in the docs around the test. I don't think any are controversial; I'll push my suggested edits to the branch and merge!

---

_@AlexWaygood reviewed on 2024-10-19 15:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1444 on 2024-10-19 15:14_

Ah yes, you did indeed lead me astray here from your comment in https://github.com/astral-sh/ruff/pull/13800#discussion_r1806833944, when you said this 😄

> I think that the hypothetical `ClassType::is_subclass_of` should handle the special case of `Any` or `Unknown` in the MRO. (And unions too, if we support them in bases, though this might be handled via checking in multiple MROs instead.)

but no harm done!

---

_Merged by @AlexWaygood on 2024-10-19 15:22_

---

_Closed by @AlexWaygood on 2024-10-19 15:22_

---

_Branch deleted on 2024-10-19 15:22_

---

_@carljm reviewed on 2024-10-19 16:12_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1444 on 2024-10-19 16:12_

I think there was a misunderstanding between "handle Any/union in the MRO", which is what I said, and "handle Any/Union" in the `other` type", which is what you implemented. I didn't mean that comment to imply handling anything else in `other`. I don't think the version you implemented did handle Any or Union found in the MRO?

But my comment was wrong regardless, because in the end I realized I don't think we should handle Any in the MRO here. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1444 on 2024-10-19 16:14_

ah, yeah, good points all round!

---

_@AlexWaygood reviewed on 2024-10-19 16:14_

---

_Comment by @brettcannon on 2024-10-26 02:59_

I got messaged by @AlexWaygood on Discord about a potential error in my blog post's code, but since I never heard back from Alex after I answered the question I wanted to make sure you all knew that the canonical code for my syntactic sugar posts can be found at https://github.com/brettcannon/desugar/ . That repo has tests to verify the unravelling matches actual semantics and the code hasn't been modified for blog post readability, i.e. it shouldn't very bugs. 😅

---
