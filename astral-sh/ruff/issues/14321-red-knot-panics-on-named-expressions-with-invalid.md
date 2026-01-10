```yaml
number: 14321
title: "[red-knot] Panics on named expressions with invalid targets"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-13T15:18:00Z
updated_at: 2024-11-13T19:50:41Z
url: https://github.com/astral-sh/ruff/issues/14321
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Panics on named expressions with invalid targets

---

_Issue opened by @sharkdp on 2024-11-13 15:18_

Red knot currently panics on three out of four of these examples from `crates/ruff_python_parser/resources/invalid/expressions/named/invalid_target.py`, at two different places:
```py
(x.y := 1)  # panicked at crates/red_knot_python_semantic/src/types/infer.rs:2521:37:
(x[y] := 1)  # panicked at crates/red_knot_python_semantic/src/types/infer.rs:2521:37:
(*x := 1)
([x, y] := [1, 2])  # panicked at crates/red_knot_python_semantic/src/semantic_index/builder.rs:224:9:
```
We currently don't handle `ExprNamed::target`s that are not an `ExprName` properly.

---

_Label `bug` added by @sharkdp on 2024-11-13 15:18_

---

_Label `red-knot` added by @sharkdp on 2024-11-13 15:18_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-13 15:18_

---

_Closed by @sharkdp on 2024-11-13 19:50_

---

_Closed by @sharkdp on 2024-11-13 19:50_

---
