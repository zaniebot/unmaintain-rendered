```yaml
number: 1210
title: Class without explicit constructor is not callable
type: issue
state: closed
author: ibraheemdev
labels: []
assignees: []
created_at: 2025-09-19T19:15:03Z
updated_at: 2025-09-23T00:26:26Z
url: https://github.com/astral-sh/ty/issues/1210
synced_at: 2026-01-12T15:54:24Z
```

# Class without explicit constructor is not callable

---

_@ibraheemdev_

The following example currently errors:

```py
from typing import Callable

class X:
    ...

def f(callable: Callable[[], X]) -> X:
    return callable()

x = f(X)
```
```py
error[invalid-argument-type]: Argument to function `f` is incorrect
 --> x.py:9:7
  |
7 |     return callable()
8 |
9 | x = f(X)
  |       ^ Expected `() -> X`, found `<class 'X'>`
  |
```

This works if an explicit `__new__` or `__init__` method is added. 

---

_Closed by @carljm on 2025-09-23 00:26_

---
