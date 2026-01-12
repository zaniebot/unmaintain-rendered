```yaml
number: 2467
title: "Generic class method \"does not satisfy upper bound\""
type: issue
state: open
author: Jeremiah-England
labels: []
assignees: []
created_at: 2026-01-12T14:11:55Z
updated_at: 2026-01-12T14:11:55Z
url: https://github.com/astral-sh/ty/issues/2467
synced_at: 2026-01-12T15:03:21Z
```

# Generic class method "does not satisfy upper bound"

---

_Issue opened by @Jeremiah-England on 2026-01-12 14:11_

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
