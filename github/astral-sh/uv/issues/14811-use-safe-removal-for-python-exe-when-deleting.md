---
number: 14811
title: "Use safe removal for `python.exe` when deleting virtual environments"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - windows
assignees: []
created_at: 2025-07-22T13:15:05Z
updated_at: 2025-11-09T14:31:00Z
url: https://github.com/astral-sh/uv/issues/14811
synced_at: 2026-01-07T13:12:19-06:00
---

# Use safe removal for `python.exe` when deleting virtual environments

---

_Issue opened by @zanieb on 2025-07-22 13:15_

As discussed in https://github.com/astral-sh/uv/issues/13986 and https://github.com/astral-sh/uv/issues/14808, we should use something like `self_replace` for `python.exe` in virtual environments, as it's common for the LSP to lock it.

---

_Label `enhancement` added by @zanieb on 2025-07-22 13:15_

---

_Label `windows` added by @zanieb on 2025-07-22 13:15_

---

_Closed by @zanieb on 2025-11-09 14:31_

---
