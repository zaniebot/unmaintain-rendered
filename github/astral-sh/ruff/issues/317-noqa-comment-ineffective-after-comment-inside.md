---
number: 317
title: noqa comment ineffective after comment inside tuple
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2022-10-04T05:18:30Z
updated_at: 2022-10-04T12:56:15Z
url: https://github.com/astral-sh/ruff/issues/317
synced_at: 2026-01-07T13:12:14-06:00
---

# noqa comment ineffective after comment inside tuple

---

_Issue opened by @andersk on 2022-10-04 05:18_

```python
(
    # comment
)

import os  # noqa: F401
```

Despite the `noqa` comment, ruff gives ``test.py:5:1: F401 `os` imported but unused``. A `git bisect` shows this was introduced by de9ceb2fe15cbcadfe0c74c36fc7f1cfdd112e7a.

---

_Label `bug` added by @charliermarsh on 2022-10-04 12:28_

---

_Referenced in [astral-sh/ruff#320](../../astral-sh/ruff/pulls/320.md) on 2022-10-04 12:55_

---

_Closed by @charliermarsh on 2022-10-04 12:56_

---
