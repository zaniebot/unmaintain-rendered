---
number: 12147
title: Some places that could be optimized
type: issue
state: closed
author: undefine-man
labels:
  - enhancement
assignees: []
created_at: 2025-03-13T02:54:41Z
updated_at: 2025-03-22T23:20:55Z
url: https://github.com/astral-sh/uv/issues/12147
synced_at: 2026-01-07T13:12:18-06:00
---

# Some places that could be optimized

---

_Issue opened by @undefine-man on 2025-03-13 02:54_

### Summary

0.Need a shortcut command to activate venv, e.g. `uv activate`.
1.Need an official command to migrate project to uv, though we can use `uvx migrate-to-ux`.
2.uv only manage difference python versions in project level. i cant use python in terminal if i dont install any python manually before install uv. I think this part can referenced to `nvm` , a project helps manage difference versions of `node`.  I can use `nvm use` to change to use which version of node when typing `node` command in terminal. But under `uv` management, i cant use `python` command in Windows cmd.
3.A better way to use mirror source, refer to a node project name `nrm`.

node:`nvm` + node:`nrm` + node:`npm` + python:venv = python: uv

### Example

I uninstall all python and only use uv to install and manage python.
![Image](https://github.com/user-attachments/assets/67f04ce4-2dbc-4954-a04a-569c56ad287a)

---

_Label `enhancement` added by @undefine-man on 2025-03-13 02:54_

---

_Comment by @charliermarsh on 2025-03-14 01:05_

Thanks! I think these are largely covered in other issues: #6265, #1910, etc.

---

_Closed by @charliermarsh on 2025-03-14 01:05_

---

_Comment by @zanieb on 2025-03-22 23:20_

> i cant use python in terminal if i dont install any python manually before install uv. 

You can also do `uv python install --preview --default`

---
