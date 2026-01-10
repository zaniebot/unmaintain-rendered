```yaml
number: 541
title: Infer class specializations for class and static methods
type: issue
state: open
author: dcreager
labels:
  - generics
assignees: []
created_at: 2025-05-29T19:04:05Z
updated_at: 2025-11-13T06:48:45Z
url: https://github.com/astral-sh/ty/issues/541
synced_at: 2026-01-10T02:06:24Z
```

# Infer class specializations for class and static methods

---

_Issue opened by @dcreager on 2025-05-29 19:04_

We can currently infer the specialization of a generic class from its constructor methods, if they include parameters that use the class's typevars:

```py
class C[T]:
    def __init__(self, value: T) -> None: ...

reveal_type(C(1))  # revealed: C[int]
```

We should do the same for `@classmethod`s and `@staticmethod`s:

```py
class C[T]:
    @classmethod
    def create(cls, value: T) -> C[T]: ...

    @staticmethod
    def instantiate(value: T) -> C[T]: ...

reveal_type(C.create(1))  # revealed: C[int]
reveal_type(C.instantiate(1))  # revealed: C[int]
```

---

_Label `generics` added by @AlexWaygood on 2025-05-29 19:04_

---

_Comment by @erictraut on 2025-05-29 19:33_

I don't think this behavior would be correct. [PEP 696](https://peps.python.org/pep-0696/) (which is now incorporated into the typing spec) effectively says that if `T` has no default type argument, the type of `C` must be evaluated as `C[Any]` in the expression `C.create` or `C.instantiate`. In other words, `C.create` and `C.instantiate` are equivalent to `C[Any].create` and `C[Any].instantiate`. If a type argument default is specified, then it is used instead, like in this example.

```python
class D[T = int]:
    @classmethod
    def create(cls, value: T) -> D[T]: ...

    @staticmethod
    def instantiate(value: T) -> D[T]: ...

reveal_type(D.create(1))  # revealed: D[int]
reveal_type(D.instantiate(1))  # revealed: D[int]
```

Mypy currently gets this wrong, but pyright conforms with the spec.

---

_Comment by @AlexWaygood on 2025-05-29 19:47_

Ah, so it would need to be something like this in order for us to validly make the inference suggested by @dcreager, correct? (`C.create()` and `C.instantiate()` would need to be generic over a different typevar than the one the class is generic over?)

```py
class C[T]:
    @classmethod
    def create[S](cls, value: S) -> C[S]: ...

    @staticmethod
    def instantiate[S](value: S) -> C[S]: ...

reveal_type(C.create(1))  # revealed: C[int]
reveal_type(C.instantiate(1))  # revealed: C[int]
```

---

_Comment by @dcreager on 2025-05-29 20:11_

Thanks for clarifying, @erictraut! So it really is class _construction_ that is special-cased to allow inferring a non-default class specialization.

@AlexWaygood, we actually already do support your example that uses a different typevar [[playground](https://play.ty.dev/f83f68fc-afc7-44d5-b6e4-f028e7fdd8bc)], with the caveat that we don't promote literal types to their instance types:

```py
class C[T]:
    @classmethod
    def create[S](cls, x: S) -> C[S]:
        return C[S]()

reveal_type(C.create(4))  # revealed: C[Literal[4]]
```

---

_Comment by @dcreager on 2025-05-29 20:12_

Correction, we support this pattern for `classmethod`, but not for `staticmethod`

```py
class C[T]:
    @staticmethod
    def create[S](x: S) -> C[S]:
        return C[S]()

reveal_type(C.create(4))  # revealed: Unknown
```

---

_Comment by @carljm on 2025-05-29 20:36_

I'm not sure I think this is what the spec _should_ say about these cases, but I'm happy to defer that question to another day, and not spend time right now doing extra work to implement something here that the spec doesn't require.

---

_Comment by @purepani on 2025-05-30 00:52_

I don't quite see how the PEP implies that if T has no default argument, then the type must be evaluated as `C[Any]`. It only says that if it does have a default, then it must evaluate as `C[Default]`.
Additionally, if I'm reading the spec correctly, it should be correct behavior to infer with Self:
```python
from typing import Self, reveal_type

class C[T]:
    @classmethod
    def method(cls, x: T) -> Self: ...

reveal_type(C.method("x")) # C[str]
```
Per https://typing.python.org/en/latest/spec/generics.html#use-in-generic-classes
> The PEP doesn’t specify the exact type of self.value within the method set_value. Some type checkers may choose to implement Self types using class-local type variables with Self = TypeVar(“Self”, bound=Container[T]), which will infer a precise type T. However, given that class-local type variables are not a standardized type system feature, it is also acceptable to infer Any for self.value. We leave this up to the type checker.

While it's definitely not specified by the type checker, I don't think wrong behavior to infer it, at least(though certainly no need to implement it yet if it's explicitly not required).



---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:48_

---
