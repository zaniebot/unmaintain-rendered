```yaml
number: 19289
title: "[ty] Extend tuple `__len__` and `__bool__` special casing to also cover tuple subclasses"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex-brent/tuple-length
created_at: 2025-07-11T18:04:40Z
updated_at: 2025-07-21T12:51:03Z
url: https://github.com/astral-sh/ruff/pull/19289
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Extend tuple `__len__` and `__bool__` special casing to also cover tuple subclasses

---

_Pull request opened by @AlexWaygood on 2025-07-11 18:04_

## Summary

This PR (a collaboration with @ntBre) extends our existing `__len__` and `__bool__` special casing so that it also works with tuple subclasses. I.e., for a class like `Foo` here, we will now understand that instances of `Foo` are always truthy, and that calling `len()` on an instance of `Foo` will always return `Literal[1]`:

```py
class Foo(tuple[int]): ...
```

The approach taken is to synthesize `__bool__` and `__len__` class attributes on the generic alias object `tuple[int]`. `tuple[int].__bool__` is inferred as a function-like `Callable` type with signature `(self: tuple[int], /) -> Literal[True]`; a similar approach is taken for `tuple.__len__`. This means that the correct signature for these methods is naturally inherited by subclasses; it means that protocol subtyping will work correctly for these subclasses; and it means that once we have implemented the Liskov Substitution Principle, unsound overrides of these methods on tuple subclasses will be naturally detected by ty.

## Test Plan

Mdtests

---

Co-authored-by: Brent Westbrook <36778786+ntBre@users.noreply.github.com>


---

_Review requested from @carljm by @AlexWaygood on 2025-07-11 18:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-11 18:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-11 18:04_

---

_Label `ty` added by @AlexWaygood on 2025-07-11 18:04_

---

_Comment by @github-actions[bot] on 2025-07-11 18:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MatthewMckee4 reviewed on 2025-07-11 20:12_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-11 20:12_

Should this not also be a Liskov violation?

---

_@MatthewMckee4 reviewed on 2025-07-11 20:17_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-11 20:17_

Okay i guess its not a Liskov violation, but should we not complain at this?

---

_@MatthewMckee4 reviewed on 2025-07-11 20:19_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-11 20:19_

Ah sorry I retract this, my bad.

---

_@MatthewMckee4 reviewed on 2025-07-11 20:23_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-11 20:23_

Okay changed my mind slightly
```py
def foo(x: tuple[int, ...]):
    if x:
        # Should we not also be able to say len(x) != 0
        # *This should pass
        assert len(x) != 0

foo(Baz(()))
```

In rust if you have a `len` function clippy tells you to have an `is_empty` function and auto generates to `self.len() == 0`.

This maybe isn't relevant, but it seems like we shouldn't allow override of *just* `__bool__`. I'm not sure where else to go with this to be honest.

---

_@AlexWaygood reviewed on 2025-07-12 14:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-12 14:32_

No, I think that's a great point! It seems like in general, as part of our Liskov implementation, we should probably enforce that `__bool__` and `__len__` are never overridden to be inconsistent with each other on any `Sequence` subtype. I think otherwise this could indeed cause us to make incorrect assumptions, not just regarding tuples

---

_@MatthewMckee4 reviewed on 2025-07-12 15:18_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-12 15:18_

Cool thanks, so in this case we could emit a diagnostic saying that there should be a ’__len__’ method that returns ’int & ~Literal[0]’ (or something like that).

---

_@AlexWaygood reviewed on 2025-07-13 21:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:108 on 2025-07-13 21:07_

Hmm, not sure about that specifically -- I guess I'm not sure we _should_ emit an error on this specific tuple subclass. It seems a bit pedantic -- just because the `__len__` method is annotated as returning `int` doesn't mean that the class is violating the contract of `Sequence`s. It just means that we haven't been given enough information to verify; I think it's probably better to steer clear of false positives in that situation and assume the user knows what they're doing. It's not the goal of ty to catch every _possible_ error that a user could make.

But I _do_ think it's worth emitting errors on these classes, because here we _do_ have enough information to say that `__len__` and `__bool__` are inconsistent with each other:

```py
from collections import Sequence
from typing import Literal

class Foo(Sequence[int]):
    def __len__(self) -> Literal[0]:
        return 0

    def __bool__(self) -> Literal[True]:
        return True

class Bar(Sequence[int]):
    def __len__(self) -> Literal[3]:
        return 3

    def __bool__(self) -> Literal[False]:
        return False
```

Even here, though, it's questionable whether this is a _typing_ error or more something that's in the domain of a type-aware linter to check. (That doesn't mean that it wouldn't be a useful rule -- I think it would! But it might not be in the core purview of ty right now -- it might be more something for the future, when we start to use ty to power type-aware lint rules in Ruff.)

So I guess I feel like you're raising a great point in general, but that for these specific cases we probably shouldn't emit errors, and even if we should it maybe shouldn't be ty _itself_ that emits these errors but maybe a type-aware linter built on top of ty :-)

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-14 07:14_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:99 on 2025-07-21 11:48_

Here, we know that the length is `>=2`, even. Would it make sense to test the "more extreme" case of just ">=1"?

```suggestion
# Unknown length, but we know the length is guaranteed to be >0
class Bar(tuple[int, *tuple[str, ...]]): ...
```

Or to adapt the description above (which is not wrong, but a bit confusing)?

```suggestion
# Unknown length, but we know the length is guaranteed to be >=2
class Bar(tuple[int, *tuple[str, ...], bytes]): ...
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:99 on 2025-07-21 11:54_

And in extensions of the above: would it make sense to add a second test here that shows that this is *not* true for `class Bar(tuple[str, ...]): ...`?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:135 on 2025-07-21 11:56_

Minor personal preference: I try to use self-explaining names (e.g. `EmptyTupleSubclass` here, `UnknownLengthTupleSubclass` below) over things like foo and bar. Makes reading the test assertions easier.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:78 on 2025-07-21 11:58_

Add a test for `D` here as well?

```suggestion
reveal_type(len(C((1, "foo"))))  # revealed: Literal[2]
reveal_type(len(D((1, 2, 3))))  # revealed: int
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:82 on 2025-07-21 11:59_

Similar here: use self-explaining names for A, B, C, D, and change this to something like the following?
```suggestion
reveal_type(ExactlyTwo.__len__)  # revealed: (self: tuple[int, int], /) -> Literal[2]
reveal_type(UnknownLength.__len__)  # revealed: (self: tuple[int, ...], /) -> int
```

---

_@sharkdp approved on 2025-07-21 12:07_

Very nice!

---

_@AlexWaygood reviewed on 2025-07-21 12:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:99 on 2025-07-21 12:27_

> And in extensions of the above: would it make sense to add a second test here that shows that this is _not_ true for `class Bar(tuple[str, ...]): ...`?

we have that one a bit lower down, in the "ambiguous values" section -- it's on line 149 

---

_Merged by @AlexWaygood on 2025-07-21 12:50_

---

_Closed by @AlexWaygood on 2025-07-21 12:50_

---

_Branch deleted on 2025-07-21 12:50_

---
