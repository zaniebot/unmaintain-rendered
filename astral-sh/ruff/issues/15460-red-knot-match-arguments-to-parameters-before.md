---
number: 15460
title: "[red-knot] match arguments to parameters before inferring argument types"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-13T18:33:35Z
updated_at: 2025-03-21T13:38:13Z
url: https://github.com/astral-sh/ruff/issues/15460
synced_at: 2026-01-10T01:22:56Z
---

# [red-knot] match arguments to parameters before inferring argument types

---

_Issue opened by @carljm on 2025-01-13 18:33_

In the medium term, we will need contextual type inference of argument types. (This will be more relevant once we have generics.) Some specific examples are at https://discuss.python.org/t/pep-747-typeexpr-type-hint-for-a-type-expression/55984/67. PEP 747 will also add a new case of this. This means we will have to know which parameter an argument maps to, before we infer the type of the argument expression.

In the short term, this will allow us to fix a bug with how we handle `KnownFunction::parameter_expectations`. It's _parameter_ expectations, but we apply it naively to _argument_ indices, giving the wrong answer when arguments are out-of-order with keywords. This bug currently shows up when `typing.cast` is called with out-of-order keyword arguments.

---

_Label `red-knot` added by @carljm on 2025-01-13 18:33_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-13 18:33_

---

_Assigned to @carljm by @carljm on 2025-02-07 15:43_

---

_Assigned to @dcreager by @carljm on 2025-03-10 23:21_

---

_Unassigned @carljm by @carljm on 2025-03-10 23:21_

---

_Referenced in [astral-sh/ruff#16568](../../astral-sh/ruff/pulls/16568.md) on 2025-03-11 18:07_

---

_Referenced in [astral-sh/ruff#16546](../../astral-sh/ruff/pulls/16546.md) on 2025-03-18 20:56_

---

_Closed by @dcreager on 2025-03-21 13:38_

---

_Closed by @dcreager on 2025-03-21 13:38_

---
