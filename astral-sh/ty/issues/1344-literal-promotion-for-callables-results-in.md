```yaml
number: 1344
title: Literal promotion for callables results in missing attributes
type: issue
state: closed
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2025-10-13T12:41:15Z
updated_at: 2025-10-13T13:21:56Z
url: https://github.com/astral-sh/ty/issues/1344
synced_at: 2026-01-10T02:06:25Z
```

# Literal promotion for callables results in missing attributes

---

_Issue opened by @sharkdp on 2025-10-13 12:41_

Consider:
```py
def f(): ...
def g(): ...

for func in [f, g]:
    # Attribute `__name__` on type `Unknown | (() -> Unknown)` may be missing
    print(func.__name__)
```
https://play.ty.dev/f7296c98-e1f9-4fc1-a4af-338f40f485cf

---

_Label `bug` added by @sharkdp on 2025-10-13 12:41_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-13 12:41_

---

_Closed by @sharkdp on 2025-10-13 13:21_

---
