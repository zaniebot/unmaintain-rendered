```yaml
number: 2723
title: "F401: false positive when __all__ contains used names"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-10T17:55:05Z
updated_at: 2023-02-10T19:32:07Z
url: https://github.com/astral-sh/ruff/issues/2723
synced_at: 2026-01-12T15:54:43Z
```

# F401: false positive when __all__ contains used names

---

_@spaceone_

`foo/__init__.py`:
```python
__all__ = ('foo', 'bar')
from foobarbaz import foo, bar
```

reports: ```F401: `foo` imported but unused; consider adding to `__all__` or using a redundant alias```

→ regression? latest unreleased ruff.

---

_Comment by @charliermarsh on 2023-02-10 17:58_

Does it error if you flip the order? (I agree that either should work.)

---

_Label `bug` added by @charliermarsh on 2023-02-10 17:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 19:13_

---

_Closed by @charliermarsh on 2023-02-10 19:32_

---
