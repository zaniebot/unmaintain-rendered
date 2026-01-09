---
number: 2784
title: "`--add-noqa` introduces syntax error for continuations"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-02-11T23:10:36Z
updated_at: 2023-02-11T23:29:39Z
url: https://github.com/astral-sh/ruff/issues/2784
synced_at: 2026-01-07T13:12:14-06:00
---

# `--add-noqa` introduces syntax error for continuations

---

_Issue opened by @charliermarsh on 2023-02-11 23:10_

If you use `--add-noqa` on this snippet:

```py
print(1)
from very_long_module\
    import nothing
```

We change it to this, which is a syntax error:

```py
print(1)
from very_long_module\  # noqa: E402
    import nothing
```

---

_Label `bug` added by @charliermarsh on 2023-02-11 23:10_

---

_Referenced in [astral-sh/ruff#2783](../../astral-sh/ruff/pulls/2783.md) on 2023-02-11 23:10_

---

_Closed by @charliermarsh on 2023-02-11 23:29_

---
