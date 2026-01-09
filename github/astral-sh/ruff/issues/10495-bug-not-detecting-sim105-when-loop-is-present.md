---
number: 10495
title: "Bug: not detecting `SIM105` when loop is present"
type: issue
state: closed
author: jamesbraza
labels:
  - rule
assignees: []
created_at: 2024-03-20T18:51:05Z
updated_at: 2024-03-21T13:47:40Z
url: https://github.com/astral-sh/ruff/issues/10495
synced_at: 2026-01-07T13:12:15-06:00
---

# Bug: not detecting `SIM105` when loop is present

---

_Issue opened by @jamesbraza on 2024-03-20 18:51_

With `ruff==0.3.3`, I think [SIM105](https://docs.astral.sh/ruff/rules/suppressible-exception/) should be triggered here, but it's not:

```python
try:
    for _ in range(1):
        pass
except (NotImplementedError, ValueError):
    pass
```

In other words, I think the above code snippet should be autofixed to:

```python
import contextlib


with contextlib.suppress(NotImplementedError, ValueError):
    for _ in range(1):
        pass
```

---

_Label `rule` added by @charliermarsh on 2024-03-21 13:46_

---

_Comment by @charliermarsh on 2024-03-21 13:47_

I think this can be merged into https://github.com/astral-sh/ruff/issues/8593.

---

_Closed by @charliermarsh on 2024-03-21 13:47_

---
