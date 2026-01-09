---
number: 3242
title: False positive PLE1206 when using star expansion in logging call
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2023-02-27T01:28:39Z
updated_at: 2023-02-27T03:58:26Z
url: https://github.com/astral-sh/ruff/issues/3242
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive PLE1206 when using star expansion in logging call

---

_Issue opened by @adamtheturtle on 2023-02-27 01:28_

* ruff 0.0.252

```python
"""Demonstration of PLE1206 error."""

import logging

items = "foo", "bar"
logging.error("Example log %s, %s", *items)
```

```toml
[tool.ruff]
select = ["ALL"]
```

The code works, logging:

```
ERROR:root:Example log foo, bar
```

`ruff` shows:

```
example.py:6:1: PLE1206 Not enough arguments for `logging` format string
```

---

_Label `bug` added by @charliermarsh on 2023-02-27 01:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-27 03:54_

---

_Referenced in [astral-sh/ruff#3244](../../astral-sh/ruff/pulls/3244.md) on 2023-02-27 03:55_

---

_Closed by @charliermarsh on 2023-02-27 03:58_

---
