```yaml
number: 409
title: improve diagnostics for other sources of failed calls to overloaded routines
type: issue
state: open
author: BurntSushi
labels:
  - diagnostics
  - calls
  - overloads
assignees: []
created_at: 2025-05-15T15:45:47Z
updated_at: 2025-11-14T14:45:30Z
url: https://github.com/astral-sh/ty/issues/409
synced_at: 2026-01-12T15:54:23Z
```

# improve diagnostics for other sources of failed calls to overloaded routines

---

_@BurntSushi_

This issue builds off of #274. The task here is to improve diagnostics for calls to overloaded routines that aren't "normal" function or method calls. In particular, https://github.com/astral-sh/ruff/pull/18122 improved diagnostics for function/method calls, but didn't cover any other cases.

One such notable case is just

```python
type()
```

Which currently has this diagnostic:

```python
error[no-matching-overload]: No overload of class `type` matches arguments
 --> main.py:1:1
  |
1 | type()
  | ^^^^^^
  |
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
```

Compare that with

```python
from typing import overload

@overload
def f(x: int) -> int: ...

@overload
def f(x: str) -> str: ...

def f(x: int | str) -> int | str:
    return x

f(b"foo")
```

and these diagnostics:

```
error[no-matching-overload]: No overload of function `f` matches arguments
  --> main.py:12:1
   |
10 |     return x
11 |
12 | f(b"foo")
   | ^^^^^^^^^
   |
info: First overload defined here
 --> main.py:4:5
  |
3 | @overload
4 | def f(x: int) -> int: ...
  |     ^^^^^^^^^^^^^^^^
5 |
6 | @overload
  |
info: Possible overloads for function `f`:
info:   (x: int) -> int
info:   (x: str) -> str
info: Overload implementation defined here
  --> main.py:9:5
   |
 7 | def f(x: str) -> str: ...
 8 |
 9 | def f(x: int | str) -> int | str:
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
10 |     return x
   |
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
```

Ideally, the existing diagnostic code path could be used for other cases:

https://github.com/astral-sh/ruff/blob/69393b2e6ee1eec4fc69906f8f6514994218e3b3/crates/ty_python_semantic/src/types/call/bind.rs#L1123-L1127

Specifically, those other cases can be found here:

https://github.com/astral-sh/ruff/blob/466021d5e1793ca30e0d860cb1cd27e32f5233aa/crates/ty_python_semantic/src/types/call/bind.rs#L1504-L1523

Ref https://github.com/astral-sh/ty/issues/274#issuecomment-2881856028

cc @dhruvmanila 

---

_Label `diagnostics` added by @BurntSushi on 2025-05-15 15:45_

---

_Label `calls` added by @BurntSushi on 2025-05-15 15:45_

---

_Label `overloads` added by @BurntSushi on 2025-05-15 15:45_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:45_

---
