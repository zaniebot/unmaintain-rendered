```yaml
number: 4824
title: RET504 when origin of return value is mixed
type: issue
state: closed
author: hynek
labels:
  - bug
assignees: []
created_at: 2023-06-03T04:35:13Z
updated_at: 2023-06-03T19:39:12Z
url: https://github.com/astral-sh/ruff/issues/4824
synced_at: 2026-01-10T11:09:47Z
```

# RET504 when origin of return value is mixed

---

_Issue opened by @hynek on 2023-06-03 04:35_

I have [a function](https://github.com/hynek/structlog/blob/a23932c4a8e87b9fcc79ef893d1822b2c5acf6aa/src/structlog/processors.py#L170C6-L217) in structlog that generates a function based on arguments and can be simplified to the following:

```python
def func(x):
    if x:
        def f(): ...
    else:
        f = 42

    return f
```

And this gives me:

```
t.py:8:12: RET504 Unnecessary variable assignment before `return` statement
```

It passes if both origins of `f` are a function definition or if both are simple value assignments.

Version is 0.0.270

---

_Label `bug` added by @charliermarsh on 2023-06-03 15:51_

---

_Comment by @charliermarsh on 2023-06-03 15:51_

👍 Can fix this today, thanks for reporting.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-03 19:09_

---

_Closed by @charliermarsh on 2023-06-03 19:39_

---
