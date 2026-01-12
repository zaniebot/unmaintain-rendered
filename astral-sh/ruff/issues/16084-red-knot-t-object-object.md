```yaml
number: 16084
title: "[red-knot] `T | object = object`"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2025-02-10T19:30:52Z
updated_at: 2025-02-10T22:07:07Z
url: https://github.com/astral-sh/ruff/issues/16084
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] `T | object = object`

---

_@sharkdp_

### Description

We can generally simplify unions involving `object` to `object`. We already do so for fully-static types, but this is also more generally true for all gradual types. For example, `Any | object` is equivalent to `object`.

---

_Label `red-knot` added by @sharkdp on 2025-02-10 19:30_

---

_Assigned to @sharkdp by @sharkdp on 2025-02-10 19:31_

---

_Closed by @sharkdp on 2025-02-10 22:07_

---
