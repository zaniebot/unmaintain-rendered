```yaml
number: 13903
title: "[red-knot] Type inference for comparisons between arbitrary instances"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-implement-comparison-instances
created_at: 2024-10-24T10:04:41Z
updated_at: 2024-10-26T18:29:00Z
url: https://github.com/astral-sh/ruff/pull/13903
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Type inference for comparisons between arbitrary instances

---

_@cake-monotone_

# Summary
 - Implemented "rich comparison" for instance comparisons.
 -  Implemented "membership test" for instance comparisons.

Closes astral-sh/ruff#13872

# Test Plan

mdtest included!
- `comparisons/instances/rich_comparison.md`
- `comparisons/instances/membership_test.md`
- (Resolved some TODO in other md tests)

---

_Review requested from @carljm by @cake-monotone on 2024-10-24 10:04_

---

_Review requested from @MichaReiser by @cake-monotone on 2024-10-24 10:04_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-10-24 10:04_

---

_Converted to draft by @cake-monotone on 2024-10-24 10:04_

---

_Comment by @cake-monotone on 2024-10-24 10:14_

Next task topics?

- We should infer Never from the membership test for some special cases.

from the old-style iteration protocol [doc](https://docs.python.org/3/reference/expressions.html#membership-test-details)

```py
class A:
    def __getitem__(self, key: str) -> str:
        return key

reveal_type("never" in A())  # revealed: Never
```

---

_@AlexWaygood reviewed on 2024-10-24 10:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3841 on 2024-10-24 10:16_

can some of this be simplified by using our pre-existing `Type::iterate()` method?

---

_Comment by @AlexWaygood on 2024-10-24 10:16_

Overall this looks fantastic; you're definitely on the right track. Thanks!!

---

_Comment by @github-actions[bot] on 2024-10-24 10:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances.md`:42 on 2024-10-24 18:34_

Commenting our tests is useful to future readers.

```suggestion
### Wrong Return Type

Python coerces the results of containment checks to bool, even if `__contains__` returns a non-bool:

```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances.md`:8 on 2024-10-24 18:40_

I think we could have a more informative test name here :)

And some prose documentation of the behavior we are testing can really help future readers also.

I would split this into three separate "tests" each with their own separate code block. The first test could be named "Implements __contains__" with the prose text:

> Classes can support membership tests by implementing the `__contains__` method:

And then the second test can be named "Implements __iter__" with the prose:

> Classes that don't implement `__contains__`, but do implement `__iter__`, also support containment checks; the needle will be sought in their iterated items:

And the third test can be named "Implements __getitem__" with the text

> The final fallback is to implement `__getitem__` for integer keys: Python will call it with 0, 1, 2... until it either finds the needle (returning `True` for the membership test) or `__getitem__` raises `IndexError`, which is silenced and returns `False` for the membership test.


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances.md`:53 on 2024-10-24 18:42_

```suggestion
### Wrong Fallback

If `__contains__` is implemented, checking membership of a type it doesn't accept is an error; it doesn't result in a fallback to `__iter__` or `__getitem__`:

```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances.md`:91 on 2024-10-24 18:44_

```suggestion
### Old protocol Unsupported

If `__getitem__` is implemented but does not accept integer arguments, then membership test is not supported.

```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances.md`:1 on 2024-10-24 18:46_

It looks like you also added an empty file `instances/membership_test.md` in this PR; did you intend to move these tests to that file? I think that would make sense.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:20 on 2024-10-24 18:48_

This test could also help verify the right dunder is being used for each type, if each dunder method returned a different type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:71 on 2024-10-24 18:51_

Again here, returning different types could help verify we are using the right reflected method for each comparison.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:80 on 2024-10-24 18:57_

And similar here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3822 on 2024-10-24 19:14_

I think we need some tests with `__contains__` methods returning e.g. `Literal[True]` or `Literal[False]` to show this logic is correct?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/tests/mdtest.rs`:8 on 2024-10-24 19:15_

This doesn't look like a change we want to land :)

---

_@carljm requested changes on 2024-10-24 19:15_

Thank you, this is excellent!!

---

_Comment by @carljm on 2024-10-24 19:16_

Oh sorry for requesting changes, I somehow failed to notice this PR was still in draft state

---

_Comment by @cake-monotone on 2024-10-25 09:55_

Although this is a draft, the reviews have been helpful! Thank you!!
Next time, I'll make sure to clearly label the title with WIP. 🥲🥲

And, I’ve separated the commits so that you can compare them with the initial draft version :)

---

_@cake-monotone reviewed on 2024-10-25 10:03_

It seems that inferring Literal types based on only rich comparison and typeshed may not be feasible.

The `__eq__` method in both builtin.str and builtin.int has an argument type of object.

https://github.com/astral-sh/ruff/blob/main/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L590
https://github.com/astral-sh/ruff/blob/main/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L319

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:95 on 2024-10-25 10:04_

https://github.com/astral-sh/ruff/pull/13903/files#diff-eff9f162b9d83ddfe43d3f6b28ed022767bc58d12d7ccb0c95d985e827471362R91-R95

It seems that inferring Literal types based on only rich comparison and typeshed may not be feasible.

The `__eq__` method in both builtin.str and builtin.int has an argument type of object.

https://github.com/astral-sh/ruff/blob/main/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L590
https://github.com/astral-sh/ruff/blob/main/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L319

---

_@cake-monotone reviewed on 2024-10-25 10:04_

---

_@cake-monotone reviewed on 2024-10-25 10:08_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:197 on 2024-10-25 10:08_

Same issue as mentioned in the previous comment

---

_Marked ready for review by @cake-monotone on 2024-10-25 10:13_

---

_Review requested from @carljm by @cake-monotone on 2024-10-25 10:13_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-10-25 10:13_

---

_@AlexWaygood reviewed on 2024-10-25 10:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:95 on 2024-10-25 10:17_

> The `__eq__` method in both builtin.str and builtin.int has an argument type of object.

Right. The convention when it comes to annotating parameters on `__eq__` and `__ne__` is different from the other four rich-comparison methods. As far as I'm aware, this is unspecified.

The convention for nearly all rich-comparison and binary-arithmetic dunders is to annotate them in such a way that any call that would result in `NotImplemented` being returned would lead to the type checker emitting an error. So although this "works" at runtime (no error is raised, just `NotImplemented` returned), [typeshed's annotations disallow it](https://github.com/python/typeshed/blob/b954cd705cb5af5a96bcbf439e2268d52da47b48/stdlib/builtins.pyi#L325):

```py
>>> (2).__lt__(2.0)
NotImplemented
```

But for `__eq__` and `__ne__`, the convention is to annotate parameters such that any calls that would succeed at runtime are allowed, even if those successful calls would return `NotImplemented`. I can think of two good reasons for this:

1. `__eq__` and `__ne__` really are different from all the others: unlike the other rich-comparison operators and the binary-arithmetic operators, `x == y` will always result in a `bool` (never `TypeError`), even if `type(x).__eq__(y)` returns `NotImplemented` _and_ `type(y).__eq__(x)` returns `NotImplemented`.
2. It's useful for type checkers to enforce the Liskov Substitution Principle for equality. If a class raises an exception during an equality check, that seriously violates some assumptions that are baked in across the language, standard library, and many third-party libraries. It's useful to be able to distinguish "this `__eq__` method returns `NotImplemented` if you pass it a `float`" and "this `__eq__` method raises `TypeError` or `AttributeError` if you pass it a `float`", because the first is fine but the second is very bad indeed. With the current convention, type checkers will warn you if you say that an `__eq__` method has to accept a non-`object` type, because that's an incompatible override of `object.__eq__` (they're able to detect the Liskov violation).

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:16 on 2024-10-25 11:03_

I think in general, we should actually return the thing we say we're returning in our `mdtest` snippets. This will make us robust to future improvements in our inference and checking: at some point, we'll want to start complaining if a method says it returns `bool` but actually returns `None`; tests like this will break if the body of the method is just `...`:

```suggestion
class A:
    def __contains__(self, item: str) -> bool:
        return True
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:31 on 2024-10-25 11:04_

`Iterator` is undefined here. Also, we don't understand generic types yet, so we can't use `Iterator` yet 😆

You'll want to do something like:

```suggestion
class StringIterator:
    def __next__(self) -> str:
        return "foo"

class A:
    def __iter__(self) -> StringIterator:
        return StringIterator()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:45 on 2024-10-25 11:05_

```suggestion
    def __getitem__(self, key: int) -> str:
        return "foo"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:59 on 2024-10-25 11:05_

```suggestion
    def __contains__(self, item: str) -> str:
        return "foo"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:74 on 2024-10-25 11:06_

```suggestion
from typing import Literal

class AlwaysTrue:
    def __contains__(self, item: int) -> Literal[1]:
        return 1

class AlwaysFalse:
    def __contains__(self, item: int) -> Literal[""]:
        return ""
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:97 on 2024-10-25 11:06_

- `Iterator` is undefined here
- Please add bodies to these methods

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:107 on 2024-10-25 11:13_

- `Iterator` is not defined here
- Please fill in the bodies of these methods

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:120 on 2024-10-25 11:13_

```suggestion
    def __getitem__(self, key: str) -> str:
        return "foo"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:22 on 2024-10-25 11:15_

```suggestion
    def __eq__(self, other: A) -> int:
        return 42

    def __ne__(self, other: A) -> float:
        return 42.0

    def __lt__(self, other: A) -> str:
        return "42"

    def __le__(self, other: A) -> bytes:
        return b"42"

    def __gt__(self, other: A) -> list:
        return [42]

    def __ge__(self, other: A) -> set:
        return {42}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:43 on 2024-10-25 11:15_

```suggestion
    def __eq__(self, other: B) -> int:
        return 42

    def __ne__(self, other: B) -> float:
        return 42.0

    def __lt__(self, other: B) -> str:
        return "42"

    def __le__(self, other: B) -> bytes:
        return b"42"

    def __gt__(self, other: B) -> list:
        return [42]

    def __ge__(self, other: B) -> set:
        return {42}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:67 on 2024-10-25 11:16_

```suggestion
    def __eq__(self, other: A) -> int:
        return 42

    def __ne__(self, other: A) -> float:
        return 42.0

    def __lt__(self, other: A) -> str:
        return "42"

    def __le__(self, other: A) -> bytes:
        return b"42"

    def __gt__(self, other: A) -> list:
        return [42]

    def __ge__(self, other: A) -> set:
        return {42}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:72 on 2024-10-25 11:16_

```suggestion
    def __eq__(self, other: str) -> B:
        return B()

    def __ne__(self, other: str) -> B:
        return B()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:86 on 2024-10-25 11:16_

```suggestion
    def __gt__(self, other: C) -> int:
        return 42

    def __ge__(self, other: C) -> float:
        return 42.0
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:103 on 2024-10-25 11:17_

```suggestion
    def __eq__(self, other: A) -> A:
        return A()

    def __ne__(self, other: A) -> A:
        return A()

    def __lt__(self, other: A) -> A:
        return A()

    def __le__(self, other: A) -> A:
        return A()
 
    def __gt__(self, other: A) -> A:
        return A()
 
    def __ge__(self, other: A) -> A:
        return A()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:111 on 2024-10-25 11:18_

```suggestion
    def __eq__(self, other: A) -> int:
        return 42

    def __ne__(self, other: A) -> float:
        return 42.0

    def __lt__(self, other: A) -> str:
        return "42"

    def __le__(self, other: A) -> bytes:
        return b"42"

    def __gt__(self, other: A) -> list:
        return [42]

    def __ge__(self, other: A) -> set:
        return {42}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:134 on 2024-10-25 11:18_

```suggestion
    def __lt__(self, other: A) -> A:
        return A()

    def __gt__(self, other: A) -> A:
        return A()

class B(A):
    def __lt__(self, other: int) -> B:
        return B()

    def __gt__(self, other: int) -> B:
        return B()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:179 on 2024-10-25 11:19_

```suggestion
    def __eq__(self, other: int) -> A:
        return A()

    def __ne__(self, other: int) -> A:
        return A()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:221 on 2024-10-25 12:04_

```suggestion
def bool_instance() -> bool:
    return True

def int_instance() -> int:
    return 1
```

---

_@AlexWaygood requested changes on 2024-10-25 12:04_

Thanks! This review only reviews your tests ;)

---

_Label `red-knot` added by @MichaReiser on 2024-10-25 12:33_

---

_@cake-monotone reviewed on 2024-10-25 15:24_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:16 on 2024-10-25 15:24_

Thank you so much for taking the time to make all these detailed adjustments—it must have been quite a task!

I have a follow-up question regarding the points you mentioned. Should we also add `from __future__ import annotations`?

```py
from __future__ import annotations  #required?!

class A:
    def foo(self, o: A) -> A:
        return A()
```

---

_@carljm reviewed on 2024-10-25 15:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:16 on 2024-10-25 15:29_

Yes, we should probably add `from __future__ import annotations` where we are relying on these self-annotations. We actually understand them even without it, because we don't currently model the fact that the class body scope runs immediately at class definition time, but probably we should model that correctly in future.

---

_@cake-monotone reviewed on 2024-10-25 15:58_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/infer.rs`:3822 on 2024-10-25 15:58_

I missed commenting earlier! sorry 😢

I think the [new test here](https://github.com/astral-sh/ruff/pull/13903/files#diff-93415f322d4e43d79a593ad9befefe07cf5c53ee929363403cb17f740f5df341R73-R95) should cover the scenario you mentioned!

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-10-25 15:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:147 on 2024-10-25 21:36_

This should also emit an unsupported-operator diagnostic, which we should mention in the TODO
```suggestion
# TODO should emit a diagnostic
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:21 on 2024-10-25 21:38_

After thinking it through (and writing up my thoughts at https://github.com/astral-sh/ruff/issues/13932) I think that we should just always infer `bool` for membership tests, even if we emit a diagnostic because the membership test is invalid. The reasoning is that membership tests in Python _always_ evaluate to `bool` at runtime, so we can have very high confidence that `bool` is the right type here, presuming the membership test itself will be fixed.
```suggestion
# TODO: should emit diagnostic, need to check arg type, will fail
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:122 on 2024-10-25 21:44_

```suggestion
# TODO: should emit diagnostic, need to check arg type, should not fall back to __iter__ or __getitem__
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:134 on 2024-10-25 21:51_

I think this comment is correct, but it could be clearer. If we are talking about equality comparison, all Python objects are comparable, in that equality comparison is not supposed to raise, it should just return `False` for unrelated objects.

Checking e.g. `42 in F()` where `F()` is a container of strings is not an error at runtime; it's just `False`. (Maybe you'd want a type-aware linter to emit a lint diagnostic on this as an always-false test, but I don't think it's a type error.)

This is consistent with the behavior tested above, where e.g. we test that `42 in A()` is `bool` where `A` defines `__getitem__` to return `str`, with no TODO for emitting a diagnostic.

So I think the comment here should clarify this a bit better:
```suggestion
# Always use `__iter__`, regardless of iterated type; there's no NotImplemented
# in this case, so there's no fallback to `__getitem__`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:140 on 2024-10-25 21:51_

```suggestion
If `__getitem__` is implemented but does not accept integer arguments, then membership test is not supported and should emit a diagnostic.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:3 on 2024-10-25 21:53_

General note throughout the mdtests -- our current practice is to hard-wrap these at 80 columns. If your editor doesn't make that easy, or you don't want to do it, it's fine, I can do it before merging as well.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:114 on 2024-10-25 22:19_

These overrides themselves should emit a diagnostic for invalid override of the `object` methods, because they violate Liskov Substitution Principle by accepting only a narrower type than `object`. And according to @AlexWaygood , for this reason these two methods typically are never annotated to accept a narrower type. Which means that equality and inequality comparisons would actually never fall back to RHS in a type checker, even though they could at runtime.

But I still think (with the TODO for the invalid-override diagnostic), it makes sense that we should still implement the same fallback logic for `__eq__` and `__ne__`, in case someone does override them in this way?
```suggestion
    # To override builtins.object.__eq__ and builtins.object.__ne__
    # TODO these should emit an invalid override diagnostic
    def __eq__(self, other: str) -> B:
        return B()

    def __ne__(self, other: str) -> B:
        return B()
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:116 on 2024-10-25 22:21_

```suggestion
# TODO: should be `int` and `float`, need to check arg type and fall back to `rhs.__eq__` and `rhs.__ne__`. Because `object.__eq__` and `object.__ne__` accept `object` in typeshed, this can only happen with an invalid override of these methods, but we still support it.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:194 on 2024-10-25 22:24_

```suggestion
In the case of a subclass, the right-hand side has priority. However, if the overridden dunder method has an mismatched type to operand, the comparison will fall back to the left-hand side.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:281 on 2024-10-25 22:27_

```suggestion
    # TODO both these overrides should emit invalid-override diagnostic
    def __eq__(self, other: int) -> A:
        return A()

    def __ne__(self, other: int) -> A:
        return A()
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:285 on 2024-10-25 22:27_

This is another odd case, in that I think it's only possible via an invalid override. But I guess it still doesn't hurt to support it?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:280 on 2024-10-25 22:29_

I think this deserves to be its own separate test, and should cover all four inequality operators.

Also, let's assert on the error code (`[unsupported-operator]`) as well as message.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:296 on 2024-10-25 22:31_

In this case I think the type should in fact be `Unknown`, since comparisons can return any type at runtime, not just `bool`. But let's also note here that there should be a diagnostic.

```suggestion
# TODO: should be Unknown and emit diagnostic, need to check arg type and should be failed
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/integers.md`:15 on 2024-10-25 22:31_

```suggestion
# TODO: should be Unknown, and emit diagnostic, once we check call argument types
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/integers.md`:26 on 2024-10-25 22:32_

🎉 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:95 on 2024-10-25 22:34_

In this particular case, I think we should be able to infer Literal False and True, without ever falling back to typeshed, because we should explicitly implement equality comparison for mismatched Literal types to return `Literal[False]`. (Currently we just don't have a match arm for that case, so we fall back to the typeshed definitions instead.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:91 on 2024-10-25 22:34_

```suggestion
# TODO: should be Literal[False], once we implement (in)equality for mismatched literals
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:94 on 2024-10-25 22:35_

```suggestion
# TODO: should be Literal[True], once we implement (in)equality for mismatched literals
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:102 on 2024-10-25 22:36_

And we can also error on these cases, once we implement ordering comparisons on mismatched literals to always be an error

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:197 on 2024-10-25 22:37_

Here too I think all we need is the explicit handling of mismatched literals case, this should never fallback to `Type::Instance` and typeshed at all, because the types are totally literal -- we just don't have the required cases in the match statement yet

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:190 on 2024-10-25 22:38_

```suggestion
# TODO should be Literal[False] once we implement comparison of mismatched literal types
reveal_type(a is b)  # revealed: bool
# TODO should be Literal[True] once we implement comparison of mismatched literal types
reveal_type(a is not b)  # revealed: bool
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/unsupported.md`:10 on 2024-10-25 22:39_

```suggestion
# TODO: should error, once operand type check is implemented
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/unsupported.md`:16 on 2024-10-25 22:39_

```suggestion
# TODO: should error, once operand type check is implemented
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3785 on 2024-10-25 22:41_

```suggestion
/// Performs a membership test (`in` and `not in`) between two instances and returns the resulting type, or `None` if the test is unsupported.
```

---

_@carljm requested changes on 2024-10-25 22:42_

Code here looks great to me!! Thank you for following up with all the review comments.

I do have a bunch more review comments, but they are all about tests and mostly about TODOs :)

---

_@cake-monotone reviewed on 2024-10-26 17:21_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:3 on 2024-10-26 17:21_

Thanks for catching that! I've formatted it with an 80-character limit 🙂

---

_Comment by @cake-monotone on 2024-10-26 17:40_

Thanks so much for the super detailed and helpful review..!! I’ve already made updates based on your reviews.

Since the code’s mostly complete and I think I’ll be in a place where it’ll be hard to make code changes for a while 😢😢😢, feel free to commit any needed fixes directly!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:5 on 2024-10-26 17:44_

```suggestion
In Python, the term "membership test operators" refers to the operators
`in` and `not in`. To customize their behavior, classes can implement one of
the special methods `__contains__`, `__iter__`, or `__getitem__`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:10 on 2024-10-26 17:45_

```suggestion
- <https://docs.python.org/3/reference/datamodel.html#object.__contains__>
- <https://snarky.ca/unravelling-membership-testing/>
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:53 on 2024-10-26 17:49_

```suggestion
The final fallback is to implement `__getitem__` for integer keys. Python will
call `__getitem__` with `0`, `1`, `2`... until either the needle is found
(leading the membership test to evaluate to `True`) or `__getitem__` raises
`IndexError` (the raised exception is swallowed, but results in the membership
test evaluating to `False`).
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:68 on 2024-10-26 17:49_

```suggestion
Python coerces the results of containment checks to `bool`, even if `__contains__`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:152 on 2024-10-26 17:50_

```suggestion
the membership test is not supported and should trigger a diagnostic.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:12 on 2024-10-26 17:51_

```suggestion
## Rich Comparison Dunder Implementations For Same Class
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:48 on 2024-10-26 17:51_

```suggestion
## Rich Comparison Dunder Implementations for Other Class
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:89 on 2024-10-26 17:52_

```suggestion
side does not define them. Note: class `B` has its own `__eq__` and `__ne__`
methods to override those of `object`, but these methods will be ignored here
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:122 on 2024-10-26 17:53_

```suggestion
# TODO: should be `int` and `float`.
# Need to check arg type and fall back to `rhs.__eq__` and `rhs.__ne__`.
#
# Because `object.__eq__` and `object.__ne__` accept `object` in typeshed,
# this can only happen with an invalid override of these methods,
# but we still support it.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:147 on 2024-10-26 17:53_

```suggestion
precedence over those in the parent class. Class `B` inherits from `A` and
redefines comparison methods to return types other than `A`.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:285 on 2024-10-26 17:55_

Yes, just because we should emit a diagnostic telling the user off doesn't mean we should ignore what the annotation says! So this looks correct to me.

---

_@AlexWaygood reviewed on 2024-10-26 17:57_

Some more docs nitpicks. I don't think any are controversial, so I'll just push them to your branch :-)

---

_@carljm approved on 2024-10-26 18:04_

This looks ready to land to me! @AlexWaygood since you just reviewed as well, I'll assume you're good with it? I'll go ahead and enable auto-merge. Feel free to cancel if there's more you want to look at first.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/membership_test.md`:134 on 2024-10-26 18:07_

```suggestion
# TODO: should emit diagnostic, need to check arg type,
# should not fall back to __iter__ or __getitem__
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:316 on 2024-10-26 18:08_

```suggestion
# TODO: should be Unknown and emit diagnostic,
# need to check arg type and should be failed
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/unsupported.md`:14 on 2024-10-26 18:10_

```suggestion
# ("Operator `<` is not supported for types `object` and `int`")
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/unsupported.md`:20 on 2024-10-26 18:10_

```suggestion
# ("Operator `<` is not supported for types `int` and `object`")
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3637 on 2024-10-26 18:11_

```suggestion
    #[must_use]
    const fn dunder(self) -> &'static str {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3649 on 2024-10-26 18:11_

```suggestion
    #[must_use]
    const fn reflect(self) -> Self {
```

---

_@AlexWaygood approved on 2024-10-26 18:14_

Fantastic work. Thanks @cake-monotone!

---

_Renamed from "[red-knot] Compare expression inference - Instance" to "[red-knot] Type inference for comparisons between arbitrary instances" by @AlexWaygood on 2024-10-26 18:15_

---

_Merged by @AlexWaygood on 2024-10-26 18:19_

---

_Closed by @AlexWaygood on 2024-10-26 18:19_

---
