```yaml
number: 939
title: eliminate definitely-impossible types from union in equality narrowing
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - narrowing
assignees: []
created_at: 2025-08-05T13:34:22Z
updated_at: 2025-09-03T15:34:47Z
url: https://github.com/astral-sh/ty/issues/939
synced_at: 2026-01-10T02:06:24Z
```

# eliminate definitely-impossible types from union in equality narrowing

---

_Issue opened by @carljm on 2025-08-05 13:34_

In this example:

```py
from typing import Literal

def test(x: Literal["a", "b", "c"] | None | int = None):
    if x in ("a", "b"):
        reveal_type(x)
```

We currently refuse to do any equality narrowing here because of the presence of `int` in the union (which may include subclasses of `int` with custom `__eq__`). But this is overly conservative; we should be able to eliminate `None` and `Literal["c"]` from the union anyway, resulting in the type `Literal["a", "b"] | int`.

---

_Label `help wanted` added by @carljm on 2025-08-05 13:34_

---

_Label `narrowing` added by @carljm on 2025-08-05 13:34_

---

_Comment by @Renkai on 2025-09-02 02:46_

proposed a PR at https://github.com/astral-sh/ruff/pull/20164, PTAL

---

_Closed by @carljm on 2025-09-03 15:34_

---
