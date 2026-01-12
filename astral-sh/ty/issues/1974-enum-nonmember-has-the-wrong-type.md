```yaml
number: 1974
title: enum.nonmember has the wrong type
type: issue
state: closed
author: refi64
labels:
  - bug
  - enums
assignees: []
created_at: 2025-12-17T02:29:56Z
updated_at: 2025-12-19T00:59:50Z
url: https://github.com/astral-sh/ty/issues/1974
synced_at: 2026-01-12T15:54:26Z
```

# enum.nonmember has the wrong type

---

_@refi64_

### Summary

https://play.ty.dev/2314690d-c925-4cc1-a5cc-418c928628ca

```python
from typing import reveal_type
import enum

class E(enum.Enum):
    A = enum.auto()
    X = enum.nonmember(123)

reveal_type(E.X)  # Revealed type: `Unknown | nonmember[int]`
```

This really should just be `int`.

### Version

ty 0.0.2

---

_Label `bug` added by @AlexWaygood on 2025-12-17 07:03_

---

_Label `enums` added by @AlexWaygood on 2025-12-17 07:03_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 07:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-17 14:09_

---

_Closed by @charliermarsh on 2025-12-19 00:59_

---
