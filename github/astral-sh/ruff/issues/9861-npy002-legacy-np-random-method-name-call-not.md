---
number: 9861
title: "NPY002 legacy np.random.{method_name} call not triggering on `np.random.random()`"
type: issue
state: closed
author: jack-mcivor
labels:
  - bug
assignees: []
created_at: 2024-02-06T17:57:40Z
updated_at: 2024-02-07T14:54:13Z
url: https://github.com/astral-sh/ruff/issues/9861
synced_at: 2026-01-07T13:12:15-06:00
---

# NPY002 legacy np.random.{method_name} call not triggering on `np.random.random()`

---

_Issue opened by @jack-mcivor on 2024-02-06 17:57_

Using ruff v0.2.1

```python
import numpy as np

np.random.random()
```

```bash
$ ruff check --isolated --select=NPY002 tmp.py
```

I expect this to trigger a violation - afaict np.random.random() is deprecated just like any other np.random.{method_name} call. Reference https://numpy.org/doc/stable/reference/random/legacy.html#functions-in-numpy-random

I'm not quite sure why it was left out of https://github.com/astral-sh/ruff/pull/2960


---

_Comment by @jack-mcivor on 2024-02-06 17:58_

I will try take

---

_Comment by @charliermarsh on 2024-02-06 19:04_

Looks like an oversight.

---

_Label `bug` added by @charliermarsh on 2024-02-06 19:04_

---

_Assigned to @jack-mcivor by @charliermarsh on 2024-02-06 19:04_

---

_Referenced in [astral-sh/ruff#9862](../../astral-sh/ruff/pulls/9862.md) on 2024-02-06 20:11_

---

_Closed by @charliermarsh on 2024-02-07 14:54_

---

_Referenced in [scikit-rf/scikit-rf#1029](../../scikit-rf/scikit-rf/pulls/1029.md) on 2024-02-19 18:44_

---
