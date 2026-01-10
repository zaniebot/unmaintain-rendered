```yaml
number: 146
title: "`F841` false negative when access from inner function"
type: issue
state: closed
author: nikolaik
labels:
  - bug
assignees: []
created_at: 2022-09-10T20:41:51Z
updated_at: 2022-09-11T14:44:28Z
url: https://github.com/astral-sh/ruff/issues/146
synced_at: 2026-01-10T15:56:05Z
```

# `F841` false negative when access from inner function

---

_Issue opened by @nikolaik on 2022-09-10 20:41_

```python
# yolo.py:
def outer():
    x = 1

    def inner():
        print(x)

    inner()
```
```console
$ ruff yolo.py
yolo.py:2:5: F841 Local variable `x` is assigned to but never used

Found 1 error(s).
```

If I remove `def outer` and dedent the rest there is no lint error.

I see similar behaviour when `inner` is defined using `lambda`.

---

_Label `bug` added by @charliermarsh on 2022-09-11 13:43_

---

_Closed by @charliermarsh on 2022-09-11 14:44_

---
