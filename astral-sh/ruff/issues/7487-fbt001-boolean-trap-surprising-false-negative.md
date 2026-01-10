---
number: 7487
title: "FBT001 Boolean trap surprising false negative with `bool | None`"
type: issue
state: closed
author: leddy231
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-09-18T09:49:35Z
updated_at: 2023-11-12T20:09:24Z
url: https://github.com/astral-sh/ruff/issues/7487
synced_at: 2026-01-10T01:22:47Z
---

# FBT001 Boolean trap surprising false negative with `bool | None`

---

_Issue opened by @leddy231 on 2023-09-18 09:49_

`FBT001` should catch boolean positional arguments in function definitions, but to my surprise it does not care about  `bool | None`, which I would expect it to do. Thinking about it, I would expect it to catch any union type where `bool` is one of the options.

In both cases, FBT003 triggers on the call to the function, but the function definition itself passes.

```python
def chaught(x: bool) -> bool:
    """FBT0001 trigger."""
    return x


def passes(x: bool | None) -> bool | None:
    """No FBT0001 trigger."""
    return x


chaught(True) #FBT003 trigger
passes(False) #FBT003 trigger
```

---

_Referenced in [astral-sh/ruff#7501](../../astral-sh/ruff/pulls/7501.md) on 2023-09-18 20:17_

---

_Label `rule` added by @charliermarsh on 2023-09-19 02:37_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-19 02:37_

---

_Closed by @charliermarsh on 2023-11-12 20:09_

---
