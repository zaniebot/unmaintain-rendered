---
number: 14177
title: "Error messages mistakenly list pre-releases for (e.g.) `==2.4.*` specifiers"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
created_at: 2025-06-21T02:40:20Z
updated_at: 2025-06-27T18:06:20Z
url: https://github.com/astral-sh/uv/issues/14177
synced_at: 2026-01-07T13:12:18-06:00
---

# Error messages mistakenly list pre-releases for (e.g.) `==2.4.*` specifiers

---

_Issue opened by @charliermarsh on 2025-06-21 02:40_

Apologies for the screenshot, but notice that `==2.4.*` gets expanded to `>=2.4.dev0,<2.5.dev0`, which we then interpreter as a pre-release in the hint. We should "contract" these in error messages, I think.

![Image](https://github.com/user-attachments/assets/ab8e944d-3305-44f6-8aaf-c6e782fa54ff)

---

_Label `error messages` added by @charliermarsh on 2025-06-21 02:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-21 18:25_

---

_Label `bug` added by @charliermarsh on 2025-06-21 20:23_

---

_Referenced in [astral-sh/uv#14195](../../astral-sh/uv/pulls/14195.md) on 2025-06-21 23:04_

---

_Closed by @zanieb on 2025-06-27 18:06_

---
