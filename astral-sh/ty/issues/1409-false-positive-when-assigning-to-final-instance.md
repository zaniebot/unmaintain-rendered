```yaml
number: 1409
title: "False positive when assigning to `Final` instance attribute in `__init__`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-10-22T15:25:21Z
updated_at: 2025-11-11T20:54:06Z
url: https://github.com/astral-sh/ty/issues/1409
synced_at: 2026-01-12T15:54:25Z
```

# False positive when assigning to `Final` instance attribute in `__init__`

---

_@sharkdp_

From the conformance suite (and 44 instances across the ecosystem once type-of-self lands):

```py
from typing import Final, Self

class ClassA:
    ID4: Final[int]  # OK because initialized in __init__

    def __init__(self: Self):
        self.ID4 = 1  #     Cannot assign to final attribute `ID4` on type `Self@__init__` (invalid-assignment)
```
https://play.ty.dev/38bf9854-4d2e-495c-b96b-29553d212819


---

_Label `bug` added by @sharkdp on 2025-10-22 15:25_

---

_Label `type properties` added by @sharkdp on 2025-10-22 15:25_

---

_Added to milestone `GA` by @carljm on 2025-10-30 16:12_

---

_Comment by @saada on 2025-10-31 02:52_

I've submitted a PR to fix this issue: https://github.com/astral-sh/ruff/pull/21158

The fix allows Final instance attributes to be initialized in `__init__` methods as per PEP 591.

---

_Closed by @carljm on 2025-11-11 20:54_

---
