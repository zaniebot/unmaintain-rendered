```yaml
number: 1128
title: "Exhaustive check in `Enum` method doesn't narrow `self` type to `Never`"
type: issue
state: closed
author: lancelote
labels: []
assignees: []
created_at: 2025-09-04T15:27:07Z
updated_at: 2025-09-04T15:48:56Z
url: https://github.com/astral-sh/ty/issues/1128
synced_at: 2026-01-10T02:06:25Z
```

# Exhaustive check in `Enum` method doesn't narrow `self` type to `Never`

---

_Issue opened by @lancelote on 2025-09-04 15:27_

### Summary

given the following example ...

```python
from enum import Enum
from typing import assert_never


class Cell(Enum):
    EMPTY = "L"
    FLOOR = "."
    OCCUPIED = "#"

    def foo(self) -> None:
        match self:
            case Cell.EMPTY:
                ...

            case Cell.FLOOR:
                ...

            case Cell.OCCUPIED:
                ...

            case _:
                assert_never(self)

```

`ty` reports ...

```
error[type-assertion-failure]: Argument does not have asserted type `Never`
  --> sample.py:22:17
   |
21 |             case _:
22 |                 assert_never(self)
   |                 ^^^^^^^^^^^^^----^
   |                              |
   |                              Inferred type of argument is `Unknown & ~Literal[Cell.EMPTY] & ~Literal[Cell.FLOOR] & ~Literal[Cell.OCCUPIED]`
   |
info: `Never` and `Unknown & ~Literal[Cell.EMPTY] & ~Literal[Cell.FLOOR] & ~Literal[Cell.OCCUPIED]` are not equivalent types
info: rule `type-assertion-failure` is enabled by default
```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-04 15:48_

Thank you for reporting this.

This is related to the fact that we currently infer `Unknown` for `self`, see #159. If you explicitly annotate `self`, it should work: https://play.ty.dev/d5e36c28-0f0e-4055-b93b-a313692cb617

---

_Closed by @sharkdp on 2025-09-04 15:48_

---
