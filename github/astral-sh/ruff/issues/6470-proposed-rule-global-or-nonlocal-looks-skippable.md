---
number: 6470
title: "Proposed rule: `global` or `nonlocal` looks skippable but applies to the whole function"
type: issue
state: open
author: andersk
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-08-10T05:54:01Z
updated_at: 2023-08-10T12:52:51Z
url: https://github.com/astral-sh/ruff/issues/6470
synced_at: 2026-01-07T13:12:15-06:00
---

# Proposed rule: `global` or `nonlocal` looks skippable but applies to the whole function

---

_Issue opened by @andersk on 2023-08-10 05:54_

In this example, the `global` declaration is misleading, because Python actually applies it to the whole function, not just the `if` branch it’s inside.

```python
g = 1

def f(b: bool) -> None:
    if b:
        global g
        g = 2
    else:
        g = 3

f(False)
print(g)  # 3, not 1
```

The specific rule I would propose is as follows:

- If there’s a `global g` or `nonlocal g` declaration at location X, and a reference to `g` at location Y, and there exists a syntactic control flow path from the function start to Y that doesn’t go through X, then raise a diagnostic “`global g` looks skippable but applies to the whole function”.

(This formulation avoids unnecessarily flagging the above example if there were no `else: g = 3` branch.)

---

_Comment by @charliermarsh on 2023-08-10 12:52_

(This would be a good rule to leverage our control-flow graph.)

---

_Label `rule` added by @charliermarsh on 2023-08-10 12:52_

---

_Label `accepted` added by @charliermarsh on 2023-08-10 12:52_

---

_Referenced in [astral-sh/ruff#19569](../../astral-sh/ruff/pulls/19569.md) on 2025-07-26 17:25_

---
