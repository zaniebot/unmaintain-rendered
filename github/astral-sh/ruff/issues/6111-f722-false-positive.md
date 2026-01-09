---
number: 6111
title: (🐞) F722 false positive 
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2023-07-27T00:41:56Z
updated_at: 2023-07-27T01:32:55Z
url: https://github.com/astral-sh/ruff/issues/6111
synced_at: 2026-01-07T13:12:15-06:00
---

# (🐞) F722 false positive 

---

_Issue opened by @KotlinIsland on 2023-07-27 00:41_

```py
from __future__ import annotations

from typing import Literal

E = Literal["01"]  #  F722 Syntax error in forward annotation: `01`
```

Python 3.11
ruff 0.0.280

---

_Comment by @charliermarsh on 2023-07-27 01:32_

This is fixed on main, sorry about that.

---

_Closed by @charliermarsh on 2023-07-27 01:32_

---
