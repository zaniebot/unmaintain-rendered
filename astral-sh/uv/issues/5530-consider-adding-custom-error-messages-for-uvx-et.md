---
number: 5530
title: "Consider adding custom error messages for `uvx` et al"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2024-07-28T21:47:47Z
updated_at: 2024-08-10T03:21:58Z
url: https://github.com/astral-sh/uv/issues/5530
synced_at: 2026-01-10T01:23:49Z
---

# Consider adding custom error messages for `uvx` et al

---

_Issue opened by @charliermarsh on 2024-07-28 21:47_

Oops, I ran the following:

```shell
❯ uvx add pipx
warning: `uvx` is experimental and may change without warning
error: Because there are no versions of add and you require add, we can conclude that the requirements are unsatisfiable.
```

---

_Label `error messages` added by @charliermarsh on 2024-07-28 21:47_

---

_Comment by @zanieb on 2024-07-30 13:29_

We should at least add context before the resolver error message.

---

_Assigned to @zanieb by @charliermarsh on 2024-07-30 16:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 14:05_

---

_Unassigned @zanieb by @charliermarsh on 2024-08-09 14:05_

---

_Referenced in [astral-sh/uv#5991](../../astral-sh/uv/pulls/5991.md) on 2024-08-10 03:03_

---

_Closed by @charliermarsh on 2024-08-10 03:21_

---

_Closed by @charliermarsh on 2024-08-10 03:21_

---
