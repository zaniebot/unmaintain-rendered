---
number: 13891
title: pyupgrade rule for Unpack
type: issue
state: closed
author: fzimmermann89
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-10-23T12:48:05Z
updated_at: 2024-10-31T06:58:36Z
url: https://github.com/astral-sh/ruff/issues/13891
synced_at: 2026-01-07T13:12:16-06:00
---

# pyupgrade rule for Unpack

---

_Issue opened by @fzimmermann89 on 2024-10-23 12:48_

It seems ruff is missing https://github.com/asottile/pyupgrade/pull/745 `G[Unpack[T]] => G[*T] ` to change UNPACK to asterisk if supported by the python version:

```
from typing import Unpack
def f(*x: Unpack[tuple[int, ...]]):
    return x

```
should be rewritten as
```
def f(*x: *tuple[int, ...]):
    return x
```



---

_Label `rule` added by @MichaReiser on 2024-10-23 13:04_

---

_Label `accepted` added by @AlexWaygood on 2024-10-23 13:33_

---

_Label `help wanted` added by @AlexWaygood on 2024-10-23 17:44_

---

_Referenced in [astral-sh/ruff#13988](../../astral-sh/ruff/pulls/13988.md) on 2024-10-30 03:50_

---

_Closed by @MichaReiser on 2024-10-31 06:58_

---
