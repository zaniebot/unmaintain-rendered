---
number: 13570
title: "red-knot thinks that the type of `1 / 2` is `Literal[0]`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2024-09-30T14:09:37Z
updated_at: 2024-09-30T20:50:38Z
url: https://github.com/astral-sh/ruff/issues/13570
synced_at: 2026-01-07T13:12:15-06:00
---

# red-knot thinks that the type of `1 / 2` is `Literal[0]`

---

_Issue opened by @AlexWaygood on 2024-09-30 14:09_

This is not accurate

---

_Label `red-knot` added by @AlexWaygood on 2024-09-30 14:09_

---

_Label `bug` added by @AlexWaygood on 2024-09-30 14:09_

---

_Label `help wanted` added by @AlexWaygood on 2024-09-30 14:10_

---

_Comment by @AlexWaygood on 2024-09-30 14:10_

the issue is this logic here: https://github.com/astral-sh/ruff/blob/5f4b2823277df38fc222e9d8096b805f9cc34c74/crates/red_knot_python_semantic/src/types/infer.rs#L2248-L2251

`<int_instance> / <int_instance>` in fact always produces an instance of `float`, never an `int`, so everything about this branch is actually incorrect. It should just always return `builtins_symbol_ty(self.db, "float").to_instance(self.db)`.

`<int_instance> // <int_instance>` produces `int`s, but that would be `ast::Operator::FloorDiv`, not `ast::Operator::Div`

---

_Assigned to @zanieb by @zanieb on 2024-09-30 18:42_

---

_Comment by @zanieb on 2024-09-30 18:42_

:)

---

_Referenced in [astral-sh/ruff#13575](../../astral-sh/ruff/pulls/13575.md) on 2024-09-30 18:53_

---

_Closed by @zanieb on 2024-09-30 20:50_

---
