```yaml
number: 15079
title: "[red-knot] Interned visibility constraints"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/interned-visibility-constraints
created_at: 2024-12-20T10:37:35Z
updated_at: 2025-02-10T12:32:33Z
url: https://github.com/astral-sh/ruff/pull/15079
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Interned visibility constraints

---

_@sharkdp_

## Summary

Unfinished experiment to intern `VisibilityConstraint` using salsa. Still >150 lifetime errors to go.



---

_Label `red-knot` added by @sharkdp on 2024-12-20 10:37_

---

_Renamed from "Interned visibility constraints" to "[red-knot] Interned visibility constraints" by @sharkdp on 2024-12-20 10:39_

---

_Closed by @sharkdp on 2025-02-10 09:29_

---

_Comment by @MichaReiser on 2025-02-10 10:20_

Uh, that sounds painful. What was the motivation for this and what was your finding that you decided against it?

---

_Comment by @sharkdp on 2025-02-10 12:32_

> What was the motivation for this

Runtime improvements. It was originally planned as a "cheap" alternative solution to implementing TDDs, so I think it's now superseded by @dcreager's recent advances in this direction. We eagerly simplify these visibility constraints now, so any salsa-based memoization would probably result in a much smaller potential benefit, if any?

---
