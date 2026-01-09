---
number: 3655
title: Ruff ignores nested string annotations
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-21T20:24:44Z
updated_at: 2023-03-26T01:56:11Z
url: https://github.com/astral-sh/ruff/issues/3655
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff ignores nested string annotations

---

_Issue opened by @charliermarsh on 2023-03-21 20:24_

Mypy respects the use of `Path` here:

```python
from pathlib import Path

x: """list['Path']""" = []
```

We consider it an unused import.


---

_Label `bug` added by @charliermarsh on 2023-03-21 20:24_

---

_Comment by @charliermarsh on 2023-03-21 20:34_

There's no reason (AFAIK) to do this as a user, but it _is_ valid.

---

_Referenced in [astral-sh/ruff#3657](../../astral-sh/ruff/pulls/3657.md) on 2023-03-22 16:09_

---

_Referenced in [astral-sh/ruff#3724](../../astral-sh/ruff/pulls/3724.md) on 2023-03-24 21:46_

---

_Closed by @charliermarsh on 2023-03-26 01:56_

---
