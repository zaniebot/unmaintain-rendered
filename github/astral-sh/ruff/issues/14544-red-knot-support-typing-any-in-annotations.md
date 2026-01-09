---
number: 14544
title: "[red-knot] support typing.Any in annotations"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-22T21:41:34Z
updated_at: 2024-12-04T14:56:37Z
url: https://github.com/astral-sh/ruff/issues/14544
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] support typing.Any in annotations

---

_Issue opened by @carljm on 2024-11-22 21:41_

We have an internal representation (`Type::Any`) for the dynamic type `Any`, but we don't yet understand  the `typing.Any` symbol as a spelling for that type in a type expression.

To add this, we'll need to track `typing.Any` as a `KnownInstance` (similar to the existing support for `typing.Union`, `typing.Optional`, `typing.Literal`), and add support for it in `Type::in_type_expression`, so that when `TypeInferenceBuilder::infer_type_expression` comes across the symbol in an annotation, we understand that annotation as describing the `Any` type.

We'll want to add mdtests for the "happy path" case like this:

```py
x: Any = 1
x = "foo"

def f():
    reveal_type(x)  # revealed: Any
```

We'll also want edge-case tests showing that `typing.Any` can be aliased to a different name, and still works (that is, we are recognizing `typing.Any` under any name, not just pattern-matching on the local name), and similarly that a locally-defined class named `Any` simply means its own instance type, not `Type::Any`, in an annotation.

---

_Label `red-knot` added by @carljm on 2024-11-22 21:41_

---

_Assigned to @carljm by @carljm on 2024-11-22 21:41_

---

_Renamed from "[red-knot] " to "[red-knot] support typing.Any in annotations" by @carljm on 2024-11-22 21:41_

---

_Referenced in [astral-sh/ruff#14172](../../astral-sh/ruff/issues/14172.md) on 2024-11-22 21:42_

---

_Referenced in [astral-sh/ruff#14546](../../astral-sh/ruff/issues/14546.md) on 2024-11-22 22:31_

---

_Assigned to @dcreager by @dcreager on 2024-12-02 17:21_

---

_Unassigned @carljm by @dcreager on 2024-12-02 17:21_

---

_Referenced in [astral-sh/ruff#14742](../../astral-sh/ruff/pulls/14742.md) on 2024-12-02 20:10_

---

_Closed by @dcreager on 2024-12-04 14:56_

---

_Closed by @dcreager on 2024-12-04 14:56_

---
