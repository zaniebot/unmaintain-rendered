```yaml
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
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] support `typing.NoReturn` and `typing.Never` in annotations

---

_@Glyphack_

(part of #14172 created to claim the task)

[NoReturn](https://typing.readthedocs.io/en/latest/spec/special-types.html#noreturn) and [Never](https://typing.readthedocs.io/en/latest/spec/special-types.html#never) are synonyms. They can both be mapped to `Type::Never`.
We can have a boolean flag on the `Type::Never` to indicate if this was a `Never` or `NoReturn` Although this is only useful for showing the type.


## Assignability

https://github.com/python/typing/blob/main/conformance/tests/specialtypes_never.py#L64
- Never is assignable to other types
- Only Never and Any are assignable to Never


---

_Label `red-knot` added by @AlexWaygood on 2024-11-23 17:11_

---

_Closed by @carljm on 2024-11-25 21:37_

---

_Closed by @carljm on 2024-11-25 21:37_

---
