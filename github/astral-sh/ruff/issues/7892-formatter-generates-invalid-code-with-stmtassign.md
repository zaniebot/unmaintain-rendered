---
number: 7892
title: Formatter generates invalid code with StmtAssign, parentheses and comments
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-10T12:52:37Z
updated_at: 2023-10-19T09:24:13Z
url: https://github.com/astral-sh/ruff/issues/7892
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter generates invalid code with StmtAssign, parentheses and comments

---

_Issue opened by @konstin on 2023-10-10 12:52_

Input:
```python
x = (
    # a
    ( # b
        1
    )
)
```
Output:
```python
x = # a
(  # b
    1
)
```
Playground link: https://play.ruff.rs/70f3526e-34ac-46e3-a0cf-098a60537a91

---

_Label `bug` added by @konstin on 2023-10-10 12:52_

---

_Label `formatter` added by @konstin on 2023-10-10 12:52_

---

_Assigned to @konstin by @konstin on 2023-10-10 12:52_

---

_Referenced in [astral-sh/ruff#7873](../../astral-sh/ruff/pulls/7873.md) on 2023-10-10 14:31_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-10-16 07:44_

---

_Closed by @konstin on 2023-10-19 09:24_

---
