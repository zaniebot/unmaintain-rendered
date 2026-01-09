---
number: 16614
title: "[red-knot] support classes defining __getattr__"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-03-10T23:28:30Z
updated_at: 2025-03-12T12:44:12Z
url: https://github.com/astral-sh/ruff/issues/16614
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] support classes defining __getattr__

---

_Issue opened by @carljm on 2025-03-10 23:28_

(We can also support `__getattribute__`, `__setattr__`, `__delattr__` if it's easy, but these are much lower priority so not considering them part of this issue.)

A class defining `__getattr__` should allow access of any otherwise-unknown instance attribute, considering it of the type given in the return type annotation on the `__getattr__` method.

---

_Label `red-knot` added by @carljm on 2025-03-10 23:28_

---

_Referenced in [astral-sh/ruff#15697](../../astral-sh/ruff/issues/15697.md) on 2025-03-10 23:30_

---

_Assigned to @sharkdp by @sharkdp on 2025-03-12 11:03_

---

_Referenced in [astral-sh/ruff#16668](../../astral-sh/ruff/pulls/16668.md) on 2025-03-12 12:08_

---

_Closed by @sharkdp on 2025-03-12 12:44_

---

_Closed by @sharkdp on 2025-03-12 12:44_

---
