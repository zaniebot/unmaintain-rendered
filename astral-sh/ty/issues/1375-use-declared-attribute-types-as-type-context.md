```yaml
number: 1375
title: Use declared attribute types as type context
type: issue
state: closed
author: ibraheemdev
labels:
  - bidirectional inference
assignees: []
created_at: 2025-10-17T03:20:39Z
updated_at: 2025-10-31T16:41:15Z
url: https://github.com/astral-sh/ty/issues/1375
synced_at: 2026-01-12T15:54:25Z
```

# Use declared attribute types as type context

---

_@ibraheemdev_

I believe this is the last source of type context we are missing, e.g.,
```py
class X:
    x: list[Literal[1]]

def _(x: X):
    x.x = [1]
```

See https://github.com/astral-sh/ruff/pull/20796#issuecomment-3392376038 for details.

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-10-17 03:20_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:38_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-10-17 14:39_

---

_Comment by @ibraheemdev on 2025-10-31 15:47_

https://github.com/astral-sh/ruff/pull/21143 fixes the common cases, but still fails when assigning to union/intersection types, e.g.
```py
from typing import Literal

class X:
    x: list[Literal[1]]

class Y:
    x: list[Literal[1]]

def _(xy: X | Y):
    xy.x = [1]
```

---

_Closed by @ibraheemdev on 2025-10-31 15:48_

---

_Reopened by @ibraheemdev on 2025-10-31 15:55_

---

_Closed by @ibraheemdev on 2025-10-31 16:41_

---
