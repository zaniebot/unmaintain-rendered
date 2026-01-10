```yaml
number: 2182
title: Problem inferring nested generic classes with defaults
type: issue
state: open
author: cmp0xff
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-12-23T09:30:15Z
updated_at: 2026-01-08T18:04:06Z
url: https://github.com/astral-sh/ty/issues/2182
synced_at: 2026-01-10T01:56:41Z
```

# Problem inferring nested generic classes with defaults

---

_Issue opened by @cmp0xff on 2025-12-23 09:30_

### Summary

`ty` cannot infer nested generic classes with default values properly.

https://play.ty.dev/4ec45154-7726-41f8-b65c-920188c3c124

```py
from typing import Any, Generic
from collections.abc import Sequence
from typing_extensions import TypeVar, Self, reveal_type

_OrderableT = TypeVar("_OrderableT", default=Any)

class Interval(Generic[_OrderableT]):
    def __new__(cls, left: _OrderableT, right: _OrderableT) -> Self:
        return cls()

IntervalT = TypeVar("IntervalT", bound=Interval, default=Interval)

S2 = TypeVar("S2")

class IntervalIndex(Generic[S2]):
    def __new__(cls, data: Sequence[IntervalT]) -> "IntervalIndex[IntervalT]":
        return cls()

reveal_type(Interval(0, 1))  # type checkers all give Interval[int]
reveal_type(IntervalIndex([Interval(0, 1)]))  # mypy gives IntervalIndex[Interval[int]], ty gives IntervalIndex[Unknown]
```

### Version

5ea30c4c5

---

_Comment by @AlexWaygood on 2025-12-23 11:09_

Hmm, I hoped this might be fixed by https://github.com/astral-sh/ruff/pull/21930, but it doesn't seem like it.

---

_Label `generics` added by @carljm on 2025-12-23 20:07_

---

_Added to milestone `M1` by @carljm on 2025-12-23 20:07_

---

_Label `bidirectional inference` added by @carljm on 2026-01-08 18:02_

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-08 18:04_

---

_Added to milestone `Stable` by @carljm on 2026-01-08 18:04_

---
