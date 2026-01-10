```yaml
number: 338
title: Intersections of multiple synthesized protocols should be merged together into a single synthesized protocol
type: issue
state: open
author: AlexWaygood
labels:
  - type properties
  - set-theoretic types
  - Protocols
assignees: []
created_at: 2025-05-12T23:31:32Z
updated_at: 2025-11-18T16:10:26Z
url: https://github.com/astral-sh/ty/issues/338
synced_at: 2026-01-10T01:58:59Z
```

# Intersections of multiple synthesized protocols should be merged together into a single synthesized protocol

---

_Issue opened by @AlexWaygood on 2025-05-12 23:31_

Currently we reveal the following here:

```py
from typing import reveal_type

def f(x: object):
    if hasattr(x, "foo") and hasattr(x, "bar"):
        reveal_type(x)  # revealed: <Protocol with members 'foo'> & <Protocol with members 'bar'>
```

https://play.ty.dev/6eba5550-5bb9-4481-a3e6-7a616a09c3a3

What would be better would be if we merged the two synthesized protocols together into a single synthesized protocol:

```py
from typing import reveal_type

def f(x: object):
    if hasattr(x, "foo") and hasattr(x, "bar"):
        reveal_type(x)  # revealed: <Protocol with members 'foo', 'bar'>
```

This could be implemented as part of the logic in our `IntersectionBuilder`. We would only want to implement the change for synthesized protocols, not for class-based protocols.

The reasons for merging them together are:
- the merged versions more readable in its display
- it will probably be better for our performance

However, I don't think we should implement this change until we start considering the types of protocol members for assignability/subtyping/equivalence. That will lead to significant changes in the inner structure of synthesized protocols.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-12 23:31_

---

_Label `type properties` added by @AlexWaygood on 2025-05-12 23:31_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-12 23:31_

---

_Label `Protocols` added by @AlexWaygood on 2025-05-14 11:03_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:16_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:17_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:17_

---

_Removed from milestone `GA` by @AlexWaygood on 2025-10-17 09:51_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:48_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
