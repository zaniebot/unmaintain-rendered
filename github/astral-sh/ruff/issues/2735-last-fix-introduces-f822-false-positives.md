---
number: 2735
title: last fix introduces F822 false positives
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-10T19:42:36Z
updated_at: 2023-02-10T20:16:22Z
url: https://github.com/astral-sh/ruff/issues/2735
synced_at: 2026-01-07T13:12:14-06:00
---

# last fix introduces F822 false positives

---

_Issue opened by @spaceone on 2023-02-10 19:42_

0377834f9fee0fecc322168eec05f5f9f3d83385 introduces `F822` false positives:

```
__all__ = ['foo']
foo = 1
def bar():
    pass
```

unusual is that it passed when no function `bar()` is defined.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 19:44_

---

_Label `bug` added by @charliermarsh on 2023-02-10 19:44_

---

_Referenced in [astral-sh/ruff#2738](../../astral-sh/ruff/pulls/2738.md) on 2023-02-10 20:09_

---

_Closed by @charliermarsh on 2023-02-10 20:16_

---
