---
number: 7068
title: "`unused-variable` (`N806`) false positive on non-global type aliases"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2023-09-03T05:08:19Z
updated_at: 2023-09-04T08:44:30Z
url: https://github.com/astral-sh/ruff/issues/7068
synced_at: 2026-01-10T01:22:46Z
---

# `unused-variable` (`N806`) false positive on non-global type aliases

---

_Issue opened by @DetachHead on 2023-09-03 05:08_

```py
Foo = int | str # no error


def foo():
    Bar = int | str # error: Variable `Bar` in function should be lowercase
```

---

_Label `bug` added by @charliermarsh on 2023-09-03 11:59_

---

_Comment by @charliermarsh on 2023-09-03 12:00_

We can probably only support avoiding this when marked as `Bar: TypeAlias` (which I'd recommend).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 22:52_

---

_Referenced in [astral-sh/ruff#7119](../../astral-sh/ruff/pulls/7119.md) on 2023-09-03 22:54_

---

_Closed by @charliermarsh on 2023-09-04 08:44_

---
