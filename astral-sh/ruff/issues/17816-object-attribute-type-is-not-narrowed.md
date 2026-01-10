---
number: 17816
title: Object attribute type is not narrowed
type: issue
state: closed
author: charliermarsh
labels:
  - ty
assignees: []
created_at: 2025-05-03T14:30:39Z
updated_at: 2025-05-03T14:36:58Z
url: https://github.com/astral-sh/ruff/issues/17816
synced_at: 2026-01-10T01:22:59Z
---

# Object attribute type is not narrowed

---

_Issue opened by @charliermarsh on 2025-05-03 14:30_

E.g., here, `request.limit + 1` is marked as: Operator `+` is unsupported between objects of type `Unknown | int | None` and `Literal[1]`

```python
class Request:
    
    def __init__(
        self,
        limit: int | None = None,
    ):
        self.limit = limit


request = Request(limit=None)

if request.limit is not None:
    print(request.limit + 1)
```

See: https://types.ruff.rs/82735c4f-5e2a-45bb-a488-1c1b45326227

---

_Label `red-knot` added by @charliermarsh on 2025-05-03 14:30_

---

_Closed by @AlexWaygood on 2025-05-03 14:36_

---
