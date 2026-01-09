---
number: 15034
title: "[red-knot] `sys.platform` support"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-12-17T12:21:48Z
updated_at: 2024-12-21T10:33:12Z
url: https://github.com/astral-sh/ruff/issues/15034
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] `sys.platform` support

---

_Issue opened by @sharkdp on 2024-12-17 12:21_

- New `python_platform` configuration setting for Red Knot
- Infer `Literal["linux"]` etc for `sys.platform` if config option is set accordingly
- Infer `str` if config option is not set or set to "all" (https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/sys/__init__.pyi#L49). Note: this could be `LiteralString`, but it should be changed in typeshed instead of patching it in Red Knot.

(this is already implemented, this is mostly a marker that no one else starts working on this)

---

_Label `red-knot` added by @sharkdp on 2024-12-17 12:21_

---

_Assigned to @sharkdp by @sharkdp on 2024-12-17 12:21_

---

_Referenced in [astral-sh/ruff#15019](../../astral-sh/ruff/pulls/15019.md) on 2024-12-17 12:22_

---

_Closed by @sharkdp on 2024-12-21 10:33_

---

_Closed by @sharkdp on 2024-12-21 10:33_

---
