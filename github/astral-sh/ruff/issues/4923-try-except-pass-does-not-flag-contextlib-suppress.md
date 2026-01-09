---
number: 4923
title: "`try-except-pass` does not flag `contextlib.suppress(Exception)`"
type: issue
state: open
author: tjkuson
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-07T11:49:26Z
updated_at: 2023-07-10T01:11:48Z
url: https://github.com/astral-sh/ruff/issues/4923
synced_at: 2026-01-07T13:12:15-06:00
---

# `try-except-pass` does not flag `contextlib.suppress(Exception)`

---

_Issue opened by @tjkuson on 2023-06-07 11:49_

According to the [Python documentation](https://docs.python.org/3/library/contextlib.html#contextlib.suppress), this

```python
import contextlib

with contextlib.suppress(Exception):
    1 / 0
```

is equivalent to

```python
try:
    1 / 0
except Exception:
    pass
```

`try-except-pass` flags the second code example but not the first.

The Python documentation says the two code snippets are the same, and `suppressible-exception` autofixes the second code example to look like the first. Therefore, I think the `try-except-pass` rule should also flag `contextlib.suppress(Exception)` and recommend logging the exception in the first code example.


---

_Label `question` added by @charliermarsh on 2023-06-08 17:41_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:11_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:11_

---

_Referenced in [astral-sh/ruff#18213](../../astral-sh/ruff/pulls/18213.md) on 2025-06-05 02:50_

---
