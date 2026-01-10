---
number: 2394
title: "0.0.9 -> 0.0.10 type checking regressions"
type: issue
state: closed
author: danielpopescu
labels: []
assignees: []
created_at: 2026-01-08T13:03:50Z
updated_at: 2026-01-09T02:00:43Z
url: https://github.com/astral-sh/ty/issues/2394
synced_at: 2026-01-10T01:48:23Z
---

# 0.0.9 -> 0.0.10 type checking regressions

---

_Issue opened by @danielpopescu on 2026-01-08 13:03_

### Summary

The following code:


```py
from __future__ import annotations

from collections.abc import Callable, Iterable, Iterator


def fold_left[T, U](lst: Iterable[T], init: U, f: Callable[[U, T], U]) -> U:
    def fold_left_it(it: Iterator[T], init: U, f: Callable[[U, T], U]) -> U:
        elem: T | None = next(it, None)
        return fold_left_it(it, f(init, elem), f) if elem is not None else init

    if isinstance(lst, Iterator):
        it: Iterator[T] = lst
        return fold_left_it(it, init, f)

    return fold_left_it(iter(lst), init, f)



def reverse[T](lst: list[T]) -> list[T]:
    empty: list[T] = []
    return fold_left(lst, empty, lambda rlst, e: [e, *rlst])


def list_zipping[T](lst: list[T]) -> list[T]:
    rst = reverse(lst)
    empty: list[T] = []
    zlst = fold_left(range(len(lst) // 2), empty, lambda ret, i: [*ret, lst[i], rst[i]])
    return zlst if len(zlst) == len(lst) else [*zlst, lst[len(lst) // 2]]

```

showed only 1 "error" (issue already reported previously) in 0.0.9, but in 0..0.10 there are 4 new "errors".

The code above checks cleanly using pyright/mypy.

[Playground](https://play.ty.dev/7bd0788b-e638-4db0-8a90-e6a6b04ffb4b)

### Version

_No response_

---

_Renamed from "0.0.9 ->0.0.10 type checking regressions" to "0.0.9 -> 0.0.10 type checking regressions" by @danielpopescu on 2026-01-08 15:23_

---

_Comment by @carljm on 2026-01-09 02:00_

This is fixed in main (by https://github.com/astral-sh/ruff/pull/22444 ) and will be fixed in the next release. (We still have the originally reported one error, but that's tracked separately.) Thanks for the report!

---

_Closed by @carljm on 2026-01-09 02:00_

---
