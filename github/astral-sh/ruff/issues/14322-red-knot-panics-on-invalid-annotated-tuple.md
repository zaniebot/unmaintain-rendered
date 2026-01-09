---
number: 14322
title: "[red-knot] Panics on invalid annotated tuple assignment"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-13T16:07:47Z
updated_at: 2024-11-13T19:50:41Z
url: https://github.com/astral-sh/ruff/issues/14322
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Panics on invalid annotated tuple assignment

---

_Issue opened by @sharkdp on 2024-11-13 16:07_

Red knot currently panics if someone tries to do this:
```py
(x, y): tuple[int, int] = (2, 3)
```
The same panic appears in this seemingly different example … 
```py
def f^(x, y):
    pass
```
where an invalid character triggers error recovery in the parser, and the remaining `(x, y):` part appears to be an annotated assignment to a tuple.


The assertion is caused by the fact that we call `SemanticIndexBuilder::add_definition` twice, once for each tuple element.

---

_Label `bug` added by @sharkdp on 2024-11-13 16:07_

---

_Label `red-knot` added by @sharkdp on 2024-11-13 16:07_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-13 16:07_

---

_Referenced in [astral-sh/ruff#14325](../../astral-sh/ruff/pulls/14325.md) on 2024-11-13 18:50_

---

_Closed by @sharkdp on 2024-11-13 19:50_

---

_Closed by @sharkdp on 2024-11-13 19:50_

---
