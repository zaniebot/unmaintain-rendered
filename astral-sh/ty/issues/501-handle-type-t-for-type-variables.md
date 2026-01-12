```yaml
number: 501
title: "Handle `type[T]` for type variables"
type: issue
state: closed
author: DouweM
labels:
  - generics
  - type-inference
assignees: []
created_at: 2025-05-23T21:11:03Z
updated_at: 2025-11-28T08:20:25Z
url: https://github.com/astral-sh/ty/issues/501
synced_at: 2026-01-12T15:54:23Z
```

# Handle `type[T]` for type variables

---

_@DouweM_

### Summary

The following works in pyright, but not in pyrefly, mypy, or (which is why I'm here!) ty:

```py
from __future__ import annotations

from dataclasses import dataclass

from typing_extensions import (
    Generic,
    Sequence,
    TypeVar,
    assert_type,
)

T = TypeVar("T")


@dataclass
class Agent(Generic[T]):
    output_type: Sequence[type[T]]


class Foo:
    pass


class Bar:
    pass


# pyright - works
# mypy - error: Expression is of type "Agent[object]", not "Agent[Foo | Bar]"  [assert-type]
# pyrefly - assert_type(Agent[Foo], Agent[Bar | Foo]) failed + Argument `list[type[Bar] | type[Foo]]` is not assignable to parameter `output_type` with type `Sequence[type[Foo]]` in function `Agent.__init__`
# ty - `Agent[Foo | Bar]` and `Agent[Unknown]` are not equivalent types
assert_type(Agent([Foo, Bar]), Agent[Foo | Bar])

# pyright - works
# mypy - error: Expression is of type "Agent[Never]", not "Agent[int | str]"  [assert-type]
# pyrefly - assert_type(Agent[int], Agent[str | int]) failed + Argument `list[type[str] | type[int]]` is not assignable to parameter `output_type` with type `Sequence[type[int]]` in function `Agent.__init__`
# ty - `Agent[int | str]` and `Agent[Unknown]` are not equivalent types
assert_type(Agent([int, str]), Agent[int | str])

# works
assert_type(Agent[Foo | Bar]([Foo, Bar]), Agent[Foo | Bar])

# works
assert_type(Agent[int | str]([int, str]), Agent[int | str])
```

It would be great to see this work in ty, but if there's a good reason the other 2 out of 3 typecheckers don't support this I'd love to understand why!

- This is related to a new PydanticAI feature, if you're curious check out https://github.com/pydantic/pydantic-ai/pull/1785#issuecomment-2905774110
- Issues for other type checkers:
  - https://github.com/facebook/pyrefly/issues/345
  - https://github.com/python/mypy/issues/19142

### Version

ty 0.0.1-alpha.6

---

_Renamed from "Sequence of types" to "Handle `type[T]` for type variables" by @dcreager on 2025-05-24 13:36_

---

_Comment by @dcreager on 2025-05-24 13:37_

Thanks for the report! The underlying issue is that we don't yet support `type[T]` for type variables. We didn't have a tracking issue for that yet, so I've updated the issue title and we'll use this one to track that feature.

---

_Label `generics` added by @dcreager on 2025-05-24 13:37_

---

_Label `type-inference` added by @dcreager on 2025-05-24 13:37_

---

_Comment by @AlexWaygood on 2025-08-04 10:57_

#783 looks like a duplicate of this issue (but I'm leaving it open for now as I think it'll be easier for users to find it on the issue tracker)

---

_Added to milestone `GA` by @carljm on 2025-11-10 17:31_

---

_Comment by @Avasam on 2025-11-11 17:28_

This may be related according to Alex:

Here's a simple example where passing a variable annotated with a constrained `TypeVar` to `type`, then instantiating it, doesn't match the `TypeVar` I started with (but instead only expects the constrained types):
```py
from typing import TypeVar

FileFlagValueT = TypeVar("FileFlagValueT", str, int, float)

# Simplified, see below for the real-world example
def foo(default_value: FileFlagValueT, value: str) -> FileFlagValueT:
    return type(default_value)(value)
```

Results in:
```
error[invalid-return-type]: Return type does not match returned value
   |
36 | def foo(default_value: FileFlagValueT, value: str) -> FileFlagValueT:
   |                                                       -------------- Expected `FileFlagValueT@foo` because of return type
37 |     return type(default_value)(value)
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `FileFlagValueT@foo`, found `str | int | float`
   |
info: rule `invalid-return-type` is enabled by default
```

Real-world example, where I extract a bit of string, and see if can be parsed as another type. But also using the fallback value's  type directly to avoid having to pass in a constructor separately. This should be "type-safe" as long as the `__new__/__init__` methods of the constrained types all accept a `str` as its lone parameter.
```py
def __value_from_filename(
    filename: str,
    delimiters: str,
    default_value: FileFlagValueT,
) -> FileFlagValueT:
    if len(delimiters) != 2:
        raise ValueError("delimiters parameter must contain exactly 2 characters")
    try:
        string_value = filename.split(delimiters[0], 1)[1].split(delimiters[1])[0]
        value = type(default_value)(string_value)
    except (IndexError, ValueError):
        return default_value
    else:
        return value
```

mypy and pyright both pass here.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-11-14 08:14_

---

_Removed from milestone `Stable` by @ibraheemdev on 2025-11-17 22:50_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-11-17 22:50_

---

_Closed by @sharkdp on 2025-11-28 08:20_

---
