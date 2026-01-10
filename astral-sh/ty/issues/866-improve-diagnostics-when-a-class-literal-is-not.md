---
number: 866
title: "Improve diagnostics when a class literal is not assignable to a `Callable` type"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-07-21T19:04:38Z
updated_at: 2026-01-09T01:50:46Z
url: https://github.com/astral-sh/ty/issues/866
synced_at: 2026-01-10T01:48:23Z
---

# Improve diagnostics when a class literal is not assignable to a `Callable` type

---

_Issue opened by @AlexWaygood on 2025-07-21 19:04_

Consider this snippet:

```py
from typing import Callable, Any

def f(x: Callable[[Any], Any]): ...

class Foo:
    def __init__(self, x, y): ...

f(Foo)
```

Ty [emits this diagnostic](https://play.ty.dev/0e830866-dca5-4fdf-89eb-c0e5219ccf06) on it, which isn't very useful:

```py
error[invalid-argument-type]: Argument to function `f` is incorrect
 --> foo.py:8:3
  |
6 |     def __init__(self, x, y): ...
7 |
8 | f(Foo)
  |   ^^^ Expected `(Any, /) -> Any`, found `<class 'Foo'>`
  |
info: Function defined here
 --> foo.py:3:5
  |
1 | from typing import Callable, Any
2 |
3 | def f(x: Callable[[Any], Any]): ...
  |     ^ ----------------------- Parameter declared here
4 |
5 | class Foo:
  |
info: rule `invalid-argument-type` is enabled by default
```

It just tells me that `<class 'Foo'>` isn't acceptable, but it doesn't tell me _why_ `<class 'Foo'>` isn't acceptable. It would be much more helpful if ty told me what `Callable` supertype it thinks `Foo` _does_ inhabit. (Here I assume it's `Callable[[Any, Any], Foo]`, which isn't assignable to `Callable[[Any], Any]`.)

---

_Label `diagnostics` added by @AlexWaygood on 2025-07-21 19:04_

---

_Comment by @dhruvmanila on 2025-07-24 04:40_

I think this would a sub-task of https://github.com/astral-sh/ty/issues/163 ?

---

_Added to milestone `Stable` by @carljm on 2026-01-09 01:50_

---
