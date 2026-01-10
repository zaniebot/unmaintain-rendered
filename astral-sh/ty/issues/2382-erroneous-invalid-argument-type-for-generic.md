---
number: 2382
title: "Erroneous `invalid-argument-type` for generic contextmanager with ParamSpec"
type: issue
state: open
author: bcmills
labels:
  - callables
assignees: []
created_at: 2026-01-07T21:13:37Z
updated_at: 2026-01-09T10:11:42Z
url: https://github.com/astral-sh/ty/issues/2382
synced_at: 2026-01-10T01:48:23Z
---

# Erroneous `invalid-argument-type` for generic contextmanager with ParamSpec

---

_Issue opened by @bcmills on 2026-01-07 21:13_

### Summary

For this program, `ty` produces this error:
```
Argument is incorrect: Expected `(**_P@_inner) -> None`, found `(**_P@outer) -> None` (invalid-argument-type) [Ln 16, Col 13]
```

Since `_P@_inner` and `_P@outer` are the _same_ `_P` in the same parameter position, they are obviously compatible. It isn't clear to me why `ty` is treating them as somehow incompatible.

---

https://play.ty.dev/f8330b63-ba0f-455e-b11b-f67c3016be21:
```py
import contextlib
from collections.abc import Callable, Iterator
from typing import ParamSpec

_P = ParamSpec("_P")

class Foo:
    @contextlib.contextmanager
    def outer(
        self,
        probe: Callable[_P, None],
        *args: _P.args,
        **kwargs: _P.kwargs,
    ) -> Iterator[None]:
        with self._inner(
            probe=probe,
        ):
            yield
    
    @contextlib.contextmanager
    def _inner(
        self,
        probe: Callable[_P, None],
        *args: _P.args,
        **kwargs: _P.kwargs,
    ) -> Iterator[None]:
        probe(*args, **kwargs)
        yield
```

### Version

ty 0.0.9

---

_Comment by @MeGaGiGaGon on 2026-01-07 21:45_

This is happening because you didn't tell ty that `Foo` is generic over `_P`, so it assumes that the methods are the things that are generic, hence the two uses being different. By making the class generic over `_P`, it works properly:

https://play.ty.dev/688e719b-6c72-4341-9944-dfabdda29c04
```py
import contextlib
from collections.abc import Callable, Iterator
from typing import ParamSpec, Generic

_P = ParamSpec("_P")

class Foo(Generic[_P]):
    @contextlib.contextmanager
    def outer(
        self,
        probe: Callable[_P, None],
        *args: _P.args,
        **kwargs: _P.kwargs,
    ) -> Iterator[None]:
        with self._inner(
            probe=probe,
        ):
            yield
    
    @contextlib.contextmanager
    def _inner(
        self,
        probe: Callable[_P, None],
        *args: _P.args,
        **kwargs: _P.kwargs,
    ) -> Iterator[None]:
        probe(*args, **kwargs)
        yield
```

---

_Comment by @bcmills on 2026-01-07 21:55_

`Foo` is _not_ generic over `_P`. Only its methods are.

---

_Comment by @MeGaGiGaGon on 2026-01-07 22:17_

Thinking about this more you should be right - it looks like the decorator is whats causing the issue, as without it it works fine:
https://play.ty.dev/6a2a1695-36f8-4252-bc4e-b81e9dda8237

---

_Label `callables` added by @carljm on 2026-01-07 23:12_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-07 23:12_

---

_Comment by @carljm on 2026-01-08 18:11_

This might have the same root cause as #2336?

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-09 10:11_

---
