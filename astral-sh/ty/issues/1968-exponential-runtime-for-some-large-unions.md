```yaml
number: 1968
title: exponential runtime for some large unions
type: issue
state: closed
author: markjm
labels:
  - performance
assignees: []
created_at: 2025-12-17T01:37:23Z
updated_at: 2025-12-17T18:35:54Z
url: https://github.com/astral-sh/ty/issues/1968
synced_at: 2026-01-10T01:53:59Z
```

# exponential runtime for some large unions

---

_Issue opened by @markjm on 2025-12-17 01:37_

### Summary

Hi all - thank you so much for this promising new project! I am excited to get our company using `ty` ASAP. With the beta announcement today, I tried it out on our JUMBO codebase. In doing so, I have identified the below issue:

Included here is a minimal repro from our codebase (reduced as much as possible and business logic filtered out, i realize it looks silly but i figure it demonstrated the issue well):

```python
"""
Minimal reproduction for ty type checker exponential slowdown.

Run with:
    TY_MAX_PARALLELISM=1 ty check slow_ty_repro.py

Timings:
    4 types: <1s
    5 types: <1s
    6 types: ~8s
    7 types: >60s (hits my timeout)

Example debug logs:
2025-12-17 01:33:38.681798202 DEBUG left implies right left=(M6 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) right=(U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements)
2025-12-17 01:33:38.682086278 DEBUG left and right overlap left=(M6 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) right=(U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) intersection=(M6 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements)
2025-12-17 01:33:38.683285882 DEBUG left implies right left=(M3 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) right=(U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements)
2025-12-17 01:33:38.683605638 DEBUG left and right overlap left=(M3 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) right=(U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) intersection=(M3 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements)
2025-12-17 01:33:38.683894444 DEBUG left implies right left=(M3 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) right=(M3 ≤ U_co@apply)
2025-12-17 01:33:38.683919264 DEBUG left and right overlap left=(M3 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements) right=(M3 ≤ U_co@apply) intersection=(M3 ≤ U_co@apply ≤ M1 | M2 | M3 | ... omitted 4 union elements)

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

Msg = Union[M1, M2, M3, M4, M5, M6, M7]

T = TypeVar("T")
U_co = TypeVar("U_co", covariant=True)


class Stream(Generic[T]):
    def apply(self, func: Callable[["Stream[T]"], "Stream[U_co]"]) -> "Stream[U_co]":
        return func(self)


TMsg = TypeVar("TMsg", bound=Msg)


class Builder(Generic[TMsg]):
    def build(self) -> Stream[TMsg]:
        stream: Stream[TMsg] = Stream()
        stream = stream.apply(self._handler) # i think here is where the issue triggers?
        return stream

    def _handler(self, stream: Stream[Msg]) -> Stream[Msg]:
        return stream

```

this initially just surfaced as what seemed to be a hang during a repo-wide `ty check`, but debug logs allowed me to narrow down to this file then I just hacked away until i got something which seemed reasonable to share.

### Version

0.0.2

---

_Comment by @markjm on 2025-12-17 02:04_

PS - to report a secondary issue: I notice yall have https://play.ty.dev/ for sharing snippets. Attempting to paste this snippet there causes the web page to hang and crash (but idk how to capture debug information from that crash to showcase the issue)

---

_Comment by @zanieb on 2025-12-17 03:59_

Thank you for the minimized reproduction!

---

_Label `performance` added by @AlexWaygood on 2025-12-17 07:41_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 07:41_

---

_Comment by @zanieb on 2025-12-17 14:44_

Here's a subset of the flamegraph (~70% of the time) for a union of 6 items

 <img width="1477" height="640" alt="Image" src="https://github.com/user-attachments/assets/0fa66dc0-e298-4b60-a017-1d6f4e2eb578" />

Here's the whole flamegraph https://share.firefox.dev/4aTSiT2



---

_Comment by @dcreager on 2025-12-17 16:34_

I just pushed up https://github.com/astral-sh/ruff/pull/22030, and confirmed locally that it makes the execution time for this example not depend on the number of `M1`, `M2`, etc, classes.

---

_Closed by @dcreager on 2025-12-17 18:35_

---
