---
number: 2336
title: decorator that uses paramspec breaks generics on decorated function
type: issue
state: open
author: DetachHead
labels:
  - generics
  - callables
assignees: []
created_at: 2026-01-05T07:38:32Z
updated_at: 2026-01-07T09:58:09Z
url: https://github.com/astral-sh/ty/issues/2336
synced_at: 2026-01-10T01:51:14Z
---

# decorator that uses paramspec breaks generics on decorated function

---

_Issue opened by @DetachHead on 2026-01-05 07:38_

### Summary

```py
from collections.abc import Callable

def fn[**P, T](value: Callable[P, T]) ->  Callable[P, T]:
    return value

@fn  # errors go away when this decorator is removed
def foo[T](value: T) -> T:
    return value

a: int = foo(1) # errors
```
```
Object of type `T@foo` is not assignable to `int` (invalid-assignment) [Ln 10, Col 10]
Argument is incorrect: Expected `T@foo`, found `Literal[1]` (invalid-argument-type) [Ln 10, Col 14]
```
https://play.ty.dev/a7e1e83f-6c64-4c50-bf7d-fc7e32fccee8

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Comment by @carljm on 2026-01-06 01:02_

I feel like we have another bug open for this, or a variant of it, but not finding it at the moment. Thanks for the report!

---

_Label `generics` added by @carljm on 2026-01-06 01:02_

---

_Label `callables` added by @carljm on 2026-01-06 01:03_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-06 01:03_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-06 15:32_

---

_Comment by @hzechr on 2026-01-07 09:58_

Just ran across a related issue (discovered this "in the wild" with the `deprecate_renamed_parameter` decorator in the `polars` dataframe library)
```python
from collections.abc import Callable

def test[**P, T]() -> Callable[[Callable[P, T]], Callable[P, T]]:
    return lambda f: f

@test()
def ident[T](x: T) -> T:
    return x

y = ident(2)
```
Here, there is no error but `y` is determined as *unknown* while it should be determined as *int* (replacing *int* with *str* or other types does not change this).

https://play.ty.dev/a10d191c-6a23-41f5-82b4-7a9338fc1ccb

---
