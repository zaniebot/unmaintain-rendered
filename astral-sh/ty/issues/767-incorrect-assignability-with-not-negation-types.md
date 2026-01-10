```yaml
number: 767
title: "Incorrect assignability with Not[] (negation types) and gradual types"
type: issue
state: closed
author: JelleZijlstra
labels:
  - type properties
assignees: []
created_at: 2025-07-05T15:34:15Z
updated_at: 2025-10-10T11:10:19Z
url: https://github.com/astral-sh/ty/issues/767
synced_at: 2026-01-10T02:06:24Z
```

# Incorrect assignability with Not[] (negation types) and gradual types

---

_Issue opened by @JelleZijlstra on 2025-07-05 15:34_

### Summary

Given [this program](https://play.ty.dev/1d50ef59-232d-4e3b-8166-c1dfbc85c09e):

```python
from ty_extensions import Not, Intersection
from typing import Any

def f(y: Not[tuple[Any]]):
    pass

def caller(lst: tuple[int], unannotated):
    f(lst)
    f((unannotated,))
    f((1,))
```

`ty` rejects all three calls to `f()`. This is incorrect by my understanding of gradual negation types: `Not[tuple[Any]]` could materialize to e.g. `Not[tuple[str]]`, and `tuple[int]` is assignable to `Not[tuple[str]]`.

If we [change](https://play.ty.dev/9c026880-c0f3-4cf2-9734-2e5923990fe8) `f` to take `Not[tuple[str]]`, ty accepts the first and third call but not the second (`f((unannotated,))`). This is also incorrect: `unannotated` could be of some type that makes the tuple disjunct with `Not[tuple[str]]`.

---

I haven't attempted to fully implement this, but here's my thinking on how assignability involving gradual types and negation types could be implemented:

If `T` is a non-static (gradual) type, then `~T` can be transformed into a type of the form `T | ~Top[T]`, where `Top[T]` is the top materialization of `T` (i.e., the union of all possible materializations). That is, the negation of a gradual type consists of the same gradual type, plus any type that is not part of any materialization of the gradual type. `Top[T]` is a fully static type, which means that we don't need to worry about assigning to a negated gradual type, only to a negated fully static type.

Next let's consider assignability of a type `U` to a negated type `~T`. If `U` is a fully static type, then we can consider whether the intersection `T & U` is inhabited. If it is, then `U` is not assignable to `~T`. If `U` is a non-static type, then it is assignable to `~T` if every materialization of `U` is disjoint from `T`. If there is a bottom materialization of `U` (a materialization that is a subtype of every other materialization), then you could check whether `Bottom[U]` is disjoint from `T`. But in Python, the bottom materialization of many gradual types is `Never`, which is disjoint from everything. (Test case: `list[Any]` should not be assignable to `~Sequence[object]`.) I think in practice, you might be able to get this to work by constructing something like the bottom materialization and just not simplifying it to Never, maybe by replacing Any with something that is considered disjoint from every other type.

---

The practical significance of all this is limited, since negation types in practice mostly come up in the negative arms of type narrowing constructs, which means we mostly only have to worry about assigning *from* negation types, not assigning *to* negation types.

However, there are cases involving contravariance where the type checker needs to check assignability to a negation type even when we don't use an explicit Not type. Here's an [example](https://play.ty.dev/961bf1dc-c3fb-47e2-9af3-c7a941f6e4b1) of a program using implicit negation types that ty incorrectly rejects (mypy and pyright accept it):

```python
from typing import Generic, TypeVar, Any, TypeIs

T_contra = TypeVar("T_contra", contravariant=True)

class X(Generic[T_contra]):
    def __init__(self, x: T_contra) -> None:
        pass

    def send(self, x: T_contra) -> None:
        pass

def f(x: X[tuple[Any]]):
    pass

def is_interesting_tuple(o: object) -> TypeIs[tuple[Any]]:
    return isinstance(o, tuple) and len(o) == 1 and len(str(o)) % 3 == 0

def doit(o: object):
    if not is_interesting_tuple(o):
        x = X(o)
        # x is of type x[~tuple[Any]]
        f(x)  # ty: Argument to function `f` is incorrect: Expected `X[tuple[Any]]`, found `X[~tuple[Any]]`
```

### Version

_No response_

---

_Label `type properties` added by @AlexWaygood on 2025-07-05 16:00_

---

_Closed by @AlexWaygood on 2025-10-10 11:10_

---
