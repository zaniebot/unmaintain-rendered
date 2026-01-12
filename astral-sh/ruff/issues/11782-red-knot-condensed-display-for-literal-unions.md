```yaml
number: 11782
title: "[red-knot] condensed display for literal unions"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-06T20:34:11Z
updated_at: 2024-06-06T22:30:41Z
url: https://github.com/astral-sh/ruff/issues/11782
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] condensed display for literal unions

---

_@carljm_

We should display `Literal[1, 2]` rather than `(Literal[1] | Literal[2])`.

This should also work if there are other elements in the union, e.g. `(Literal[1, 2] | None)`

---

_Label `red-knot` added by @carljm on 2024-06-06 20:34_

---

_Comment by @AlexWaygood on 2024-06-06 21:33_

> This should also work if there are other elements in the union, e.g. `(Literal[1, 2] | None)`

(`None` itself is actually a valid argument to `Literal` according to PEP 586, so this example could theoretically be flattened further to `Literal[1, 2, None]`. But people very rarely do that, and we decided somewhere sometime at typeshed that we never wanted to use that. `Literal[1, 2] | None` seems clearer.)

---

_Closed by @carljm on 2024-06-06 22:30_

---
