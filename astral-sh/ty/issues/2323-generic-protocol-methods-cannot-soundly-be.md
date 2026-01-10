```yaml
number: 2323
title: Generic protocol methods cannot soundly be implemented by a non-generic method
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2026-01-04T21:36:58Z
updated_at: 2026-01-04T22:11:54Z
url: https://github.com/astral-sh/ty/issues/2323
synced_at: 2026-01-10T01:56:41Z
```

# Generic protocol methods cannot soundly be implemented by a non-generic method

---

_Issue opened by @carljm on 2026-01-04 21:36_

> I've come across an interesting case: how is `Self` handled when used in a protocol? Based on the logic, the code below should trigger a type error, but surprisingly, neither `mypy` nor `pyright` or `ty` flag it. What’s the reasoning behind this?
> 
> ```python
> from __future__ import annotations
> 
> from typing import Protocol, Self
> 
> 
> class Semigroup(Protocol):
>     def op(
>         self: Self,
>         other: Self,
>     ) -> Self: ...
> 
> 
> class IntSum:
>     def __init__(self, value: int):
>         self.value = value
> 
>     def op(self, other: IntSum) -> IntSum:
>         return IntSum(
>             self.value + other.value,
>         )
> 
> 
> class StringConcat:
>     def __init__(self, value: str):
>         self.value = value
> 
>     def op(self, other: StringConcat) -> StringConcat:
>         return StringConcat(
>             self.value + other.value,
>         )
> 
> 
> def combine_semi[T: Semigroup](
>     a: T,
>     b: T,
> ) -> T:
>     return a.op(b)
> 
> 
> x = combine_semi(
>     IntSum(1),
>     StringConcat("hello"),
> )
> ``` 

 _Originally posted by @kamalfarahani in [#2255](https://github.com/astral-sh/ty/issues/2255#issuecomment-3703527962)_

---

_Comment by @carljm on 2026-01-04 21:38_

All other type-checkers (unsoundly) allow this, and so does ty. But it's clearly unsound -- `IntSum` and `StringConcat` should not be considered to implement the `Semigroup` protocol.

---

_Comment by @carljm on 2026-01-04 22:09_

This is not really specific to `Self` (as should be evident, since `Self` is syntactic sugar for a type variable), it affects generic protocols more broadly:

```py
from __future__ import annotations

from typing import Protocol, Self


class Semigroup[T](Protocol):
    def op(
        self: T,
        other: T,
    ) -> T: ...


class IntSum:
    def __init__(self, value: int):
        self.value = value

    def op(self, other: IntSum) -> IntSum:
        return IntSum(
            self.value + other.value,
        )


class StringConcat:
    def __init__(self, value: str):
        self.value = value

    def op(self, other: StringConcat) -> StringConcat:
        return StringConcat(
            self.value + other.value,
        )


def combine_semi[T: Semigroup](
    a: T,
    b: T,
) -> T:
    return a.op(b)


x = combine_semi(
    IntSum(1),
    StringConcat("hello"),
)
```

Even if `self` parameter isn't involved, it's still unsound:

```py
from __future__ import annotations

from typing import Protocol, Self


class Semigroup[T](Protocol):
    def op(
        self,
        a: T,
        b: T,
    ) -> T: ...


class IntSum:
    def __init__(self, value: int):
        self.value = value

    def op(self, a: IntSum, b: IntSum) -> IntSum:
        return IntSum(
            a.value + b.value,
        )


class StringConcat:
    def __init__(self, value: str):
        self.value = value

    def op(self, a: StringConcat, b: StringConcat) -> StringConcat:
        return StringConcat(
            a.value + b.value,
        )


def combine_semi[T: Semigroup](
    a: T,
    b: T,
) -> T:
    return a.op(a, b)


x = combine_semi(
    IntSum(1),
    StringConcat("hello"),
)
```

---

_Renamed from "Only methods using `Self` should satisfy a protocol method using `Self`" to "Generic protocol methods cannot soundly be implemented by a non-generic method" by @carljm on 2026-01-04 22:11_

---
