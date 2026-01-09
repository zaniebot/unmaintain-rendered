---
number: 3131
title: "PLW0120 (`else` clause on loop without a `break` statement) false positive inside a `match`"
type: issue
state: closed
author: bluetech
labels:
  - bug
assignees: []
created_at: 2023-02-22T16:24:18Z
updated_at: 2023-02-22T19:12:46Z
url: https://github.com/astral-sh/ruff/issues/3131
synced_at: 2026-01-07T13:12:14-06:00
---

# PLW0120 (`else` clause on loop without a `break` statement) false positive inside a `match`

---

_Issue opened by @bluetech on 2023-02-22 16:24_

Excited by the new `match` support, I ran it on a big project which contains a lot of match statements. The following is the only issue.

Snippet:

```py
while 1:
    match 10:
        case _:
            break
else:
    print('here')
```

Command: `ruff --isolated --select PLW0120 x.py`

Expected: no violations -- the `else` is definitely not redundant here.

Actual:

```
x.py:5:1: PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
```

Version: 0.0.251


---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-22 18:16_

---

_Label `bug` added by @charliermarsh on 2023-02-22 18:17_

---

_Comment by @charliermarsh on 2023-02-22 18:18_

Thanks, will fix today.

---

_Referenced in [astral-sh/ruff#3136](../../astral-sh/ruff/pulls/3136.md) on 2023-02-22 19:10_

---

_Closed by @charliermarsh on 2023-02-22 19:12_

---
