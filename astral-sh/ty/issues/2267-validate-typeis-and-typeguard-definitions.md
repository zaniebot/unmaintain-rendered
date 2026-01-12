```yaml
number: 2267
title: validate TypeIs and TypeGuard definitions
type: issue
state: open
author: carljm
labels:
  - help wanted
  - typing semantics
  - conformance
assignees: []
created_at: 2025-12-29T23:14:51Z
updated_at: 2026-01-06T17:02:28Z
url: https://github.com/astral-sh/ty/issues/2267
synced_at: 2026-01-12T15:54:26Z
```

# validate TypeIs and TypeGuard definitions

---

_@carljm_

In order to fully pass the conformance suite for `TypeIs` and `TypeGuard`, we need to add some extra validations of functions annotated with a `TypeIs` or `TypeGuard` return type.

1. The function should accept exactly one parameter (not counting `self` or `cls` if it's a method / classmethod).
2. For `TypeIs`, the type parameter of the `TypeIs` return type must be assignable to the annotated type of the single parameter.


---

_Label `typing semantics` added by @carljm on 2025-12-29 23:14_

---

_Label `help wanted` added by @carljm on 2025-12-29 23:15_

---

_Label `conformance` added by @carljm on 2025-12-29 23:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-30 15:52_

---

_Comment by @dangotbanned on 2025-12-30 17:39_

> The function should accept exactly one parameter (not counting self or cls if it's a method / classmethod).

Is this a new requirement of type narrowing functions?

The spec describes something quite different currently:


https://typing.python.org/en/latest/spec/narrowing.html#typeis
> Type narrowing functions must accept at least one positional argument. The type narrowing behavior is applied to the first positional argument passed to the function. **The function may accept additional arguments, but they are not affected by type narrowing**. If a type narrowing function is implemented as an instance method or class method, the first positional argument maps to the second parameter (after `self` or `cls`).

There are these examples as well which show multiple parameters:

> ```py
> def is_str_list(val: list[object], allow_empty: bool) -> TypeGuard[list[str]]:
>     if len(val) == 0:
>         return allow_empty
>     return all(isinstance(x, str) for x in val)
> 
> def is_set_of[T](val: set[Any], type: type[T]) -> TypeGuard[set[T]]:
>     return all(isinstance(x, type) for x in val)
> ```

---

_Comment by @carljm on 2025-12-30 18:10_

My bad, should have double checked the spec. You're right, extra parameters are allowed. (I'm not sure they should be tbh, since if they affect the return type of the function at all, that means the function isn't sound in some way -- but whether I think they should be isn't relevant here 😉)

---

_Comment by @charliermarsh on 2025-12-30 18:17_

(Just confirming that I believe the linked PR implements the appropriate behavior.)

---

_Comment by @jelle-openai on 2026-01-05 17:24_

> if they affect the return type of the function at all, that means the function isn't sound in some way

That's not true for TypeGuard, though it is true for TypeIs.

---

_Comment by @dangotbanned on 2026-01-06 12:29_

FYI, [this disaster](https://github.com/narwhals-dev/narwhals/blob/00858bba9666a2cbf71d271308a0ab07413d6867/narwhals/_utils.py#L708-L757) is understood by *at-least* `pyright` - if you wanted a challenge 😉

<img width="868" alt="Image" src="https://github.com/user-attachments/assets/65593716-1ed3-4785-9c14-4be72759facb" />

Like a lot of python typing, that came *after* it was already in use at runtime. I'm not vouching for this as a good idea.

It still has [quite a few hits on a code search](https://github.com/search?q=repo%3Anarwhals-dev/narwhals%20isinstance_or_issubclass&type=code) and you might be suprised to know it was once even worse (https://github.com/narwhals-dev/narwhals/pull/3139)


```py
from typing import Any, TypeVar, overload
from typing_extensions import TypeIs

_T1 = TypeVar("_T1")
_T2 = TypeVar("_T2")
_T3 = TypeVar("_T3")


class DType: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: type, cls_or_tuple: type[_T]
) -> TypeIs[type[_T]]: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: object | type, cls_or_tuple: type[_T]
) -> TypeIs[_T | type[_T]]: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: type, cls_or_tuple: tuple[type[_T1], type[_T2]]
) -> TypeIs[type[_T1 | _T2]]: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: object | type, cls_or_tuple: tuple[type[_T1], type[_T2]]
) -> TypeIs[_T1 | _T2 | type[_T1 | _T2]]: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: type, cls_or_tuple: tuple[type[_T1], type[_T2], type[_T3]]
) -> TypeIs[type[_T1 | _T2 | _T3]]: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: object | type, cls_or_tuple: tuple[type[_T1], type[_T2], type[_T3]]
) -> TypeIs[_T1 | _T2 | _T3 | type[_T1 | _T2 | _T3]]: ...


@overload
def isinstance_or_issubclass(
    obj_or_cls: Any, cls_or_tuple: tuple[type, ...]
) -> TypeIs[Any]: ...


def isinstance_or_issubclass(obj_or_cls: Any, cls_or_tuple: Any) -> bool:
    # from narwhals.dtypes import DType

    if isinstance(obj_or_cls, DType):
        return isinstance(obj_or_cls, cls_or_tuple)
    return isinstance(obj_or_cls, cls_or_tuple) or (
        isinstance(obj_or_cls, type) and issubclass(obj_or_cls, cls_or_tuple)
    )
```

---

_Comment by @carljm on 2026-01-06 17:02_

@dangotbanned I'm not seeing anything there that I think should give us problems, but there definitely might be a bug. Can you also show some examples of using it that should work, particularly if they don't currently? (And in that case, might be good to open a new issue for it, too, since this issue is just about the validation.)

(I realized yesterday that generic `TypeIs` functions are a case where a multi-argument `TypeIs` function can be sound and sensible -- so I was just wrong to suggest that they should be single-argument.)

---
