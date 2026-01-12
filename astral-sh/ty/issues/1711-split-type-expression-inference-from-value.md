```yaml
number: 1711
title: split type-expression inference from value-expression inference
type: issue
state: open
author: carljm
labels:
  - typing semantics
  - type aliases
assignees: []
created_at: 2025-12-01T21:39:42Z
updated_at: 2025-12-19T12:11:57Z
url: https://github.com/astral-sh/ty/issues/1711
synced_at: 2026-01-12T15:54:25Z
```

# split type-expression inference from value-expression inference

---

_@carljm_

Many of our problems with understanding implicit type aliases (particularly generic ones) are ultimately based in our conflation of value types and type-expression types for names. Many of these problems would be solved by separating these. This would mean eliminating `Type::in_type_expression`. When we see a name used in a type expression, rather than resolving its type as a value expression and trying to re-interpret that type as a type expression, we could instead go through a separate resolution path for that name to resolve "what it means as a type expression." This means that the RHS of an implicit type alias would be inferred twice, once as a value expression (in order to catch any runtime-invalid operations performed in it) and separately also (assuming it is ever used in a type expression) as a type expression.

---

_Added to milestone `Stable` by @carljm on 2025-12-01 21:39_

---

_Label `typing semantics` added by @carljm on 2025-12-01 21:39_

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 12:11_

---
