---
number: 11331
title: "[preview][E302] False positive in overload when comment added"
type: issue
state: closed
author: randolf-scholz
labels:
  - bug
  - preview
assignees: []
created_at: 2024-05-08T07:19:07Z
updated_at: 2024-05-08T15:19:28Z
url: https://github.com/astral-sh/ruff/issues/11331
synced_at: 2026-01-07T13:12:15-06:00
---

# [preview][E302] False positive in overload when comment added

---

_Issue opened by @randolf-scholz on 2024-05-08 07:19_

```python
@overload
def arrow_strip_whitespace(obj: Table, /, *cols: str) -> Table: ...
@overload
def arrow_strip_whitespace(obj: Array, /, *cols: str) -> Array: ...  # type: ignore[misc]
def arrow_strip_whitespace(obj, /, *cols):
    ...
```

This raises E302 on the line `def arrow_strip_whitespace(obj, /, *cols)`. If I remove the type-ignore comment, it does not flag it anymore.

https://play.ruff.rs/cfc09a7a-1e90-4254-8e16-827aebd143c9

---

_Label `bug` added by @MichaReiser on 2024-05-08 08:49_

---

_Label `preview` added by @MichaReiser on 2024-05-08 08:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-08 15:12_

---

_Referenced in [astral-sh/ruff#11342](../../astral-sh/ruff/pulls/11342.md) on 2024-05-08 15:13_

---

_Closed by @charliermarsh on 2024-05-08 15:19_

---
