---
number: 6454
title: Spurious logging-too-many-args (PLE1205) if using the msg kwarg
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2023-08-09T18:10:59Z
updated_at: 2023-08-09T20:39:12Z
url: https://github.com/astral-sh/ruff/issues/6454
synced_at: 2026-01-07T13:12:15-06:00
---

# Spurious logging-too-many-args (PLE1205) if using the msg kwarg

---

_Issue opened by @adamtheturtle on 2023-08-09 18:10_

`logging.info`, `logging.warning`, `logging.error`, etc. each take `(msg, *args, **kwargs)`.
If `msg` is given as a keyword argument, ruff 0.0.283 gives an error (https://beta.ruff.rs/docs/rules/logging-too-many-args/), which is incorrect.

```python
# example.py

import logging

logging.info(msg="X")
```

```
ruff --select=PLE --isolated example.py
```

```
example.py:3:1: PLE1205 Too many arguments for `logging` format string
```

---

_Comment by @charliermarsh on 2023-08-09 18:33_

Thanks, will get this fixed shortly.

---

_Label `bug` added by @charliermarsh on 2023-08-09 18:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-09 20:04_

---

_Referenced in [astral-sh/ruff#6456](../../astral-sh/ruff/pulls/6456.md) on 2023-08-09 20:29_

---

_Closed by @charliermarsh on 2023-08-09 20:39_

---
