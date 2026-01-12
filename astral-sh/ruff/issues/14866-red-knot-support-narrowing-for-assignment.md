```yaml
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
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Support narrowing for assignment expressions

---

_@sharkdp_

Narrowing should also be supported if the target is inside an assignment expression. For example:

```py
def f() -> int | None: ...

if (x := f()) is not None:
    reveal_type(x)  # should be `int`
```

---

_Label `red-knot` added by @sharkdp on 2024-12-09 11:36_

---

_Closed by @carljm on 2025-04-18 00:28_

---
