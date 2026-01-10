```yaml
number: 1463
title: Literal promotion should not affect negated types
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-10-31T13:56:37Z
updated_at: 2025-10-31T15:00:31Z
url: https://github.com/astral-sh/ty/issues/1463
synced_at: 2026-01-10T02:06:25Z
```

# Literal promotion should not affect negated types

---

_Issue opened by @sharkdp on 2025-10-31 13:56_

Consider the following example. I think what's happening is that we turn `str & ~Literal["a"]` into `str & ~str = Never` when performing literal promotion.
```py
class C[T]:
    def __init__(self, x: T):
        pass

def _(message: str):
    if message != "a":
        C(message)  # Expected `Never`, found `str & ~Literal["a"]`

        reveal_type([message])  # ty: list[Unknown], should be list[str & ~Literal["a"] | Unknown]
```
https://play.ty.dev/438a72e8-8269-4ec4-94e4-3c84754e339d

---

_Label `bug` added by @sharkdp on 2025-10-31 13:56_

---

_Label `generics` added by @sharkdp on 2025-10-31 13:56_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-31 13:56_

---

_Closed by @sharkdp on 2025-10-31 15:00_

---
