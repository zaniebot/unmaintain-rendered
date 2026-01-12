```yaml
number: 6148
title: RUF015 auto fix changes behavior
type: issue
state: closed
author: jc-louis
labels:
  - bug
assignees: []
created_at: 2023-07-28T12:23:58Z
updated_at: 2023-07-28T13:38:15Z
url: https://github.com/astral-sh/ruff/issues/6148
synced_at: 2026-01-12T15:54:45Z
```

# RUF015 auto fix changes behavior

---

_@jc-louis_

Using ruff v0.0.280 with `--fix` the rule RUF015 changes code behavior:

```python
# before RUF015 auto fix
def fct(a: list, b: list):
    return a + [ele for ele in b if ele][:1]


print(fct([1], [0]))  # [1]


# after RUF015 auto fix
def fct(a: list, b: list):
    return [*a, next(ele for ele in b if ele)]


print(fct([1], [0]))  # raises StopIteration
```

---

_Label `bug` added by @charliermarsh on 2023-07-28 12:50_

---

_Comment by @charliermarsh on 2023-07-28 12:50_

We should probably remove the slice cases here.

---

_Closed by @charliermarsh on 2023-07-28 13:38_

---
