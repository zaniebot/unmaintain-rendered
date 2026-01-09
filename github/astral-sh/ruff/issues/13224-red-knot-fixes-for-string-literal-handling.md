---
number: 13224
title: "[red-knot] Fixes for string literal handling"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-09-03T07:29:20Z
updated_at: 2024-09-08T03:25:11Z
url: https://github.com/astral-sh/ruff/issues/13224
synced_at: 2026-01-07T13:12:15-06:00
---

# [red-knot] Fixes for string literal handling

---

_Issue opened by @MichaReiser on 2024-09-03 07:29_

* Chane the type of `StringLiteralType::value` to `Option<Box>` so that we can distinguish between empty strings in the source code and strings that were truncated
* Avoid building strings that are larger than `MAX_STRING_LITERAL_SIZE` when concatenating strings with `+` in `infer_binary_expression`


See comments in https://github.com/astral-sh/ruff/pull/13117

---

_Label `help wanted` added by @MichaReiser on 2024-09-03 07:29_

---

_Label `red-knot` added by @MichaReiser on 2024-09-03 07:29_

---

_Comment by @AlexWaygood on 2024-09-03 07:33_

> * Chane the type of `StringLiteralType::value` to `Option<Box>` so that we can distinguish between empty strings in the source code and strings that were truncated

I think we should just use `Type::LiteralString` (which means "a string literal, but not one we know the value of") for the latter rather than changing the type of `StringLiteralType::value`. We shouldn't use `StringLiteralType` unless we know the value of the string literal.

---

_Referenced in [astral-sh/ruff#13117](../../astral-sh/ruff/pulls/13117.md) on 2024-09-03 07:34_

---

_Renamed from "Follow ups for #13117" to "[red-knot] Fixes for string literal handling" by @carljm on 2024-09-03 21:38_

---

_Referenced in [astral-sh/ruff#13276](../../astral-sh/ruff/pulls/13276.md) on 2024-09-07 06:16_

---

_Closed by @carljm on 2024-09-08 03:25_

---

_Closed by @carljm on 2024-09-08 03:25_

---
