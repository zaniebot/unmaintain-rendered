```yaml
number: 1824
title: simplify intersections of overlapping covariant generic protocols
type: issue
state: open
author: danielpopescu
labels:
  - narrowing
  - generics
  - type properties
assignees: []
created_at: 2025-12-09T14:04:07Z
updated_at: 2026-01-08T18:52:09Z
url: https://github.com/astral-sh/ty/issues/1824
synced_at: 2026-01-12T15:54:25Z
```

# simplify intersections of overlapping covariant generic protocols

---

_@danielpopescu_

### Summary

Type error in this code

```py
def fold_left[T, U](lst: Iterable[T], init: U, f: Callable[[U, T], U]) -> U:
    def fold_left_it(it: Iterator[T], init: U, f: Callable[[U, T], U]) -> U:
        elem: T | None = next(it, None)
        return fold_left_it(it, f(init, elem), f) if elem is not None else init

    if isinstance(lst, Iterator):
        it: Iterator[T] = lst

        return fold_left_it(it, init, f)
    return fold_left_it(iter(lst), init, f)

```

error ty     invalid-assignment    Object of type `Iterable[T@fold_left] & Iterator[object]` is not assignable to `Iterator[T@fold_left]`

mypy and pyright work fine ....

### Version

_No response_

---

_Label `generics` added by @carljm on 2025-12-09 17:48_

---

_Label `type properties` added by @carljm on 2025-12-09 17:48_

---

_Label `narrowing` added by @carljm on 2025-12-09 17:48_

---

_Added to milestone `Stable` by @carljm on 2025-12-09 17:48_

---

_Renamed from "Iterable, Iterator, isinstance type error" to "simplify intersections of overlapping generic protocols" by @carljm on 2025-12-09 17:49_

---

_Comment by @carljm on 2025-12-09 17:49_

Thanks for the report!

Yeah, this is a bug. We should simplify the intersection `Iterable[T@fold_left] & Iterator[object]` to `Iterator[T@fold_left]`.

The key factor here is that both `Iterable` and `Iterator` share an `__iter__` method. This means that it is not possible for an `Iterable` of one type to also be an `Iterator` of a different type, which means we have to unify the typevars in this intersection. And since both `Iterable` and `Iterator` are covariant, we can unify them, resulting in `Iterable[T@fold_left] & Iterator[T@fold_left]`, which simplifies to `Iterator[T@fold_left]`.

---

_Renamed from "simplify intersections of overlapping generic protocols" to "simplify intersections of overlapping covariant generic protocols" by @carljm on 2026-01-08 18:52_

---
