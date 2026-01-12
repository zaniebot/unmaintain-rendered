```yaml
number: 2123
title: exponential runtime for some large unions (part 2)
type: issue
state: open
author: markjm
labels:
  - performance
  - generics
assignees: []
created_at: 2025-12-19T23:28:53Z
updated_at: 2026-01-09T13:44:25Z
url: https://github.com/astral-sh/ty/issues/2123
synced_at: 2026-01-12T15:54:26Z
```

# exponential runtime for some large unions (part 2)

---

_@markjm_

### Summary

I'm back!

I want to express my gratitude for the quick fix on #1968. The repro works like a charm now. Unfortunately, when I tried out 0.0.3 (or 4) on the actual sourcecode which triggered the issue in our codebase, I could still repro the slowdown :(

I have below another similar repro which I see still has exponential runtime based on the size of the `StreamMessage` union:

```python
"""
ty type checker hang reproduction.
Run: uvx ty==0.0.4 check ty_repro.py
"""

from __future__ import annotations

from typing import Callable
from typing import Generic
from typing import TypeVar
from typing import Union


class M1: pass
class M2: pass
class M3: pass
class M4: pass
class M5: pass
class M6: pass
class M7: pass
class M8: pass
class M9: pass
class M10: pass
class M11: pass
class M12: pass
class M13: pass
class M14: pass
class M15: pass

# limiting the # of elements in this union improves runtime. For me, 12 elements takes 5sec, 13 takes 12sec, and grows further
StreamMessage = Union[M1, M2, M3, M4, M5, M6, M7, M8, M9, M10, M11, M12, M13, M14, M15]

T = TypeVar("T")
U_co = TypeVar("U_co", covariant=True)


class AsyncStream(Generic[T]):
    def apply(self, func: Callable[[AsyncStream[T]], AsyncStream[U_co]]) -> AsyncStream[U_co]:
        return func(self)

    def filter(self, predicate: Callable[[T], bool]) -> AsyncStream[T]:
        return AsyncStream()  # type: ignore

    def peek_index(self, action: Callable[[T, int], None]) -> AsyncStream[T]:
        return AsyncStream()  # type: ignore


TStreamMessage = TypeVar("TStreamMessage", bound=StreamMessage)


class Builder(Generic[TStreamMessage]):
    def build(self) -> AsyncStream[TStreamMessage]:
        stream = AsyncStream().filter(lambda x: True)  # type: ignore
        stream = stream.apply(self._handle_2).apply(self._handle_1)
        stream = stream.peek_index(self._peek)
        return stream

    def _handle_1(self, stream: AsyncStream[TStreamMessage]) -> AsyncStream[TStreamMessage]:
        return AsyncStream()  # type: ignore

    def _handle_2(self, stream: AsyncStream[StreamMessage]) -> AsyncStream[StreamMessage]:
        return AsyncStream()  # type: ignore

    def _peek(self, item: StreamMessage, index: int) -> None:
        pass
```

This is as much of a repro i could narrow before heading out for the week, so sorry its a bit messy. Happy holidays!

### Version

0.0.4

---

_Added to milestone `Stable` by @carljm on 2025-12-19 23:33_

---

_Label `performance` added by @carljm on 2025-12-19 23:33_

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:37_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:37_

---

_Label `generics` added by @carljm on 2025-12-20 00:53_

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:44_

---
