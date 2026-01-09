---
number: 4408
title: Suppress ANN401 for methods annotated with typing_extensions.override
type: issue
state: closed
author: sirrus233
labels:
  - bug
assignees: []
created_at: 2023-05-12T23:39:56Z
updated_at: 2023-05-13T15:20:06Z
url: https://github.com/astral-sh/ruff/issues/4408
synced_at: 2026-01-07T13:12:14-06:00
---

# Suppress ANN401 for methods annotated with typing_extensions.override

---

_Issue opened by @sirrus233 on 2023-05-12 23:39_

I believe this is a follow-on request from: https://github.com/charliermarsh/ruff/issues/3952

Where that discussion landed was that it is reasonable to suppress `ARG002` for overridden methods, since you cannot control the method signature. I think it follows that `ANN401` should be suppressed as well, since if we cannot control the method signature we also cannot control the types, and it is impossible to narrow or remove `Any` in such cases.

---

_Referenced in [astral-sh/ruff#4409](../../astral-sh/ruff/pulls/4409.md) on 2023-05-13 10:11_

---

_Label `bug` added by @charliermarsh on 2023-05-13 12:59_

---

_Closed by @charliermarsh on 2023-05-13 15:20_

---
