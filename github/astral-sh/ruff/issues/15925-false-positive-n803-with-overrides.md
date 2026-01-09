---
number: 15925
title: False-positive N803 with overrides
type: issue
state: closed
author: bersbersbers
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-02-04T08:33:49Z
updated_at: 2025-02-05T08:35:58Z
url: https://github.com/astral-sh/ruff/issues/15925
synced_at: 2026-01-07T13:12:16-06:00
---

# False-positive N803 with overrides

---

_Issue opened by @bersbersbers on 2025-02-04 08:33_

### Description

This is my code. `BadClass` is bad, but I have to live with it:

```python
from typing import override


class BadClass:
    def fun(self, DaTa: int) -> None: ...  # noqa: N803


class MyClass(BadClass):
    @override
    def fun(self, DaTa: int) -> None: ...
```

`ruff --isolated check --select=N803 bug.py` (v0.9.4) gives:

```
>ruff --isolated check --select=N803 bug.py
bug.py:10:19: N803 Argument name `DaTa` should be lowercase
   |
 8 | class MyClass(BadClass):
 9 |     @override
10 |     def fun(self, DaTa: int) -> None: ...
   |                   ^^^^^^^^^ N803
   |
```

However, I'd argue I have to name that argument that way - otherwise, `pyright` tells me:
```
c:\Git\project\bug.py
  c:\Git\project\bug.py:10:9 - error: Method "fun" overrides class "BadClass" in an incompatible manner
    Parameter 2 name mismatch: base parameter is named "DaTa", override parameter is named "data" (reportIncompatibleMethodOverride)
1 error, 0 warnings, 0 informations
```

So I'd argue that N803 should ignore functions with `@override` (as well as their `@overload`s!).

---

_Label `bug` added by @MichaReiser on 2025-02-04 08:48_

---

_Comment by @MichaReiser on 2025-02-04 08:48_

Thanks, skipping `N803` for overriden methods does make sense to me.

---

_Label `rule` added by @MichaReiser on 2025-02-04 08:48_

---

_Label `help wanted` added by @MichaReiser on 2025-02-04 08:49_

---

_Comment by @VascoSch92 on 2025-02-04 12:54_

Regarding as well as their @overloads!, I believe this is not possible since the linter only has access to one script at a time. This means that if the base method is in a different script from its overloads, it cannot verify that one is overriding the other. (@MichaReiser, is that correct? 🙂)

That said, skipping N803 for overridden methods seems like a good idea.

---

_Comment by @MichaReiser on 2025-02-04 13:01_

Yes, Ruff can't check today if the decorated method actually overrides a method (in the correct way) from a parent class but we can just trust the developer here. 

We plan to implement a rule similar to pyright's for Red Knot, our upcoming type checker. 

---

_Referenced in [astral-sh/ruff#15954](../../astral-sh/ruff/pulls/15954.md) on 2025-02-05 00:12_

---

_Closed by @MichaReiser on 2025-02-05 08:35_

---

_Closed by @MichaReiser on 2025-02-05 08:35_

---
