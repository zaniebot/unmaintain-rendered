---
number: 14861
title: "[red-knot] Narrowing for `while` loops"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-12-09T07:36:11Z
updated_at: 2024-12-13T06:40:15Z
url: https://github.com/astral-sh/ruff/issues/14861
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Narrowing for `while` loops

---

_Issue opened by @sharkdp on 2024-12-09 07:36_

We currently only narrow for `if`/`elif`, `match` and Boolean expressions. Narrowing for `while` loops should also be supported, for example:

```py
def f(n: int) -> int | None: ...

n = 0

while (x := f(n)) is not None:
    reveal_type(x)
    n += 1
```

---

_Label `red-knot` added by @sharkdp on 2024-12-09 07:36_

---

_Assigned to @sharkdp by @sharkdp on 2024-12-09 07:36_

---

_Referenced in [astral-sh/ruff#14759](../../astral-sh/ruff/pulls/14759.md) on 2024-12-09 08:06_

---

_Referenced in [astral-sh/ruff#14947](../../astral-sh/ruff/pulls/14947.md) on 2024-12-12 21:05_

---

_Closed by @sharkdp on 2024-12-13 06:40_

---

_Closed by @sharkdp on 2024-12-13 06:40_

---
