```yaml
number: 5307
title: "N805: False positive with @abstractclassmethod"
type: issue
state: closed
author: mflova
labels:
  - bug
assignees: []
created_at: 2023-06-22T16:41:14Z
updated_at: 2024-08-02T17:18:11Z
url: https://github.com/astral-sh/ruff/issues/5307
synced_at: 2026-01-12T15:54:45Z
```

# N805: False positive with @abstractclassmethod

---

_@mflova_

Using the latest release.

Minimal example:

```python
from abc import ABC, abstractclassmethod

class Rule(ABC):
    @abstractclassmethod
    def check(cls) -> None:
        ...
```

It forces me to rename it to `self` because it is not able to detect the pressence of `abstractclassmethod`

This one seems to be deprecated since Python 3.3 so I am not sure if it makes sense to fix it

---

_Closed by @mflova on 2023-06-22 16:46_

---

_Reopened by @mflova on 2023-06-22 16:48_

---

_Comment by @charliermarsh on 2023-06-22 19:16_

I think it's reasonable to fix this, even if deprecated.

---

_Label `bug` added by @charliermarsh on 2023-06-22 19:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-22 19:16_

---

_Closed by @charliermarsh on 2023-06-22 19:52_

---
