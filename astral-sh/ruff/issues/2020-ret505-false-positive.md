---
number: 2020
title: RET505 false positive
type: issue
state: closed
author: Kludex
labels: []
assignees: []
created_at: 2023-01-20T10:22:04Z
updated_at: 2023-01-20T11:05:41Z
url: https://github.com/astral-sh/ruff/issues/2020
synced_at: 2026-01-10T01:22:40Z
---

# RET505 false positive

---

_Issue opened by @Kludex on 2023-01-20 10:22_

# Description

I'm having a RET505 on `elif` statements, where they shouldn't be raised. 

## Code

```python
from typing import Any


def parser(obj: Any) -> str:
    if isinstance(obj, dict):
        return "a"
    elif isinstance(obj, int):
        return "b"
    return "c"
```

## Command

```bash
ruff main.py
```

## Output

```bash
main.py:5:5: RET505 Unnecessary `elif` after `return` statement
```

---

_Comment by @bluetech on 2023-01-20 10:30_

Why shouldn't it be raised? The `elif` is indeed superfluous here (can be just `if`).

---

_Comment by @Kludex on 2023-01-20 11:05_

Hmmm... Ok. :eyes: 

Thanks.

---

_Closed by @Kludex on 2023-01-20 11:05_

---
