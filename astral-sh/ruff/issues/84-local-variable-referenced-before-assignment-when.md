```yaml
number: 84
title: "\"Local variable `...` referenced before assignment\" when shadowing"
type: issue
state: closed
author: orsinium
labels:
  - bug
assignees: []
created_at: 2022-09-02T09:34:41Z
updated_at: 2022-09-02T13:01:12Z
url: https://github.com/astral-sh/ruff/issues/84
synced_at: 2026-01-12T15:54:40Z
```

# "Local variable `...` referenced before assignment" when shadowing

---

_@orsinium_

Code:

```python
def dec(x):
    return x

@dec
def f():
    dec = 1
    return dec
```

Error:

```
./tmp.py:6:5: F832 Local variable `dec` referenced before assignment
```

---

_Label `bug` added by @charliermarsh on 2022-09-02 12:43_

---

_Comment by @charliermarsh on 2022-09-02 13:01_

Thanks! Fixed in #86.

---

_Closed by @charliermarsh on 2022-09-02 13:01_

---
