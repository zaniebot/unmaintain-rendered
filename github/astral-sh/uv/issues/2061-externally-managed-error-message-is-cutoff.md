---
number: 2061
title: "`EXTERNALLY-MANAGED` error message is cutoff"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-02-29T00:14:30Z
updated_at: 2024-02-29T03:00:25Z
url: https://github.com/astral-sh/uv/issues/2061
synced_at: 2026-01-07T13:12:17-06:00
---

# `EXTERNALLY-MANAGED` error message is cutoff

---

_Issue opened by @charliermarsh on 2024-02-29 00:14_

It turns out that we only print the first line! The parsing takes place in here: https://github.com/astral-sh/uv/blob/9c63412526cdc1284e1825078af4c841058f7cde/crates/uv-interpreter/src/interpreter.rs#L270.

---

_Label `bug` added by @charliermarsh on 2024-02-29 00:14_

---

_Label `error messages` added by @charliermarsh on 2024-02-29 00:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 02:48_

---

_Referenced in [astral-sh/uv#2073](../../astral-sh/uv/pulls/2073.md) on 2024-02-29 02:49_

---

_Closed by @charliermarsh on 2024-02-29 03:00_

---
