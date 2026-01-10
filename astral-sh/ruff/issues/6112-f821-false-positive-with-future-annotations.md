---
number: 6112
title: "(🐞) F821 false positive with `__future__.annotations`"
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2023-07-27T00:45:44Z
updated_at: 2023-07-27T01:33:06Z
url: https://github.com/astral-sh/ruff/issues/6112
synced_at: 2026-01-10T01:22:45Z
---

# (🐞) F821 false positive with `__future__.annotations`

---

_Issue opened by @KotlinIsland on 2023-07-27 00:45_

```py
from __future__ import annotations

from typing import Literal

E = Literal["P"]  # F821 Undefined name `P`
```

python 3.11
ruff 0.0.280

---

_Renamed from "(🐞) F821 false positive with `__Future__.annoations`" to "(🐞) F821 false positive with `__future__.annoations`" by @KotlinIsland on 2023-07-27 00:45_

---

_Renamed from "(🐞) F821 false positive with `__future__.annoations`" to "(🐞) F821 false positive with `__future__.annotations`" by @KotlinIsland on 2023-07-27 00:54_

---

_Comment by @charliermarsh on 2023-07-27 01:33_

This is fixed on main, thank you for reporting, sorry about that.

---

_Closed by @charliermarsh on 2023-07-27 01:33_

---
