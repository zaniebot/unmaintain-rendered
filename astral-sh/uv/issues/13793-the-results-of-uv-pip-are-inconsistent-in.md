---
number: 13793
title: The results of uv pip are inconsistent in PowerShell and cmd
type: issue
state: closed
author: RayLam2022
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-06-03T02:19:04Z
updated_at: 2025-06-03T04:29:53Z
url: https://github.com/astral-sh/uv/issues/13793
synced_at: 2026-01-10T01:25:38Z
---

# The results of uv pip are inconsistent in PowerShell and cmd

---

_Issue opened by @RayLam2022 on 2025-06-03 02:19_

### Question

In the directory where a .venv virtual environment has been created, running the uv pip list command in ​cmd​ displays the dependencies of that .venv, while in ​PowerShell, it shows the dependencies of the ​system's Python environment.

### Platform

Windows11

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

---

_Label `question` added by @RayLam2022 on 2025-06-03 02:19_

---

_Label `needs-mre` added by @samypr100 on 2025-06-03 02:46_

---

_Comment by @samypr100 on 2025-06-03 02:48_

@RayLam2022 I tried this and uv pip list behaves as expected. It's possible you may have different environment configuration between cmd and powershell.

---

_Closed by @RayLam2022 on 2025-06-03 04:29_

---
