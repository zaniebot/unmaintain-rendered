```yaml
number: 492
title: Different specializations of the same generic are considered different bases
type: issue
state: open
author: InSyncWithFoo
labels:
  - bug
  - generics
  - runtime semantics
assignees: []
created_at: 2025-05-23T02:14:17Z
updated_at: 2025-11-13T06:58:27Z
url: https://github.com/astral-sh/ty/issues/492
synced_at: 2026-01-12T15:54:23Z
```

# Different specializations of the same generic are considered different bases

---

_@InSyncWithFoo_

Consider the following innocent-looking example ([playground](https://play.ty.dev/c7337e08-b762-456f-a988-91812a6e296e)):

```python
class StrConverter[T](Protocol):
    def convert(self, original: T, /) -> str: ...

class NumberToStrConverter(StrConverter[int], StrConverter[float]):
    def convert(self, original: int | float, /) -> str:
        return str(original)
```

It appears that ty considers `StrConverter[int]` and `StrConverter[float]` to be two different bases. This is incorrect; at runtime, the two specializations are considered the same base class, having the same [`origin`](https://docs.python.org/3/library/typing.html#typing.get_origin), and so the class definition fails:

```text
TypeError: duplicate base class StrConverter
```

---

_Label `bug` added by @AlexWaygood on 2025-05-23 02:17_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-23 02:17_

---

_Comment by @AlexWaygood on 2025-05-23 23:09_

The same underlying issue can also cause us to infer invalid MROs in some situations, where the same underlying class appears twice with two different specialisations:

```py
class Foo[T]: ...
class Bar(Foo[int]): ...
class Baz(Foo[str]): ...
class Spam(Bar, Baz): ...
reveal_type(Spam.__mro__)  # tuple[<class 'Spam'>, <class 'Bar'>, <class 'Foo[int]'>, <class 'Baz'>, <class 'Foo[str]'>, typing.Generic, <class 'object'>]`
```

That should never happen.

@dcreager, this makes me think that we probably shouldn't be sending specialised class objects into the `c3_merge` function -- I think we should probably be sending class literals into it, which is what happens at runtime. How about this for an alternative to what we're currently doing:
1. The `Mro` struct holds a list of class-literals that are always unspecialised, and this unspecialised MRO is what's returned by `Class::try_mro`.
2. But `MroIterator` lazily specialises each MRO entry as it yields them one-by-one, ensuring that `Class::iter_mro` always yields an MRO in which all entries are specialised.

Does that sound like a plausible approach?

---

_Label `generics` added by @AlexWaygood on 2025-05-23 23:19_

---

_Comment by @dcreager on 2025-05-24 13:51_

> The same underlying issue can also cause us to infer invalid MROs in some situations, where the same underlying class appears twice with two different specialisations:
> 
> ```py
> class Foo[T]: ...
> class Bar(Foo[int]): ...
> class Baz(Foo[str]): ...
> class Spam(Bar, Baz): ...
> reveal_type(Spam.__mro__)  # tuple[<class 'Spam'>, <class 'Bar'>, <class 'Foo[int]'>, <class 'Baz'>, <class 'Foo[str]'>, typing.Generic, <class 'object'>]`
> ```
> 
> That should never happen.

What is the correct MRO in this case? If `Foo` should only appear once (since c3-merge operates on unspecialized class literals), and we want to include specializations in the MROs that we produce, which specialization should we use?

Or put differently, what should the following reveal?

```py
class Foo[T]:
    def method(self) -> T: ...

class Bar(Foo[int]): ...
class Baz(Foo[str]): ...
class Spam(Bar, Baz): ...

reveal_type(Spam.method)
```

---

_Comment by @InSyncWithFoo on 2025-05-24 17:54_

`Spam` inherits from both `Foo[int]` and `Foo[str]`, so its `method` would have the type of `(self: Self) -> int & str` (or `((self: Self) -> int) & ((self: Self) -> str)`) where `Self` is a subtype of `Foo[int] & Foo[str]`.

The runtime MRO is `(Spam, Bar, Baz, Foo, Generic, object)`, so the revealed MRO should look like this:

```python
tuple[<class 'Spam'>, <class 'Bar'>, <class 'Baz'>, <class 'Foo[int] & Foo[str]'>, typing.Generic, <class 'object'>]
```

---

_Comment by @carljm on 2025-05-27 18:51_

Pyright considers `Spam` invalid; it says `Foo[str]` and `Foo[int]` are incompatible bases.

Mypy is fine with this code in a way that looks unsound; it seems to do something similar to what we currently do (at least, it infers the type of an attribute typed as the generic typevar as whichever `Foo` specialization would come first in the MRO).

If the "duplicate but differently specialized" base classes are direct bases instead, as in the OP example, then both mypy and pyright error (which makes sense, since that version is an error at runtime.)

I think @InSyncWithFoo's answer is probably the right one if we want to support the indirect version, but I also think supporting this is not high priority.

---

_Comment by @AlexWaygood on 2025-05-27 18:54_

I lean towards doing what pyright does and saying that it's invalid to have two specialisations of the same class in any MRO. I don't see a use case for it, and I think it could produce surprising behaviour if the inferred specialisation for a method on that base ends up simplifying to `Never` due to disjoint types intersecting with each other.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:58_

---
