```yaml
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
synced_at: 2026-01-12T15:54:43Z
```

# `--add-noqa` introduces syntax error for continuations

---

_@charliermarsh_

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

_Closed by @charliermarsh on 2023-02-11 23:29_

---
