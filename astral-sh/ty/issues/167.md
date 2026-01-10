```yaml
number: 167
title: "enforce type of `ClassVar` in subclasses"
type: issue
state: closed
author: beauxq
labels:
  - typing semantics
assignees: []
created_at: 2025-03-20T13:41:47Z
updated_at: 2025-12-22T13:55:28Z
url: https://github.com/astral-sh/ty/issues/167
synced_at: 2026-01-10T01:56:40Z
```

# enforce type of `ClassVar` in subclasses

---

_Issue opened by @beauxq on 2025-03-20 13:41_

### Summary

This is for red knot, tested in https://playknot.ruff.rs/
```python
from typing import ClassVar, assert_type, reveal_type

class B:
    x: ClassVar[int | str] = 0

class C(B):
    x = "random"

class D(C):
    x = 1

class E(D):
    x = b"x"


def foo(b: type[B]) -> None:
    x = b.x
    reveal_type(x)
    if isinstance(x, str):
        print(x + "y")
    else:
        reveal_type(x)
        print(x + 3)


foo(E)
```
This program crashes with a `TypeError` because an invalid type is assigned to `x` in `E`.
But that invalid assignment is not reported.

### Version

https://playknot.ruff.rs/

---

_Comment by @MichaReiser on 2025-03-20 13:47_

[Here a link](https://playknot.ruff.rs/a7eef939-906a-4f53-ac84-a64f7f2d1fb6) to the playground with the given example.

---

_Comment by @carljm on 2025-03-20 23:03_

Thanks for the report! Yes, it's a known issue that in general we don't have any checking of Liskov validity of subclasses yet (for any method or attribute, of any type). One of many things that just aren't done yet!

In fact, it looks like while we have this on our internal roadmap, we don't yet have a GH issue for it. I just created astral-sh/ty#166 so that it's publicly tracked.

---

_Renamed from "[red-knot] enforce type of `ClassVar` in subclasses" to "enforce type of `ClassVar` in subclasses" by @MichaReiser on 2025-05-07 15:26_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:53_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 14:48_

---

_Added to milestone `GA` by @carljm on 2025-08-15 14:48_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:12_

---

_Comment by @AlexWaygood on 2025-12-22 13:55_

I've opened https://github.com/astral-sh/ty/issues/2158 with some more details about exactly what we need to support here. I'll close this in favour of that

---

_Closed by @AlexWaygood on 2025-12-22 13:55_

---
