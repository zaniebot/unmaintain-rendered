---
number: 14648
title: "[red-knot] Narrowing on `s == \"foo\"` where `s` is of type `LiteralString`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - ty
assignees: []
created_at: 2024-11-28T00:10:02Z
updated_at: 2024-12-03T03:31:59Z
url: https://github.com/astral-sh/ruff/issues/14648
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Narrowing on `s == "foo"` where `s` is of type `LiteralString`

---

_Issue opened by @InSyncWithFoo on 2024-11-28 00:10_

For any symbol `s` of type `LiteralString`, it should be safe to narrow it according to comparisons of the pattern `s == "..."`.

```python
a: LiteralString

if a == "foo":
	reveal_type(a)  # revealed: Literal["foo"]
```

This issue is part of #13694.

---

_Referenced in [astral-sh/ruff#14649](../../astral-sh/ruff/pulls/14649.md) on 2024-11-28 00:11_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-28 00:49_

---

_Closed by @carljm on 2024-12-03 03:31_

---
