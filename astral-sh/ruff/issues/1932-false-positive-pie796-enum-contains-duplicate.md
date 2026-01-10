```yaml
number: 1932
title: "False-positive PIE796: Enum contains duplicate value: `enum.auto()`"
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2023-01-17T15:45:44Z
updated_at: 2023-01-17T16:14:13Z
url: https://github.com/astral-sh/ruff/issues/1932
synced_at: 2026-01-10T11:09:44Z
```

# False-positive PIE796: Enum contains duplicate value: `enum.auto()`

---

_Issue opened by @actionless on 2023-01-17 15:45_

ruff 0.0.224

```python
import enum


class PackageSource(enum.Enum):
    REPO = enum.auto()
    AUR = enum.auto()  # E: PIE796 Enum contains duplicate value: `enum.auto()`
    LOCAL = enum.auto()  # E: PIE796 Enum contains duplicate value: `enum.auto()`
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-17 16:04_

---

_Label `bug` added by @charliermarsh on 2023-01-17 16:04_

---

_Comment by @charliermarsh on 2023-01-17 16:04_

Ah right, totally. Will fix today.

---

_Closed by @charliermarsh on 2023-01-17 16:14_

---
