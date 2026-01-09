---
number: 2752
title: "[bug?] SLF001 does not detect private access to function return value"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2023-02-10T23:47:44Z
updated_at: 2023-02-11T00:23:24Z
url: https://github.com/astral-sh/ruff/issues/2752
synced_at: 2026-01-07T13:12:14-06:00
---

# [bug?] SLF001 does not detect private access to function return value

---

_Issue opened by @smackesey on 2023-02-10 23:47_

```
foo._bar  # SLF001 error
foo()._bar  # not an error
```

Is this a bug? Would expect ruff not to care whether we are accessing a `_`-prefixed symbol on a function return value or not.

---

_Label `bug` added by @charliermarsh on 2023-02-11 00:07_

---

_Comment by @charliermarsh on 2023-02-11 00:08_

Yeah I think that's a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-11 00:12_

---

_Referenced in [astral-sh/ruff#2753](../../astral-sh/ruff/pulls/2753.md) on 2023-02-11 00:21_

---

_Closed by @charliermarsh on 2023-02-11 00:23_

---
