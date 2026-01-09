---
number: 14866
title: "[red-knot] Support narrowing for assignment expressions"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-12-09T11:36:08Z
updated_at: 2025-04-18T00:28:07Z
url: https://github.com/astral-sh/ruff/issues/14866
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Support narrowing for assignment expressions

---

_Issue opened by @sharkdp on 2024-12-09 11:36_

Narrowing should also be supported if the target is inside an assignment expression. For example:

```py
def f() -> int | None: ...

if (x := f()) is not None:
    reveal_type(x)  # should be `int`
```

---

_Label `red-knot` added by @sharkdp on 2024-12-09 11:36_

---

_Referenced in [astral-sh/ruff#17437](../../astral-sh/ruff/issues/17437.md) on 2025-04-17 06:25_

---

_Referenced in [astral-sh/ruff#17448](../../astral-sh/ruff/pulls/17448.md) on 2025-04-17 12:27_

---

_Closed by @carljm on 2025-04-18 00:28_

---
