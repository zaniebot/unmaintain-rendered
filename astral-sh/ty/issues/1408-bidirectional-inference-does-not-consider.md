```yaml
number: 1408
title: Bidirectional inference does not consider parameter types of constructors
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-10-22T09:49:17Z
updated_at: 2025-10-24T20:14:19Z
url: https://github.com/astral-sh/ty/issues/1408
synced_at: 2026-01-12T15:54:25Z
```

# Bidirectional inference does not consider parameter types of constructors

---

_@sharkdp_

Consider:
```py
from typing import Literal

def f(default: dict[Literal["value"], str]):
    pass

# this works fine
f({"value": "hello"})

class C:
    def __init__(self, default: dict[Literal["value"], str]):
        pass

# this throws an error
C({"value": "hello"})
```
https://play.ty.dev/c4307737-6697-4b77-9192-1e1e6e14e70a

---

_Label `bug` added by @sharkdp on 2025-10-22 09:49_

---

_Label `bidirectional inference` added by @sharkdp on 2025-10-22 09:49_

---

_Closed by @ibraheemdev on 2025-10-24 20:14_

---
