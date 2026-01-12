```yaml
number: 11892
title: Why Windows path looks like this?
type: issue
state: open
author: MaxITService
labels:
  - question
  - windows
assignees: []
created_at: 2025-03-02T14:00:11Z
updated_at: 2025-03-07T17:11:28Z
url: https://github.com/astral-sh/uv/issues/11892
synced_at: 2026-01-12T16:00:49Z
```

# Why Windows path looks like this?

---

_@MaxITService_

### Question

Sorry for stupid question, This is Windows 11 PowerShell 7.4

 in last line:

```
uv venv --python python C:\Code\open-interpreter-venv

#Using CPython 3.13.1 interpreter at: scoop\apps\python\current\python.exe
#Creating virtual environment at: C:\Code\open-interpreter-venv
Activate with: C:\Code\open-interpreter-venv\Scripts\activate

C:\Code\open-interpreter-venv\Scripts\Activate.ps1

Get-Command python | Select-Object -ExpandProperty Source
#Produces strange output like
C:\Code\open-interpreter-venv/Scripts\python.exe

```
There is "/" forward slash, why is that? 

### Platform

Windows 11

### Version

uv 0.5.9 (0652800cb 2024-12-13)

---

_Label `question` added by @MaxITService on 2025-03-02 14:00_

---

_Label `windows` added by @konstin on 2025-03-03 18:45_

---

_Comment by @zanieb on 2025-03-07 17:11_

It looks like a bug. It's hard to display paths nicely across platforms.

---
