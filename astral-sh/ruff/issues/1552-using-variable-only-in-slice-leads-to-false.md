```yaml
number: 1552
title: "Using variable only in slice leads to false positive for F841: Locale variable is assigned but never used."
type: issue
state: closed
author: Wooza
labels:
  - bug
assignees: []
created_at: 2023-01-02T12:05:19Z
updated_at: 2023-01-02T17:13:46Z
url: https://github.com/astral-sh/ruff/issues/1552
synced_at: 2026-01-10T12:05:30Z
```

# Using variable only in slice leads to false positive for F841: Locale variable is assigned but never used.

---

_Issue opened by @Wooza on 2023-01-02 12:05_

Applying ruff on the following code leads to `repro.py:6:5: F841 Local variable 'shift_by' is assigned to but never used`:
```python
# File repro.py
from __future__ import annotations


def foo() -> None:
    shift_by = 1
    for a in list([1, 2])[shift_by:]:
        print(a)
```

Tested on ruff version `0.0.206`

---

_Label `bug` added by @charliermarsh on 2023-01-02 17:12_

---

_Closed by @charliermarsh on 2023-01-02 17:13_

---
