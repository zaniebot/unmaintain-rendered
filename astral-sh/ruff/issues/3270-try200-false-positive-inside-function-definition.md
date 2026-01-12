```yaml
number: 3270
title: TRY200 false positive inside function definition inside except block
type: issue
state: closed
author: manueljacob
labels:
  - bug
assignees: []
created_at: 2023-02-28T10:59:47Z
updated_at: 2023-02-28T16:56:30Z
url: https://github.com/astral-sh/ruff/issues/3270
synced_at: 2026-01-12T15:54:43Z
```

# TRY200 false positive inside function definition inside except block

---

_@manueljacob_

```python
def good():
    try:
        from mod import f
    except ImportError:
        def f():
            raise MyException()
```

Currently, with ruff 0.0.253, this prints “TRY200 Use `raise from` to specify exception cause”, but setting the ImportError as the exception cause inside `f()` wouldn’t make any sense.

---

_Label `bug` added by @charliermarsh on 2023-02-28 15:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-28 16:34_

---

_Closed by @charliermarsh on 2023-02-28 16:56_

---
