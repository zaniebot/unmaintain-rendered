---
number: 13191
title: uv init example command should also include a .gitignore file
type: issue
state: closed
author: beyg1
labels:
  - enhancement
assignees: []
created_at: 2025-04-29T11:01:44Z
updated_at: 2025-05-04T12:47:34Z
url: https://github.com/astral-sh/uv/issues/13191
synced_at: 2026-01-07T13:12:18-06:00
---

# uv init example command should also include a .gitignore file

---

_Issue opened by @beyg1 on 2025-04-29 11:01_

### Summary

the boilerplate app should also have a .gitgnore with .env or .env.local statements

### Example

_No response_

---

_Label `enhancement` added by @beyg1 on 2025-04-29 11:01_

---

_Comment by @charliermarsh on 2025-05-04 12:47_

`uv init` does create a `.gitignore`, but it doesn't include `.env`. I think that's ok, since the default `Python.gitignore` doesn't either.

---

_Closed by @charliermarsh on 2025-05-04 12:47_

---
