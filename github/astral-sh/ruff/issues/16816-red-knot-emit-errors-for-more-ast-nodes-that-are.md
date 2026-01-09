---
number: 16816
title: "[red-knot] Emit errors for more AST nodes that are invalid (or only valid in specific contexts) in type expressions"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-03-17T16:31:46Z
updated_at: 2025-03-18T17:16:52Z
url: https://github.com/astral-sh/ruff/issues/16816
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Emit errors for more AST nodes that are invalid (or only valid in specific contexts) in type expressions

---

_Issue opened by @AlexWaygood on 2025-03-17 16:31_

Some AST nodes are always invalid in type expressions; we should emit an `invalid-type-form` diagnostic in red-knot if we see one of these in a type expression. This applies to all of these branches:

https://github.com/astral-sh/ruff/blob/7ca5f132ca274a45ea3776b8732ab8dadf91f349/crates/red_knot_python_semantic/src/types/infer.rs#L6150-L6233

Similarly, an ellipsis literal is only valid in very specific contexts in type expressions: as part of a `Callable` type expression (e.g. `Callable[..., int]` or as part of a `tuple`-subscript type expression (e.g. `tuple[int, ...]`. We should never call `infer_type_expression_no_store` with an ellipsis literal node if we're visiting a sub-expression in either of those type expressions. So we should also be emitting a diagnostic if we hit this branch:

https://github.com/astral-sh/ruff/blob/7ca5f132ca274a45ea3776b8732ab8dadf91f349/crates/red_knot_python_semantic/src/types/infer.rs#L6070-L6074

This would be a good contributor task. The changes required will be very similar to the ones done in https://github.com/astral-sh/ruff/pull/16765. @MatthewMckee4, you might be interested in taking this one on, possibly? :-)



---

_Label `help wanted` added by @AlexWaygood on 2025-03-17 16:31_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-17 16:31_

---

_Comment by @MatthewMckee4 on 2025-03-17 19:19_

> Some AST nodes are always invalid in type expressions; we should emit an `invalid-type-form` diagnostic in red-knot if we see one of these in a type expression. This applies to all of these branches:
> 
> https://github.com/astral-sh/ruff/blob/7ca5f132ca274a45ea3776b8732ab8dadf91f349/crates/red_knot_python_semantic/src/types/infer.rs#L6150-L6233

So currently we don't emit any error message right? So we want to do similar to what we did for the literal types. Also, right now, why is the expression still evaluated (or inferred)?

Edit: sorry I didn't see that you referenced my PR

---

_Comment by @MatthewMckee4 on 2025-03-17 19:53_

I think I can see why we still infer the type, so nevermind. I'm happy to take this on

---

_Referenced in [astral-sh/ruff#16822](../../astral-sh/ruff/pulls/16822.md) on 2025-03-17 21:33_

---

_Assigned to @MatthewMckee4 by @carljm on 2025-03-18 15:54_

---

_Closed by @AlexWaygood on 2025-03-18 17:16_

---

_Closed by @AlexWaygood on 2025-03-18 17:16_

---
