```yaml
number: 14925
title: "[red-knot] `is_subtype_of`-transitivity violation"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2024-12-11T20:28:48Z
updated_at: 2024-12-14T10:02:50Z
url: https://github.com/astral-sh/ruff/issues/14925
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] `is_subtype_of`-transitivity violation

---

_Issue opened by @sharkdp on 2024-12-11 20:28_

Found by running the property tests on `main`.

We currently have `type[str] <: type` and `type <: ~None`, but not `type[str] <: ~None`.

---

_Label `red-knot` added by @sharkdp on 2024-12-11 20:28_

---

_Comment by @sharkdp on 2024-12-13 07:50_

The problem seems to be that we do not understand that `type[str]` is disjoint from `None`.

---

_Closed by @sharkdp on 2024-12-14 10:02_

---

_Closed by @sharkdp on 2024-12-14 10:02_

---
