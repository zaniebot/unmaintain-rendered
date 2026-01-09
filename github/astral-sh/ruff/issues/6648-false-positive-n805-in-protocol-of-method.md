---
number: 6648
title: "False-positive N805 in `Protocol` of method"
type: issue
state: open
author: bersbersbers
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2023-08-17T10:52:17Z
updated_at: 2023-09-07T08:50:14Z
url: https://github.com/astral-sh/ruff/issues/6648
synced_at: 2026-01-07T13:12:15-06:00
---

# False-positive N805 in `Protocol` of method

---

_Issue opened by @bersbersbers on 2023-08-17 10:52_

```python
from typing import Protocol

class Class:
    def method(self) -> None:
        ...

class Method(Protocol):
    def __call__(self_, self: Class) -> None:
        ...

method: Method = Class.method
```

`ruff --isolated --select N805 bug.py` (0.0.284) gives
> bug.py:8:18: N805 First argument of a method should be named `self`

I think `ruff` is correct, *but* mypy seems to require that `self` match the name of the variable in `Class.method` (compare https://github.com/python/typing/discussions/1040), so we have to use `self_` (or anything else) as a first parameter in `Method.method`.

One alternative that also type-checks (but throws the same ruff error N805, just at a different location), is

```python
from typing import Protocol

class Class:
    def method(self_) -> None:
        ...

class Method(Protocol):
    def __call__(self, self_: Class) -> None:
        ...

method: Method = Class.method
```

---

_Comment by @charliermarsh on 2023-08-17 15:46_

Ah interesting. I'm not sure what the best solution is here. Allow `self_` if the second argument to the method is `self` in a protocol?

---

_Label `bug` added by @charliermarsh on 2023-08-17 17:10_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-17 17:10_

---

_Comment by @charliermarsh on 2023-08-17 17:10_

Or always allow `self_` in `__call__`?

---

_Comment by @bersbersbers on 2023-08-17 20:04_

Or both, allow `self_` if the second argument to the method is `self` in `__call__` in a protocol? I don't know, to be honest.

---

_Comment by @SXHRYU on 2023-09-07 07:30_

I don't think it should be considered a false positive. Nowhere in mypy docs nor python docs is `self_` used. On the contrary, both docs contain valid examples with `Protocol` classes with methods that have `self` passed as first argument.

You can find topics online regarding usage of `self_` in callable protocols, but they all seem to be more of a workaround or hacking things together (some are done simply for typing reasons, which is more of an **extension** of python than core functionality). A simple `noqa: N805` should be enough.

---

_Comment by @bersbersbers on 2023-09-07 08:48_

>  On the contrary, both docs contain valid examples with Protocol classes with methods that have self passed as first argument.

I would love to see those in the mypy docs - I cannot seem to find any. Note that this is about typing the method, not the class, which is why there are two different versions of `self`, one referring to the instance holding the method, and another one referring to to the instance of the Protocol.

> You can find topics online regarding usage of self_ in callable protocols, but they all seem to be more of a workaround or hacking things together (some are done simply for typing reasons, which is more of an extension of python than core functionality).

I would argue that the whole point of `Protocol`s is typing reasons, see PEP 544. Saying that `self_` should be flagged because it is used only for typing reasons basically comes down to saying `Protocol` could be flagged for whatever it is doing, because it is used only for typing reasons.


---
