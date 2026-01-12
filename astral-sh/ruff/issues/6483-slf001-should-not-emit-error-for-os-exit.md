```yaml
number: 6483
title: "`SLF001` should not emit error for `os._exit()`"
type: issue
state: closed
author: dosisod
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-10T18:51:49Z
updated_at: 2023-08-11T00:54:40Z
url: https://github.com/astral-sh/ruff/issues/6483
synced_at: 2026-01-12T15:54:46Z
```

# `SLF001` should not emit error for `os._exit()`

---

_@dosisod_

The [`os._exit()`](https://docs.python.org/3/library/os.html#os._exit) function is often used to exit without doing any cleanup, either because you don't have any resources to cleanup, or you're running in another thread/subprocess like the docs describe.

Ruff shouldn't emit a `SLF001` error because it isn't "private" per-se: it's well documented, and the `_` prefix is probably indented to encourage people to use `sys.exit()` unless they need `os._exit()`.

Example:

```python
import os

os._exit(1)
```

Ruff output:

```
$ ruff --version
ruff 0.0.284

$ ruff x.py
x.py:3:1: SLF001 Private member accessed: `_exit`
Found 1 error.
```

Expected: no error.

---

_Label `bug` added by @charliermarsh on 2023-08-10 22:50_

---

_Label `accepted` added by @charliermarsh on 2023-08-10 22:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-11 00:44_

---

_Closed by @charliermarsh on 2023-08-11 00:54_

---
