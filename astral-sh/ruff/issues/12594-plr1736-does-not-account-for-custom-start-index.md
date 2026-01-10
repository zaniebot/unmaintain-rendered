---
number: 12594
title: PLR1736 does not account for custom start index
type: issue
state: closed
author: pork-you-pine
labels:
  - bug
assignees: []
created_at: 2024-07-31T15:04:16Z
updated_at: 2024-08-01T01:21:17Z
url: https://github.com/astral-sh/ruff/issues/12594
synced_at: 2026-01-10T01:22:52Z
---

# PLR1736 does not account for custom start index

---

_Issue opened by @pork-you-pine on 2024-07-31 15:04_

Hi there, it seems like PLR1736 does not account for custom start indices in enumerate and transforms:

```python 
def foo(some_list: list[int]):
    for index, list_item in enumerate(some_list, start=1):
        print(some_list[index])
```
into:

```python 
def foo(some_list: list[int]):
    for index, list_item in enumerate(some_list, start=1):
        print(list_item)
```

Used command:
`ruff check path/to/file.py --fix --isolated --select PL`

Version:
`ruff 0.5.5`

---

_Label `bug` added by @charliermarsh on 2024-07-31 15:04_

---

_Comment by @charliermarsh on 2024-07-31 15:04_

Thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-01 01:14_

---

_Referenced in [astral-sh/ruff#12603](../../astral-sh/ruff/pulls/12603.md) on 2024-08-01 01:17_

---

_Closed by @charliermarsh on 2024-08-01 01:21_

---

_Closed by @charliermarsh on 2024-08-01 01:21_

---
