```yaml
number: 1017
title: error if a legacy typevar with explicit variance is used in a way that contradicts its explicit variance
type: issue
state: open
author: carljm
labels:
  - generics
  - typing semantics
assignees: []
created_at: 2025-08-15T22:19:30Z
updated_at: 2025-10-06T16:39:04Z
url: https://github.com/astral-sh/ty/issues/1017
synced_at: 2026-01-12T15:54:24Z
```

# error if a legacy typevar with explicit variance is used in a way that contradicts its explicit variance

---

_@carljm_

Example:

```py
from typing import TypeVar

T = TypeVar("T", covariant=True)

class C[T]:
    def method(self, x: T):  # this usage requires C to be at least contravariant in T
        pass

---

_Added to milestone `GA` by @carljm on 2025-08-15 22:19_

---

_Label `typing semantics` added by @AlexWaygood on 2025-10-06 16:39_

---

_Label `generics` added by @AlexWaygood on 2025-10-06 16:39_

---
