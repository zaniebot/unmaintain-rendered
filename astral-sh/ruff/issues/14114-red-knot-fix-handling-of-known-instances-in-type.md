```yaml
number: 14114
title: "[red-knot] fix handling of known instances in `Type` relation methods"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-05T17:50:26Z
updated_at: 2024-11-07T22:07:28Z
url: https://github.com/astral-sh/ruff/issues/14114
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] fix handling of known instances in `Type` relation methods

---

_Issue opened by @carljm on 2024-11-05 17:50_

We currently represent "known instances" (e.g. special forms like `typing.Literal`, which are an instance of `typing._SpecialForm`, but need to be handled differently from other instances of `typing._SpecialForm`) as an `InstanceType` with a `known` field that is `Some(...)`.

This makes it easy to handle a known instance as if it were a regular instance type (by ignoring the `known` field), and in some cases (e.g. `Type::member`) that is correct and convenient. But in other cases (e.g. `Type::is_equivalent_to`) it is not correct, and we currently have a bug that we would consider the known-instance type of `typing.Literal` as equivalent to the general instance type for `typing._SpecialForm`, and we would fail to consider it a singleton type or a single-valued type (even though it is both.)

An instance type with `known.is_some()` is semantically quite different from an instance type with `known.is_none()`. The former is a singleton type that represents exactly one runtime object; the latter is an open type that represents many runtime objects, including instances of unknown subclasses. It is too error-prone to represent these very-different types as a single `Type` variant. We should instead introduce a dedicated `Type::KnownInstance` variant and force ourselves to handle these explicitly in all `Type` variant matches.

---

_Label `red-knot` added by @carljm on 2024-11-05 17:50_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-11-07 14:01_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:23_

---

_Closed by @carljm on 2024-11-07 22:07_

---

_Closed by @carljm on 2024-11-07 22:07_

---
