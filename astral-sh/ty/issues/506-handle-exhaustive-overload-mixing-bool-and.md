```yaml
number: 506
title: "Handle exhaustive overload mixing `bool` and `Literal[True]`/`Literal[False]`"
type: issue
state: closed
author: flying-sheep
labels:
  - bug
  - overloads
assignees: []
created_at: 2025-05-25T11:01:49Z
updated_at: 2025-05-26T15:05:05Z
url: https://github.com/astral-sh/ty/issues/506
synced_at: 2026-01-10T02:34:10Z
```

# Handle exhaustive overload mixing `bool` and `Literal[True]`/`Literal[False]`

---

_Issue opened by @flying-sheep on 2025-05-25 11:01_

### Summary

Original mypy issue: https://github.com/python/mypy/issues/14764#issuecomment-2690976768

Playground link: https://play.ty.dev/18477e1d-cea2-48db-b07f-ca7eb4394f7d

Unclear if this is a duplicate of #216, but I thought I might mention it.

```py
from typing import overload, Literal

class ndarray: ...
class DaskArray: ...

@overload
def to_dense(a: ndarray, *, to_memory: bool = False) -> ndarray: ...
@overload
def to_dense(a: DaskArray, *, to_memory: Literal[False] = False) -> DaskArray: ...
@overload
def to_dense(a: DaskArray, *, to_memory: Literal[True]) -> ndarray: ...

def to_dense(*args: object, **kw: object) -> object: ...


def main(x: ndarray | DaskArray, to_memory: bool) -> None:
    to_dense(x, to_memory=to_memory)
    # No overload of function `to_dense` matches arguments (no-matching-overload) [Ln 17, Col 5]
```

### Version

0.0.0 (the playground doesn’t say anything more specific)

---

_Label `overloads` added by @AlexWaygood on 2025-05-25 11:04_

---

_Label `bug` added by @sharkdp on 2025-05-26 11:31_

---

_Comment by @sharkdp on 2025-05-26 11:34_

Thank you for reporting this!

Here's a slightly smaller example that focuses on the core issue here:
```py
from typing import overload, Literal

@overload
def f(arg: Literal[False] = False) -> int: ...
@overload
def f(arg: Literal[True]) -> str: ...

def f(arg: bool) -> int | str:
    return "a" if arg else 0


def _(flag: bool) -> None:
    f(flag)
```
https://play.ty.dev/4dbdf5f0-ed07-4f5a-9609-df3a5da9b3d6


It looks closely related to https://github.com/astral-sh/ty/issues/468, but it's probably worth keeping this in mind, since the "union" type here is only implicit, not explicit (`bool = Literal[False] | Literal[True]`).

---

_Comment by @dhruvmanila on 2025-05-26 14:40_

Thanks, this is same as #468 as `bool` will be expanded into `Literal[True]` and `Literal[False]` (ref https://typing.python.org/en/latest/spec/overload.html#argument-type-expansion).

---

_Comment by @sharkdp on 2025-05-26 15:05_

Okay. If it's on your mind and explicitly mentioned in the spec, I think we should close this in favor of #468 

---

_Closed by @sharkdp on 2025-05-26 15:05_

---
