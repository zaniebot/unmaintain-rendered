```yaml
number: 103
title: Check for overlapping overloads
type: issue
state: open
author: dhruvmanila
labels:
  - overloads
assignees: []
created_at: 2025-05-01T20:11:57Z
updated_at: 2025-06-11T01:02:59Z
url: https://github.com/astral-sh/ty/issues/103
synced_at: 2026-01-10T02:08:20Z
```

# Check for overlapping overloads

---

_Issue opened by @dhruvmanila on 2025-05-01 20:11_

This requires some discussion on what the concrete rules are. There's some context on a discussion about this when the overloads chapter in the typing spec was being updated: https://github.com/python/typing/pull/1839#discussion_r1728160022.

For example (copied from the above discussion):

```py
from typing import Literal, overload

@overload
def is_one(x: Literal[1]) -> True: ...
@overload
def is_one(x: int) -> False: ...

def f(x: int) -> bool:
    return is_one(x)

f(1)  # Presumably True at runtime, but type checker will say False (unless it performs inlining)
```

---

_Renamed from "[red-knot] Check for overlapping overloads" to "Check for overlapping overloads" by @MichaReiser on 2025-05-07 15:24_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:01_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 01:02_

---

_Added to milestone `GA` by @carljm on 2025-06-11 01:02_

---
