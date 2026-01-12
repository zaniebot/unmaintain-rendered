```yaml
number: 1231
title: resolve intersection/not for comprehensive overload matching
type: issue
state: open
author: KotlinIsland
labels:
  - wish
  - overloads
  - set-theoretic types
assignees: []
created_at: 2025-09-22T02:02:24Z
updated_at: 2025-11-18T16:10:38Z
url: https://github.com/astral-sh/ty/issues/1231
synced_at: 2026-01-12T15:54:24Z
```

# resolve intersection/not for comprehensive overload matching

---

_@KotlinIsland_

### Summary

```py
from ty_extensions import Not
from typing import overload, Literal

@overload
def f1(a: Literal[True]) -> int: ...

@overload
def f1(a: Literal[False]) -> str: ...

def f1(a):
    return 1

@overload
def f2(a: int) -> int: ...
@overload
def f2(a: Not[int]) -> str: ...

def f2(a):
    return 1

def main(b: bool, o: object):
    _ = f1(b) # correct: int | str
    _ = f2(o) # incorrect: Unknown
```

here the overloads of `int` and `~int` should combine to match the input `object` and result in `int | str`

### Version

_No response_

---

_Label `wish` added by @AlexWaygood on 2025-09-22 07:39_

---

_Label `overloads` added by @AlexWaygood on 2025-09-22 07:39_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-09-22 07:39_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
