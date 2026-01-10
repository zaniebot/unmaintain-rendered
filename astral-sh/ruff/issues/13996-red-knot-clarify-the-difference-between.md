---
number: 13996
title: "[red-knot] Clarify the difference between `CallOutcome::return_ty` and `::return_ty_result`"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2024-10-30T12:38:38Z
updated_at: 2025-01-15T20:42:02Z
url: https://github.com/astral-sh/ruff/issues/13996
synced_at: 2026-01-10T01:22:54Z
---

# [red-knot] Clarify the difference between `CallOutcome::return_ty` and `::return_ty_result`

---

_Issue opened by @MichaReiser on 2024-10-30 12:38_

The documentation and the names of `CallOutcome::return_ty` and `CallOutcome::return_ty_result` both give the impression that they only differ in their return type, but that's not the case:

* `return_ty_result` emits reveal type diagnostics where `return_ty` does not
* I do think both behave the same in regards of how they handle unions but isn't entirely clear

It wasn't clear to me when I'm supposed to use witch method. 

We should improve the documentation or consider unifying the two if all that's different is the return type (you can get an `Option` from a `Result` using `.ok`)

---

_Label `red-knot` added by @MichaReiser on 2024-10-30 12:38_

---

_Referenced in [astral-sh/ruff#14012](../../astral-sh/ruff/issues/14012.md) on 2024-10-31 12:52_

---

_Referenced in [astral-sh/ruff#14137](../../astral-sh/ruff/pulls/14137.md) on 2024-11-07 09:13_

---

_Comment by @AlexWaygood on 2025-01-15 20:42_

Closing in favour of #15377

---

_Closed by @AlexWaygood on 2025-01-15 20:42_

---
