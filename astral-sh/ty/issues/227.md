```yaml
number: 227
title: "Do not treat nominal types inheriting from `Any` as fully static"
type: issue
state: open
author: sharkdp
labels:
  - type properties
assignees: []
created_at: 2024-12-04T16:15:41Z
updated_at: 2025-11-18T16:10:23Z
url: https://github.com/astral-sh/ty/issues/227
synced_at: 2026-01-10T01:58:59Z
```

# Do not treat nominal types inheriting from `Any` as fully static

---

_Issue opened by @sharkdp on 2024-12-04 16:15_

We currently don't understand that classes like `class C(Any): ...` represent non-fully-static types. This is captured in the following TODO comment:

https://github.com/astral-sh/ruff/blob/af43bd4b0fa5fdf016252085771cfb75846a0b0c/crates/red_knot_python_semantic/src/types.rs#L1031-L1044

See also the discussion [here](https://github.com/astral-sh/ruff/pull/14758#discussion_r1868436510) and a prior attempt at solving this (including a test) in this commit: https://github.com/astral-sh/ruff/commit/ec64bd840393aa908445720df9236b9cc11f0fb2

This is currently blocked on astral-sh/ty#230 



---

_Renamed from "[red-knot] Handle inheriting from `Any` in `Type::is_fully_static`" to "Handle inheriting from `Any` in `Type::is_fully_static`" by @MichaReiser on 2025-05-07 15:27_

---

_Label `subtyping/assignability` added by @AlexWaygood on 2025-05-10 21:39_

---

_Renamed from "Handle inheriting from `Any` in `Type::is_fully_static`" to "Do not treat nominal types inheriting from `Any` as fully static" by @AlexWaygood on 2025-10-05 14:50_

---

_Comment by @AlexWaygood on 2025-10-05 14:50_

Retitled the issue to reflect the current state (we no longer have a `Type::is_fully_static` method, but I suspect we still treat nominal types that inherit from `Any` as participating in subtyping etc., which we probably still need to fix)

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:00_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
