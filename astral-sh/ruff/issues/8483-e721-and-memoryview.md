```yaml
number: 8483
title: E721 and memoryview
type: issue
state: closed
author: hauntsaninja
labels: []
assignees: []
created_at: 2023-11-03T22:48:20Z
updated_at: 2023-11-04T13:41:59Z
url: https://github.com/astral-sh/ruff/issues/8483
synced_at: 2026-01-12T15:54:48Z
```

# E721 and memoryview

---

_@hauntsaninja_

```
λ cat x.py
def f(t):
    if t == str:
        pass
    elif t == bytes:
        pass
    elif t == memoryview:
        pass
λ ruff check --select=E721 x.py
x.py:2:8: E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
x.py:4:10: E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
Found 2 errors.
```
ruff should complain about comparison with memoryview as well

---

_Closed by @charliermarsh on 2023-11-04 13:41_

---
