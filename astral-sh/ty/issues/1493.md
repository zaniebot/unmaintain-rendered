---
number: 1493
title: Unpacking a typed dictionary into another should transfer defined keys.
type: issue
state: open
author: lypwig
labels:
  - typeddict
assignees: []
created_at: 2025-11-06T07:43:49Z
updated_at: 2025-12-30T00:48:00Z
url: https://github.com/astral-sh/ty/issues/1493
synced_at: 2026-01-10T01:51:14Z
---

# Unpacking a typed dictionary into another should transfer defined keys.

---

_Issue opened by @lypwig on 2025-11-06 07:43_

### Summary

```python
from typing import TypedDict

class MyTypedDict1(TypedDict):
    aaa: int
    bbb: int


class MyTypedDict2(TypedDict):
    aaa: int
    bbb: int
    ccc: int

d1: MyTypedDict1 = {
    "aaa": 1,
    "bbb": 2,
}

d2: MyTypedDict2 = {
    **d1,
    "ccc": 3,
}
```

> Missing required key 'aaa' in TypedDict `MyTypedDict2` constructor (missing-typed-dict-key) [Ln 18, Col 20]
> Missing required key 'bbb' in TypedDict `MyTypedDict2` constructor (missing-typed-dict-key) [Ln 18, Col 20]

https://play.ty.dev/5bf7c51c-fb7d-4a32-8fc3-799f1e014d76

### Version

0.0.1a25

---

_Renamed from "Unpacking a typed dictionnary into an other should transfer defined keys." to "Unpacking a typed dictionary into another should transfer defined keys." by @AlexWaygood on 2025-11-06 10:33_

---

_Label `typeddict` added by @carljm on 2025-11-06 13:52_

---

_Comment by @nathanscain on 2025-12-30 00:35_

Been hitting this for a bit. Confirmed it still is an issue in 0.0.8.

---

_Added to milestone `Stable` by @carljm on 2025-12-30 00:48_

---
