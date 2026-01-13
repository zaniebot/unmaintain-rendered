```yaml
number: 2467
title: "Generic class method \"does not satisfy upper bound\""
type: issue
state: closed
author: Jeremiah-England
labels:
  - bug
  - generics
assignees: []
created_at: 2026-01-12T14:11:55Z
updated_at: 2026-01-13T01:23:32Z
url: https://github.com/astral-sh/ty/issues/2467
synced_at: 2026-01-13T02:20:57Z
```

# Generic class method "does not satisfy upper bound"

---

_@Jeremiah-England_

### Summary

We upgraded `ty` from `0.0.8` to `0.0.11` and I think this failure is causing 153/223 of our new errors. It was introduced in `0.0.10`.

playground link: https://play.ty.dev/0622514b-0f00-4d0b-8ca5-c61f454650a9

```python
"""Minimal stdlib-only repro for a `ty` failure mode."""

from __future__ import annotations

from dataclasses import dataclass
from typing import NewType

K = NewType("K", int)


@dataclass(frozen=True)
class C[T: K]:
    x: T

    def g(self):
        ...


C(x=K(0)).g()  # 1. Argument to bound method `g` is incorrect: Argument type `C[K]` does not satisfy upper bound `C[T@C]` of type variable `Self` [invalid-argument-type]
```



### Version

0.0.11

---

_Comment by @carljm on 2026-01-12 23:04_

Oof, that seems bad. Sorry about this! We'll get it fixed ASAP.

---

_Label `bug` added by @carljm on 2026-01-12 23:04_

---

_Label `generics` added by @carljm on 2026-01-12 23:04_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-12 23:04_

---

_Comment by @carljm on 2026-01-12 23:41_

Further minimized, the repro does not require dataclasses, but it does require the `NewType`:

```py
from typing import NewType

K = NewType("K", int)

class C[T: K]:
    def __init__(self, x: T) -> None:
        self.x = x

    def g(self):
        ...


C(x=K(0)).g()
```

---

_Comment by @carljm on 2026-01-13 00:28_

This bisects to https://github.com/astral-sh/ruff/pull/22105

---

_Assigned to @carljm by @carljm on 2026-01-13 01:09_

---

_Closed by @carljm on 2026-01-13 01:23_

---
