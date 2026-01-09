---
number: 2481
title: F841 - False positive
type: issue
state: closed
author: Mister7F
labels:
  - bug
assignees: []
created_at: 2023-02-02T16:09:55Z
updated_at: 2023-02-02T17:22:00Z
url: https://github.com/astral-sh/ruff/issues/2481
synced_at: 2026-01-07T13:12:14-06:00
---

# F841 - False positive

---

_Issue opened by @Mister7F on 2023-02-02 16:09_

Found this issue while playing with ruff, the variable `exponential` is used

```py
def f():
    exponential, base_multiplier = 1, 2
    hash_map = {
        (exponential := (exponential * base_multiplier) % 3): i + 1 for i in range(2)
    }
    return hash_map

# ruff --select=F test.py
# test.py:2:5: F841 Local variable `exponential` is assigned to but never used
```

---

_Label `bug` added by @charliermarsh on 2023-02-02 16:13_

---

_Comment by @charliermarsh on 2023-02-02 16:13_

Interesting, thanks!

---

_Comment by @charliermarsh on 2023-02-02 17:13_

Flake8 misses this too.

---

_Referenced in [astral-sh/ruff#2484](../../astral-sh/ruff/pulls/2484.md) on 2023-02-02 17:17_

---

_Closed by @charliermarsh on 2023-02-02 17:22_

---
