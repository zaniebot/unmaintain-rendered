---
number: 142
title: "`F821 Undefined name` outer class method using inner class as type hint"
type: issue
state: closed
author: nikolaik
labels:
  - bug
assignees: []
created_at: 2022-09-10T18:42:20Z
updated_at: 2022-09-10T19:20:40Z
url: https://github.com/astral-sh/ruff/issues/142
synced_at: 2026-01-07T13:12:14-06:00
---

# `F821 Undefined name` outer class method using inner class as type hint

---

_Issue opened by @nikolaik on 2022-09-10 18:42_

Sorry for spamming you with issues! Found another one while running ruff on a project for the first time:

```python
# yolo.py:
from enum import Enum

class Ticket:
    class Status(Enum):
        OPEN = 'OPEN'
        CLOSED = 'CLOSED'

    def set_status(self, status: Status):
        self.status = status
```

gives

```console
$ ruff yolo.py
yolo.py:9:34: F821 Undefined name `Status`

Found 1 error(s).
```

---

_Comment by @charliermarsh on 2022-09-10 18:51_

Np, the issues are appreciated!

---

_Referenced in [astral-sh/ruff#144](../../astral-sh/ruff/pulls/144.md) on 2022-09-10 19:14_

---

_Label `bug` added by @charliermarsh on 2022-09-10 19:14_

---

_Closed by @charliermarsh on 2022-09-10 19:20_

---
