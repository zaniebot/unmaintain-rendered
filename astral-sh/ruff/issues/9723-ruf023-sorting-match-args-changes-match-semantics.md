```yaml
number: 9723
title: "RUF023: Sorting `__match_args__` changes match semantics"
type: issue
state: closed
author: dosisod
labels:
  - bug
  - rule
assignees: []
created_at: 2024-01-30T22:02:54Z
updated_at: 2024-01-30T22:44:51Z
url: https://github.com/astral-sh/ruff/issues/9723
synced_at: 2026-01-10T11:09:51Z
```

# RUF023: Sorting `__match_args__` changes match semantics

---

_Issue opened by @dosisod on 2024-01-30 22:02_

RUF023 should not sort `__match_args__` because the order of `__match_args__` affects which elements are matched against when using positional args in class patterns.

Take the following code example:

```python
from dataclasses import dataclass

@dataclass
class UnaryExpr:
    __match_args__ = ("op", "lhs")

    op: str
    lhs: int


def is_minus_one(expr):
    match expr:
        case UnaryExpr("-", 1):
            return True

    return False


is_minus_one(UnaryExpr("-", 1))  # True
is_minus_one(UnaryExpr("+", 1))  # False
```

Ruff emits the following errors:

```
$ ruff --preview --select RUF023 file.py
file.py:6:22: RUF023 [*] `UnaryExpr.__match_args__` is not sorted
```

If you change `__match_args__` to `("lhs", "op")`, the semantics of the match statement changes, and now both `is_minus_one` calls return `False`.

---

_Comment by @AlexWaygood on 2024-01-30 22:06_

Oof, I can't believe I forgot that this was how things worked. Thanks!

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-01-30 22:06_

---

_Label `bug` added by @AlexWaygood on 2024-01-30 22:06_

---

_Label `rule` added by @AlexWaygood on 2024-01-30 22:06_

---

_Closed by @AlexWaygood on 2024-01-30 22:44_

---
