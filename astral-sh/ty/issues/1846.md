```yaml
number: 1846
title: "don't discard \"unused\" typevars from generic context of simplified generic type alias"
type: issue
state: open
author: cmp0xff
labels:
  - generics
  - type aliases
assignees: []
created_at: 2025-12-10T20:21:43Z
updated_at: 2025-12-14T10:07:01Z
url: https://github.com/astral-sh/ty/issues/1846
synced_at: 2026-01-10T01:55:00Z
```

# don't discard "unused" typevars from generic context of simplified generic type alias

---

_Issue opened by @cmp0xff on 2025-12-10 20:21_

### Summary

## Summary

Hi, the following code snippet causes `invalid-type-arguments` in current `ty`, see [the playground](https://play.ty.dev/5e7a9db4-c5cc-4b6e-87a5-46789473576a):

```py
from collections.abc import Hashable, Sequence
from typing import TypeAlias, TypeVar

HashableT = TypeVar("HashableT", bound=Hashable)

VType: TypeAlias = Hashable | Sequence[HashableT]

def foo(p: VType[Hashable]) -> None: ...   # Too many type arguments: expected 0, got 1 (invalid-type-arguments) [Ln 8, Col 18]
```

It passes the check of `mypy` and `pyright`.

The issue arose from pandas-dev/pandas-stubs#1537.

## Version

5dc0079e7

### Version

_No response_

---

_Comment by @carljm on 2025-12-10 20:43_

What is happening here is that the entire concept of a `Hashable` protocol is fundamentally broken in the Python type system, because `object` itself (a type which includes all Python objects) meets the requirements of `Hashable`, and yet many subtypes of `object` _remove_ hashability (violating the Liskov Substitution Principle, which is the core assumption of all treatment of subclasses in Python typing.) Thus [this comment in typeshed](https://github.com/python/typeshed/blob/main/stdlib/typing.pyi#L520-L522) describing `Hashable` as "mostly useless for static type checking."

Because of this, ty sees `Hashable` as equivalent to `object` (which is reasonable, given that the description of `object` in typeshed does satisfy `Hashable`), and thus sees the union of anything with `Hashable` as also equivalent to `object`. So `Hashable | Sequence[HashableT]` all simplifies to `object` -- thus the typevar is irrelevant as far as ty is concerned.

I do have one question about the code here: why is it `Hashable | Sequence[HashableT]` rather than `HashableT | Sequence[HashableT]`? The latter seems to make more sense (it's not clear why only the type inside the sequence variant should be determined by the type argument), and it would eliminate this problem.

I do think there are several improvements we need to consider for ty here:

1. We probably need to do something to be more similar to other type checkers in their (unsound) treatment of `Hashable`. This is already tracked in https://github.com/astral-sh/ty/issues/1162
2. Entirely apart from `Hashable`, I think we should not discard "redundant" typevars in type aliases, ideally. Even if the type alias were literally `VType: TypeAlias = object | Sequence[T]`, I think we ideally should not error on `VType[int]`, despite `VType[whatever]` always simplifying to `object`. Let's use this issue to track that improvement.

Thanks for the report!


---

_Renamed from "`invalid-type-arguments` with `Union` type in `TypeAlias`" to "don't discard "unused" typevars from generic context of simplified generic type alias" by @carljm on 2025-12-10 20:45_

---

_Added to milestone `Stable` by @carljm on 2025-12-10 20:45_

---

_Label `generics` added by @carljm on 2025-12-10 20:46_

---

_Label `type aliases` added by @carljm on 2025-12-10 20:47_

---

_Comment by @sharkdp on 2025-12-10 21:18_

Point 2 seems related to / the same as #1666?

---

_Comment by @carljm on 2025-12-10 21:20_

Yes, thank you!

---

_Closed by @carljm on 2025-12-10 21:20_

---

_Comment by @AlexWaygood on 2025-12-10 21:24_

Interesting. I assumed that `object` would be the one exception we would carve out for https://github.com/astral-sh/ty/issues/1666, since we consider even `Any` a subtype of (and redundant with) `object`. So surely any `TypeVar` should be too, even after you take into account the possibility that the `TypeVar` could be solved as `Any`?

---

_Comment by @carljm on 2025-12-10 21:43_

Hmm, yeah. So there may be something separate to solve here, where we collect typevars before eliminating union redundancies.

Or we could decide that in general we're fine with `x: TypeAlias = object | list[T]` being treated as a non-generic type alias, because that's a weird thing to do -- so we we just need to handle `Hashable` differently. (But I don't think I agree with this. If the user put a typevar in their type alias, they are intending it to be generic -- eager union simplification should not change that.)

---

_Comment by @carljm on 2025-12-10 21:45_

Going to reopen this, since it does seem to require some different handling of typevars in union type aliases, even if #1666 is fixed.

---

_Reopened by @carljm on 2025-12-10 21:45_

---

_Comment by @cmp0xff on 2025-12-14 08:25_

Hi, with the new alpha release, the error message has shifted to
> Cannot subscript non-generic type: `<types.UnionType special form 'Hashable'>` is already specialized (non-subscriptable) [Ln 8, Col 12]

---

_Comment by @cmp0xff on 2025-12-14 10:07_

@carljm 

> I do have one question about the code here: why is it `Hashable | Sequence[HashableT]` rather than `HashableT | Sequence[HashableT]`? The latter seems to make more sense (it's not clear why only the type inside the sequence variant should be determined by the type argument), and it would eliminate this problem.

Good point, it was an old piece of code, but I dared to touch it.

---
