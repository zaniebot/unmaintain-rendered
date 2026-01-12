```yaml
number: 13853
title: "[red-knot] Infer slice expression types"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-10-21T10:14:21Z
updated_at: 2024-10-29T09:17:46Z
url: https://github.com/astral-sh/ruff/issues/13853
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] Infer slice expression types

---

_@sharkdp_

Part of astral-sh/ty#244, and also astral-sh/ruff#13689. Infer types for [slice expressions](https://docs.python.org/3/reference/expressions.html#slicings).

- [x] x[1:3] should be inferred as tuple[str, str] if x has type tuple[int, str, str, int]
- [x] y[1:3] should be inferred as Literal["aa"] if y has type Literal["baab"].

---

_Renamed from "slice expression" to "[red-knot] Infer slice expression types" by @sharkdp on 2024-10-21 10:14_

---

_Label `red-knot` added by @sharkdp on 2024-10-21 10:14_

---

_Assigned to @sharkdp by @sharkdp on 2024-10-21 10:19_

---

_Closed by @sharkdp on 2024-10-29 09:17_

---
