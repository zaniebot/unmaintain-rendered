---
number: 2158
title: Enforce the Liskov Substitution Principle for non-methods
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-12-22T13:54:58Z
updated_at: 2025-12-22T14:07:37Z
url: https://github.com/astral-sh/ty/issues/2158
synced_at: 2026-01-10T01:52:52Z
---

# Enforce the Liskov Substitution Principle for non-methods

---

_Issue opened by @AlexWaygood on 2025-12-22 13:54_

We have implemented a basic version of the Liskov Substitution Principle to cover cases where methods are incompatibly overridden in subclasses. We also need to implement separate diagnostics to cover properties incompatibly overridden in subclasses, and mutable attributes incompatibly overridden in subclasses.

The reason why these are planned as separate diagnostics is because a large amount of code in the ecosystem unsoundly overrides mutable attributes covariantly, e.g.

```py
class A:
    x: int

class B(A):
    x: bool
```

Partly this is because mypy allowed this for years -- and still does, unless users explicitly opt into the `mutable-override` error code. Implementing these as separate diagnostics to our existing `invalid-method-override` diagnostic will therefore allow users to switch the mutable-attribute-override rule off specifically if it causes a large number of diagnostics on their code.

---

Sub-tasks (many of these may share a common implementation):

- [ ] Enforce Liskov for property types: this should cause us to emit a diagnostic:
    ```py
    class A:
        @property
        def f(self) -> int:
            return 42

    class B(A):
        @property
        def f(self) -> str:  ❌ Superclass returns `int`, subclass returns `str`, `str` is not a subtype of `int`
            return "42"
    ```

- [ ] Enforce that a writable attribute cannot be overridden with a read-only property:
    ```py
    class A:
        x: int

    class B(A):
        @property
        def x(self) -> int:  ❌ Superclass attribute is writable, subclass attribute is read-only
            return 42
    ```

    and

    ```py
    class A:
        @property
        def x(self) -> int:
            return 42

        @x.setter
        def x(self, value: int) -> None: ...

    class B(A):
        @property
        def x(self) -> int:  ❌ Superclass attribute is writable, subclass attribute is read-only
            return 42
    ```

- [ ] Enforce Liskov for attribute types:
    ```py
    class A:
        x: int

    class B(A):
        x: bool  ❌ Type of `x` attribute is invariant because it is mutable
    ```

- [ ] Enforce that a non-`Final` attribute cannot be overridden with a `Final` one
    ```py
    from typing import Final

    class A:
        x: int

    class B(A):
        x: Final[int]  ❌ Superclass attribute is writable, subclass attribute is read-only
    ```

- [ ] Enforce that a non-`ClassVar` attribute cannot be overridden with a `ClassVar` attribute:
    ```py
    from typing import ClassVar

    class A:
        x: int

    class B(A):
        x: ClassVar[int]  ❌ Superclass attribute is writable on instances, subclass attribute is not
    ```

- [ ] Enforce that a `ClassVar` attribute cannot be overridden with an implicit instance attribute, since `ClassVar`s can be mutated on the class object itself but implicit instance attributes cannot:
    ```py
    from typing import ClassVar

    class A:
        x: ClassVar[int]

    class B(A):
        def __init__(self):
            self.x: int  ❌ Superclass attribute is writable on the class object, subclass attribute is not
    ```

- [ ] Enforce that a non-`ReadOnly` `TypedDict` field cannot be overridden with a `ReadOnly` `TypedDict` field:
    ```py
    from typing import TypedDict, ReadOnly

    class A(TypedDict):
        x: int

    class B(A):
        x: ReadOnly[int]  ❌ Superclass field is writable, subclass attribute is read-only
    ```

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-22 13:54_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-22 13:55_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-22 13:55_

---

_Comment by @beauxq on 2025-12-22 14:07_

I think it's an important checklist item to check multiple levels of subclasses, not just one base class and one subclass -
because mypy and pyright each get things wrong in different places here:

```python
from typing import ClassVar, reveal_type

class B:
    x: ClassVar[int | str] = 0

class C(B):
    x = "random"

class D(C):
    x = 1  # mypy complains when it shouldn't

class E(D):
    x = b"x"  # pyright doesn't complain when it should


def foo(b: type[B]) -> None:
    x = b.x
    reveal_type(x)
    if isinstance(x, str):
        print(x + "y")
    else:
        reveal_type(x)
        print(x + 3)


foo(E)
```

---
