```yaml
number: 93
title: Stack overflows for recursive protocols
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-05-07T07:04:56Z
updated_at: 2025-08-15T01:25:13Z
url: https://github.com/astral-sh/ty/issues/93
synced_at: 2026-01-10T02:06:24Z
```

# Stack overflows for recursive protocols

---

_Issue opened by @sharkdp on 2025-05-07 07:04_

astral-sh/ruff#17880 fixed one particular stack overflow involving `is_fully_static`-checks on recursive protocols, but there are other type properties that have the same problem (assignability, equivalence). To fix this, we probably need a more general solution that might also supersede the specific handling for `is_fully_static`.

Example with stack overflow:
```py
from __future__ import annotations

from typing import Protocol

class P(Protocol):
    parent: P

class Q(Protocol):
    parent: Q

def _(p: P, q: Q):
    p = q
```

Existing tests, commented out:

https://github.com/astral-sh/ruff/blob/04457f99b6ba69dff38c1163a1668b6332ebd8b3/crates/ty_python_semantic/resources/mdtest/protocols.md?plain=1#L1585-L1594

---

_Label `bug` added by @sharkdp on 2025-05-07 07:04_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-07 07:07_

---

_Added to milestone `ty beta` by @AlexWaygood on 2025-05-07 07:09_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-07 07:10_

---

_Renamed from "[ty] Stack overflows for recursive protocols" to "Stack overflows for recursive protocols" by @MichaReiser on 2025-05-07 15:24_

---

_Closed by @sharkdp on 2025-05-09 12:54_

---

_Closed by @sharkdp on 2025-05-09 12:54_

---

_Comment by @MichaReiser on 2025-05-09 12:56_

Let's keep this open because we can only handle self-referential protocols. See https://github.com/astral-sh/ruff/pull/17929#issuecomment-2863922607

---

_Reopened by @MichaReiser on 2025-05-09 12:56_

---

_Comment by @sharkdp on 2025-05-09 12:58_

In particular, there are still problems with mutually-recursive protocols, see https://github.com/astral-sh/ruff/pull/17929#discussion_r2080141116.

---

_Label `fatal` added by @AlexWaygood on 2025-05-10 18:00_

---

_Comment by @LordAro on 2025-06-10 14:18_

In the interests of reporting, I've got this with `ty 0.0.1-alpha.8`, which I *think* is after the above was merged? So this is a separate issue for Protocols that are self-referential? Unless the use of Self circumvents that...

```python
from __future__ import annotations

from typing import Protocol, Self

class Loc(Protocol):
    all_loc: Self
    def __getitem__(self, item: str) -> Loc | None: ...
```

Removing either line from Loc allows it to pass

---

_Assigned to @carljm by @carljm on 2025-06-10 23:51_

---

_Unassigned @sharkdp by @carljm on 2025-06-10 23:51_

---

_Comment by @carljm on 2025-06-11 01:12_

Yeah, the use of `Self` here adds an indirection to the recursion that bypasses the current simple self-recursion detection.

---

_Comment by @carljm on 2025-08-15 01:25_

I think we can close this issue as recursive protocols generally do not stack overflow anymore? There may be some more specific bugs yet, but those should get individual issues opened for them.

---

_Closed by @carljm on 2025-08-15 01:25_

---
