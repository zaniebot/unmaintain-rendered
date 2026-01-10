---
number: 1799
title: "`del x[k]` should be allowed iff `x` can `__delitem__`"
type: issue
state: closed
author: jorenham
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-12-07T20:26:45Z
updated_at: 2025-12-24T03:34:22Z
url: https://github.com/astral-sh/ty/issues/1799
synced_at: 2026-01-10T01:52:52Z
---

# `del x[k]` should be allowed iff `x` can `__delitem__`

---

_Issue opened by @jorenham on 2025-12-07 20:26_

### Summary

With

```py
from typing import Protocol

class CanDelItem[KT](Protocol):
    def __delitem__(self, k: KT, /): ...

def f[KT](x: CanDelItem[KT], k: KT, /) -> None:
    del x[k]  # ❌ 
```

ty reports

```
Cannot subscript object of type `CanDelItem[KT@f]` with no `__getitem__` method (non-subscriptable) [Ln 7, Col 9]
```

even though it's valid at runtime:

```pycon
>>> class A:
...     def __delitem__(self, k):
...         print(f"del {self}[{k!r}]")
...         
>>> a = A()
>>> del a["spam"]
del <__main__.A object at 0x790ad544b4d0>['spam']
```

https://play.ty.dev/90b54f57-a0e7-4fd4-8597-4287764c5d7b

### Version

ty 0.0.1-alpha.32

---

_Label `bug` added by @AlexWaygood on 2025-12-07 21:54_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-12-07 21:54_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-07 21:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-20 23:03_

---

_Closed by @charliermarsh on 2025-12-24 03:34_

---
