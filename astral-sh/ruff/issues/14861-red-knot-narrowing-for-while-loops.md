```yaml
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
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Narrowing for `while` loops

---

_@sharkdp_

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

_Closed by @sharkdp on 2024-12-13 06:40_

---

_Closed by @sharkdp on 2024-12-13 06:40_

---
