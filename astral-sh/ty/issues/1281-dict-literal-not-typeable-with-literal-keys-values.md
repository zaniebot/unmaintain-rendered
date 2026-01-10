```yaml
number: 1281
title: Dict literal not typeable with Literal keys/values
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-09-29T21:16:23Z
updated_at: 2025-09-30T15:48:11Z
url: https://github.com/astral-sh/ty/issues/1281
synced_at: 2026-01-10T02:06:25Z
```

# Dict literal not typeable with Literal keys/values

---

_Issue opened by @MeGaGiGaGon on 2025-09-29 21:16_

### Summary

This is probably covered by one of the other bi-directional type inference issues, but I could not find it exactly, so I figured I'd open a new issue.

https://play.ty.dev/b0eee25d-19ac-40cf-a5f5-6b3271ee61e7
```py
from typing import Literal

issue: dict[Literal["a"], Literal[1]] = {
    "a": 1
}  # Object of type `dict[Unknown | str, Unknown | int]` is not assignable to `dict[Literal["a"], Literal[1]]` (invalid-assignment) [Ln 3, Col 1]
```

This could probably be solved also if there was some mechanism where the literal was also considered to have the Literals of all it's key/values Literal variants, but I think this could also be solved with bi-directional type inference.

### Version

Playground (6b3c493cf)

---

_Label `bug` added by @sharkdp on 2025-09-30 06:50_

---

_Label `bidirectional inference` added by @sharkdp on 2025-09-30 06:50_

---

_Comment by @sharkdp on 2025-09-30 06:50_

Thank you for reporting this.

@ibraheemdev FYI

---

_Closed by @ibraheemdev on 2025-09-30 15:48_

---
