---
number: 2255
title: Self should be equivalent to bound typevar, but only Self causes Liskov violation
type: issue
state: open
author: kamalfarahani
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-29T08:03:48Z
updated_at: 2026-01-04T21:39:20Z
url: https://github.com/astral-sh/ty/issues/2255
synced_at: 2026-01-10T01:51:14Z
---

# Self should be equivalent to bound typevar, but only Self causes Liskov violation

---

_Issue opened by @kamalfarahani on 2025-12-29 08:03_

### Question

I am experimenting with `typing.Self` (introduced in [PEP 673](https://peps.python.org/pep-0673/)) to define a `Monoid` interface. According to the PEP, `Self` can be used to annotate parameters that expect instances of the current class.

However, when I attempt to narrow the type of the parameter in a subclass, my type checker `ty`  flags an error.

```python
class Monoid(ABC):
    @abstractmethod
    def op(self, other: Self) -> Self:
        raise NotImplementedError()


class IntAdditionMonoid(Monoid):
    def __init__(self, value: int):
        self.value = value

    def op(self, other: IntAdditionMonoid) -> Self:
        return IntAdditionMonoid(
            self.value + other.value,
        )
```
from what I understand from this [PEP](https://peps.python.org/pep-0673/):
> Another use for Self is to annotate parameters that expect instances of the current class

If the PEP states that `Self` represents the "current class," why am I unable to replace the `Self` annotation with the concrete class name in the subclass implementation?

### Version

ty 0.0.7

---

_Label `question` added by @kamalfarahani on 2025-12-29 08:03_

---

_Comment by @kamalfarahani on 2025-12-29 08:32_

Another observation is that in the PEP 673 it's noted that the `Self` behavior should be the same as the following code:
```python
M = TypeVar(
    "M",
    bound="Monoid",
)


class Monoid(ABC):
    @abstractmethod
    def op(self: M, other: M) -> M:
        raise NotImplementedError()


class IntAdditionMonoid(Monoid):
    def __init__(self, value: int):
        self.value = value

    def op(self, other: IntAdditionMonoid) -> IntAdditionMonoid:
        return IntAdditionMonoid(
            self.value + other.value,
        )
```

But with this implementation, `ty` won't yell at me, isn't it inconsistent behavior?

---

_Comment by @MichaReiser on 2025-12-29 11:02_

Pyrefly seems to be the only type checker that accepts this but I'm not sure why (I'm not a typing expert myself). All other type checkers (Pyright, mypy) reject this program.  I also couldn't find any test resembling your example in our extensive [liskov test suite](https://github.com/astral-sh/ruff/blob/e42cdf84957b178dc7e37aed0f2b1f4d5f0c272a/crates/ty_python_semantic/resources/mdtest/liskov.md#L4). I'm sorry that I don't know the answer to this but someone more knowledgable than I in Pythoon typing will get back to you, once they're back from their PTO. Happy holidays.

---

_Label `generics` added by @MichaReiser on 2025-12-29 11:02_

---

_Comment by @AlexWaygood on 2025-12-29 19:00_

> Pyrefly seems to be the only type checker that accepts this but I'm not sure why (I'm not a typing expert myself). All other type checkers (Pyright, mypy) reject this program. I also couldn't find any test resembling your example in our extensive [liskov test suite](https://github.com/astral-sh/ruff/blob/e42cdf84957b178dc7e37aed0f2b1f4d5f0c272a/crates/ty_python_semantic/resources/mdtest/liskov.md#L4). I'm sorry that I don't know the answer to this but someone more knowledgable than I in Pythoon typing will get back to you, once they're back from their PTO. Happy holidays.

Mypy does reject the program as written in the above snippet, but not because of a Liskov violation. It (correctly) complains that `IntAdditionMonoid.op` returns `IntAdditionMonoid` when it's annotated as returning `Self`, but there's no Liskov diagnostic.

If we change the snippet slightly to the following, ty and pyright issue a Liskov diagnostic (and only a Liskov diagnostic), but mypy and pyrefly do not:

```py
from __future__ import annotations
from abc import ABC, abstractmethod
from typing import Self

class Monoid(ABC):
    @abstractmethod
    def op(self, other: Self) -> Self:
        raise NotImplementedError()


class IntAdditionMonoid(Monoid):
    def __init__(self, value: int):
        self.value = value

    def op(self, other: IntAdditionMonoid) -> Self:
        return self.__class__(
            self.value + other.value,
        )
```

I think mypy/pyrefly are correct here, and ty/pyright are incorrect. This seems to me like it's a sound override; I don't think this breaks the Liskov Substitution Principle. The underlying bug is probably somewhere in `Signature::has_relation_to_impl`.

---

_Label `question` removed by @AlexWaygood on 2025-12-29 19:01_

---

_Label `bug` added by @AlexWaygood on 2025-12-29 19:01_

---

_Renamed from "Why this case is violation of Liskov Substitution Principle" to "Overriding a `foo: Self` parameter with `foo: <current class>` in a subclass should not be flagged as a Liskov violation" by @AlexWaygood on 2025-12-29 19:02_

---

_Comment by @carljm on 2025-12-29 19:09_

It looks to me like mypy and pyrefly are both OK with both versions (with `Self` and with `M`). Pyright errors on both versions. I agree that the two versions should be consistent (and I'm not sure why they diverge for us currently, since we literally implement `Self` as a bounded typevar.)

I think that this is not a safe override, though, and both versions _should_ error. (That is, pyright is correct, mypy and pyrefly are wrong.) Imagine we also have `class StrAdditionMonoid(Monoid)`, defined similarly to `IntAdditionMonoid`. Now we have a function `def op(m1: Monoid, m2: Monoid): m1.op(m2)`. If we allow this override, then the call `m1.op(m2)` is unsound if `m1` is an `IntAdditionMonoid` and `m2` is a `StrAdditionMonoid` -- we will end up calling `IntAdditionMonoid.op` method with arguments that violate its annotations.

For the same reason, annotating `other: Self` on a subclass method is also unsound!

Covariant typing of binary operators is a known problematic case; it can't be done soundly. `IntAdditionMonoid` cannot restrict its `other` argument to be an `IntAdditionMonoid` (excluding other subclasses of `Monoid`) without breaking Liskov.

(EDIT: I wrote my comment before seeing Alex's comment -- and I had tested the top snippet with `-> IntAdditionMonoid` on the subclass override, not the version with `-> Self` as written -- because I actually copied and modified the second snippet.)

---

_Renamed from "Overriding a `foo: Self` parameter with `foo: <current class>` in a subclass should not be flagged as a Liskov violation" to "Self should be equivalent to bound typevar, but only Self causes Liskov violation" by @carljm on 2025-12-29 19:14_

---

_Comment by @carljm on 2025-12-29 19:15_

Re-titled the issue, since I think the diagnostic on the Self version is correct -- the only mystery/bug here is why the explicit-TypeVar version does not behave the same as the Self version.

---

_Added to milestone `Stable` by @carljm on 2025-12-29 19:15_

---

_Comment by @AlexWaygood on 2025-12-29 19:27_

@carljm thanks -- agreed! This is indeed unsound:

```pycon
% python                                           
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from __future__ import annotations
... from abc import ABC, abstractmethod
... from typing import Self
... 
... class Monoid(ABC):
...     @abstractmethod
...     def op(self, other: Self) -> Self:
...         raise NotImplementedError()
... 
... class StrAdditionMonoid(Monoid):
...     def __init__(self, value: str):
...         self.value = value
... 
...     def op(self, other: StrAdditionMonoid) -> Self:
...         return self.__class__(
...             self.value + other.value,
...         )
... 
... class IntAdditionMonoid(Monoid):
...     def __init__(self, value: int):
...         self.value = value
... 
...     def op(self, other: IntAdditionMonoid) -> Self:
...         return self.__class__(
...             self.value + other.value,
...         )
... 
... def monoid_op(a: Monoid, b: Monoid):
...     a.op(b)
... 
... def unsound(a: IntAdditionMonoid, b: StrAdditionMonoid):
...     monoid_op(a, b)
...     
>>> unsound(IntAdditionMonoid(42), StrAdditionMonoid("42"))
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    unsound(IntAdditionMonoid(42), StrAdditionMonoid("42"))
    ~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<python-input-0>", line 32, in unsound
    monoid_op(a, b)
    ~~~~~~~~~^^^^^^
  File "<python-input-0>", line 29, in monoid_op
    a.op(b)
    ~~~~^^^
  File "<python-input-0>", line 25, in op
    self.value + other.value,
    ~~~~~~~~~~~^~~~~~~~~~~~~
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

---

_Comment by @kamalfarahani on 2025-12-29 21:45_

@carljm 

> I think that this is not a safe override, though, and both versions _should_ error. (That is, pyright is correct, mypy and pyrefly are wrong.)

Based on the specifications in **PEP 673**, I believe `mypy` and `pyrefly` are correctly implementing the standard, while `ty` and `pyright` appear to be in error. My reasoning centers on how `Self` constrains the relationship between parameters within a class hierarchy.

### 1. The Equivalence of `Self`

According to the PEP, the use of `Self` in a method signature is effectively syntactic sugar for a generic type variable bound to the enclosing class. Therefore, this definition:

```python
class Monoid(ABC):
    @abstractmethod
    def op(self, other: Self) -> Self:
        ...

```

Is semantically equivalent to using a type variable  bound to `Monoid`:

```python
class Monoid(ABC):
    @abstractmethod
    def op[M: Monoid](self: M, other: M) -> M:
        ...

```

### 2. Type Consistency Requirements

The critical takeaway from this equivalence is that `self` and `other` must be of the **exact same type** . In a concrete implementation, this means `type(self)` must be identical to `type(other)`.

### 3. Implications for Heterogeneous Types

If we accept the logic above, the following function should fail type checking:

```python
def monoid_op(a: Monoid, b: Monoid):
    a.op(b)  # Should raise a type error

```

**Reasoning:** The type checker cannot guarantee that `a` and `b` are the same subtype of `Monoid`. For example, if `IntegerMonoid` and `StringMonoid` both inherit from `Monoid`, passing a string monoid to an integer monoid's `op` method would violate the `Self` constraint.

Since `a` and `b` are typed only as the base `Monoid`, their specific subclasses are unknown and potentially incompatible, making the call to `a.op(b)` type-unsafe.

---

_Comment by @carljm on 2025-12-29 21:59_

> The critical takeaway from this equivalence is that `self` and `other` must be of the **exact same type** . In a concrete implementation, this means `type(self)` must be identical to `type(other)`.

No, this is not how generic functions work. The requirement is simply that `M` must solve to some type which satisfies all constraints. Pyright and mypy both allow this code, as they should:

```py
class A:
    pass

class B(A):
    pass

class C(A):
    pass

def func[T: A](x: T, y: T) -> T:
    ...

reveal_type(func(B(), C()))
```

Pyright solves the type variable to `B | C`; mypy solves it to `A`. Both are valid solutions.

The exception is constrained typevars (e.g. `[T: (A, B)]`), which are unusual, in that they list a specific set of types, and the typevar must solve to exactly one of those types. But that's not relevant here, since there is no constrained type variable.

> If we accept the logic above, the following function should fail type checking:
> 
> def monoid_op(a: Monoid, b: Monoid):
>     a.op(b)  # Should raise a type error

Yes, this is a good illustration of why we cannot, and should not, accept the (incorrect) requirement that a typevar must solve to the precise concrete type of every argument. Requiring type checkers to reject all generic calls with multiple arguments mapping to the same type variable if they cannot prove same-origin for all of those arguments, would be a massive change to the Python type system resulting in tons of false positives.

Mypy and pyrefly do not implement the interpretation of generics you are proposing; they would not error on the `a.op(b)` call either. They just choose to allow some forms of unsound overrides of generic methods.

---

_Comment by @kamalfarahani on 2026-01-01 10:39_

@carljm 

I've come across an interesting case: how is `Self` handled when used in a protocol? Based on the logic, the code below should trigger a type error, but surprisingly, neither `mypy` nor `pyright` or `ty` flag it. What’s the reasoning behind this?

```python
from __future__ import annotations

from typing import Protocol, Self


class Semigroup(Protocol):
    def op(
        self: Self,
        other: Self,
    ) -> Self: ...


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

---

_Comment by @kamalfarahani on 2026-01-02 18:35_

> Covariant typing of binary operators is a known problematic case; it can't be done soundly. `IntAdditionMonoid` cannot restrict its `other` argument to be an `IntAdditionMonoid` (excluding other subclasses of `Monoid`) without breaking Liskov.

As a solution for above problem I proposed the following addition to python typing which I think is related to the problems we discussed here:
https://github.com/python/typing/discussions/2143

---

_Comment by @carljm on 2026-01-04 21:39_

@kamalfarahani I agree with you, I think that type checkers (including ty) are behaving wrongly here: neither `IntSum` nor `StringConcat` should be considered to implement the `Semigroup` protocol in your example. This seems like a separate issue, so I opened #2323 to track it. Thanks!

---
