---
number: 14558
title: "[red-knot] support `typing.NoReturn` and `typing.Never` in annotations"
type: issue
state: closed
author: Glyphack
labels:
  - ty
assignees: []
created_at: 2024-11-23T17:01:19Z
updated_at: 2024-11-25T21:37:57Z
url: https://github.com/astral-sh/ruff/issues/14558
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] support `typing.NoReturn` and `typing.Never` in annotations

---

_Issue opened by @Glyphack on 2024-11-23 17:01_

(part of #14172 created to claim the task)

[NoReturn](https://typing.readthedocs.io/en/latest/spec/special-types.html#noreturn) and [Never](https://typing.readthedocs.io/en/latest/spec/special-types.html#never) are synonyms. They can both be mapped to `Type::Never`.
We can have a boolean flag on the `Type::Never` to indicate if this was a `Never` or `NoReturn` Although this is only useful for showing the type.


## Assignability

https://github.com/python/typing/blob/main/conformance/tests/specialtypes_never.py#L64
- Never is assignable to other types
- Only Never and Any are assignable to Never


---

_Referenced in [astral-sh/ruff#14559](../../astral-sh/ruff/pulls/14559.md) on 2024-11-23 17:07_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-23 17:11_

---

_Referenced in [astral-sh/ruff#14172](../../astral-sh/ruff/issues/14172.md) on 2024-11-23 19:34_

---

_Closed by @carljm on 2024-11-25 21:37_

---

_Closed by @carljm on 2024-11-25 21:37_

---
