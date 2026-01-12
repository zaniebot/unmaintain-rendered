```yaml
number: 2314
title: "False positive [invalid-assignment] on assigning to union with bounded typevar"
type: issue
state: open
author: randolf-scholz
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2026-01-03T10:20:16Z
updated_at: 2026-01-05T22:35:34Z
url: https://github.com/astral-sh/ty/issues/2314
synced_at: 2026-01-12T15:54:26Z
```

# False positive [invalid-assignment] on assigning to union with bounded typevar

---

_@randolf-scholz_

### Summary

https://play.ty.dev/b3098a7e-d5ce-480f-be95-3f4cd7a6d224

```python
def upcast(arg: int) -> int:
    return arg

def test[X: int](items: list[X]) -> list[X]:
    _a: list[int] = list(items)           # ✅️
    _b: list[int] = [*items]              # ✅️
    _c: list[int] = [x for x in items]    # ✅️

    _1: list[str] | list[int] = list(items)         # ❌️
    _2: list[int] | list[str] = list(items)         # ❌️
    _3: list[str] | list[int] = [*items]            # ✅️
    _4: list[int] | list[str] = [*items]            # ✅️
    _5: list[str] | list[int] = [x for x in items]  # ✅️
    _6: list[int] | list[str] = [x for x in items]  # ✅️
    _7: list[str] | list[int] = [upcast(x) for x in items]  # ✅️
    _8: list[int] | list[str] = [upcast(x) for x in items]  # ✅️
    return items
```

This reports

> Object of type `list[X@test]` is not assignable to `list[str] | list[int]

But at the same time, it reports `list` is `[T] (Iterable[T]) -> list[T]`.

So in `_1` and `_2` we have joint constraints:

- `X <: T` (`list[X]` must be assignable to `Iterable[T]`), 
- `X <: int` upper bound
- `T = int` or `T=str` (`list[T] <: list[int] | list[str]`)

So, this has the unique solution `T=int`, but `ty` seems to incorrectly select `T=X`.

---

_Comment by @ElVynz on 2026-01-03 23:55_

I have encountered the same issue in a very similar case (https://play.ty.dev/77b3c10e-0aeb-4f2f-8007-31b79671733e)

---

_Comment by @carljm on 2026-01-04 22:51_

Thanks for the report!

---

_Added to milestone `Stable` by @carljm on 2026-01-04 22:51_

---

_Label `bidirectional inference` added by @carljm on 2026-01-04 22:51_

---

_Label `generics` added by @carljm on 2026-01-04 22:51_

---

_Comment by @Hugo-Polloli on 2026-01-05 22:16_

If that is alright, I would like to try to fix this one :)

---

_Comment by @carljm on 2026-01-05 22:35_

> If that is alright, I would like to try to fix this one :)

Definitely!

---

_Assigned to @Hugo-Polloli by @carljm on 2026-01-05 22:35_

---
