---
number: 11333
title: PLE0237 false positive for slots literal concatenation 
type: issue
state: closed
author: airallergy
labels:
  - bug
assignees: []
created_at: 2024-05-08T09:28:25Z
updated_at: 2024-05-22T04:18:02Z
url: https://github.com/astral-sh/ruff/issues/11333
synced_at: 2026-01-10T01:22:51Z
---

# PLE0237 false positive for slots literal concatenation 

---

_Issue opened by @airallergy on 2024-05-08 09:28_

I have a class, whose slots are formed by concatenating other literal tuples via RUF005 style. Below `B` is reported to violate PLE0237, which is a false positive, I also tried a few variants of `B`:

- `C`: If `B` is a subclass of any other class, PLE0237 seems to work fine
- `D`: If `B` slots has no other literal items, PLE0237 seems to work fine
- `E`: If `B` slots uses tuple concatenation, which violates RUF005, but PLE0237 seems to work fine

```python
class A:
    pass


class B:
    names = ("x",)
    __slots__ = (*names, "a")

    def __init__(self) -> None:
        self.x = 1  # PLE0237: Attribute `x` is not defined in class's `__slots__`


class C(A):
    names = ("x",)
    __slots__ = (*names, "a")

    def __init__(self) -> None:
        self.x = 1


class D:
    names = ("x",)
    __slots__ = (*names,)

    def __init__(self) -> None:
        self.x = 1


class E:
    names = ("x",)
    __slots__ = names + ("a",)  # RUF005: Consider `(*names, "a")` instead of concatenation

    def __init__(self) -> None:
        self.x = 1
```


---

_Comment by @dhruvmanila on 2024-05-09 11:30_

Thank you for opening this issue! I think we should ignore raising `PLE0237` if `__slots__` contain an unpacked argument.

---

_Label `bug` added by @dhruvmanila on 2024-05-09 11:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 04:00_

---

_Referenced in [astral-sh/ruff#11488](../../astral-sh/ruff/pulls/11488.md) on 2024-05-22 04:12_

---

_Closed by @charliermarsh on 2024-05-22 04:18_

---
