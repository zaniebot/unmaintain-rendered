```yaml
number: 2446
title: "Unannotated implicit instance attributes: consider prioritising the inferred type in constructor methods over the inferred type in other methods"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
  - attribute access
assignees: []
created_at: 2026-01-11T14:03:28Z
updated_at: 2026-01-11T14:03:41Z
url: https://github.com/astral-sh/ty/issues/2446
synced_at: 2026-01-12T15:54:26Z
```

# Unannotated implicit instance attributes: consider prioritising the inferred type in constructor methods over the inferred type in other methods

---

_@AlexWaygood_

Currently, if an instance attribute is not annotated in a class's body or in any of its methods, ty infers the type of that attribute as the union of all assignments to that attribute inside the class. Assignments in constructor methods are treated identically to assignments in other methods. For example:

```py
class A:
    def __init__(self, x: int):
        self.x = x

class B:
    def foo(self):
        self.x = 42

class C:
    def __init__(self, x: int):
        self.x = x

    def foo(self):
        self.x = "foo"

class D:
    def foo(self):
        self.x = 42

    def bar(self):
        self.x = "bar"

reveal_type(A(42).x)  # revealed: Unknown | int
reveal_type(B().x)  # revealed: Unknown | Literal[42]
reveal_type(C(42).x)  # revealed: Unknown | int | Literal["foo"]
reveal_type(D().x)  # revealed: Unknown | Literal[42, "bar"]
```

Once https://github.com/astral-sh/ty/issues/1240#issuecomment-3729947824 has been implemented, the revealed types will change to:

```py
reveal_type(A(42).x)  # revealed: int
reveal_type(B().x)  # revealed: int
reveal_type(C(42).x)  # revealed: int | str
reveal_type(D().x)  # revealed: int | str
```

But while I would be happy with the inferred types for `A`, `B` and `D` there, I'm still not sure that this is what most users would expect or want for `C`. I think most users would probably consider their `C` class to be fully typed, and would want the type checker to complain about the assignment of a string in the `C.foo` method, because it is inconsistent with the inferred type of the attribute when it was assigned in `__init__`. While in principle there is nothing special about `__init__` when it comes to setting attributes, in practice most Python developers declare and assign most instance attributes in `__init__` and think of unannotated attribute assignments to `self` in these methods as having similar status to a "declaration" of the attribute.

I therefore propose that for instance attributes which:
1. Have no explicit annotation in any method or the class body, and
2. Are assigned in `__init__` and/or `__new__`,

we simply ignore the inferred type of assignments in any other methods and treat the "declared type" of that attribute as the type that was assigned in `__init__` and/or `__new__`. This would lead us to reject the `self.x` assignment in `C.foo` and infer the following types:

```py
reveal_type(A(42).x)  # revealed: int
reveal_type(B().x)  # revealed: int
reveal_type(C(42).x)  # revealed: int
reveal_type(D().x)  # revealed: int | str
```

It would bring us closer to [mypy's behaviour](https://mypy-play.net/?mypy=latest&python=3.12&gist=3fe94795c0019335686cbc0efcc8e437) -- but, unlike mypy, I would want to retain our current behaviour for the `D` class and not emit a diagnostic on `D.foo`.

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-11 14:03_

---

_Label `attribute access` added by @AlexWaygood on 2026-01-11 14:03_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-11 14:03_

---
