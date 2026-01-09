---
number: 13863
title: "[red-knot] rename `Type::Class` and `Type::Function` to `Type::ClassLiteral` and `Type::FunctionLiteral`"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-10-21T18:02:48Z
updated_at: 2024-10-22T20:10:54Z
url: https://github.com/astral-sh/ruff/issues/13863
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] rename `Type::Class` and `Type::Function` to `Type::ClassLiteral` and `Type::FunctionLiteral`

---

_Issue opened by @carljm on 2024-10-21 18:02_

We've discussed and agreed this rename would be an improvement. It clarifies better what these types represent, and avoids the likely confusion of assuming `Type::Function` is something like `typing.Callable` and `Type::Class` is the representation of a type expression like `str`.

It also avoids the current false parallel between `Type::Class` and `Type::Instance`, where `Type::Class` is a singleton type representing exactly one class definition, but `Type::Instance` includes instances of subclasses.


---

_Label `help wanted` added by @carljm on 2024-10-21 18:02_

---

_Label `red-knot` added by @carljm on 2024-10-21 18:02_

---

_Referenced in [astral-sh/ruff#13873](../../astral-sh/ruff/pulls/13873.md) on 2024-10-22 06:53_

---

_Closed by @sharkdp on 2024-10-22 20:10_

---
