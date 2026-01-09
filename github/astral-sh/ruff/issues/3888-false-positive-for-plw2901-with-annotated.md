---
number: 3888
title: False positive for PLW2901 with annotated assignment without value
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2023-04-05T17:04:54Z
updated_at: 2023-04-05T22:02:33Z
url: https://github.com/astral-sh/ruff/issues/3888
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive for PLW2901 with annotated assignment without value

---

_Issue opened by @dhruvmanila on 2023-04-05 17:04_

### Environment
* `ruff --version`: `0.0.261`

### Code (`t.py`):
```python
for i in []:
    i: int  # This is an annotated assignment without value
```

### Running `ruff`:
```console
$ ruff check --isolated --show-source --no-cache --select PLW2901 t.py 
t.py:2:5: PLW2901 `for` loop variable `i` overwritten by assignment target
  |
2 |     i: int  # no error
  |     ^ PLW2901
  |

Found 1 error.
```


---

_Label `bug` added by @charliermarsh on 2023-04-05 17:05_

---

_Referenced in [astral-sh/ruff#3891](../../astral-sh/ruff/pulls/3891.md) on 2023-04-05 18:53_

---

_Closed by @charliermarsh on 2023-04-05 22:02_

---
