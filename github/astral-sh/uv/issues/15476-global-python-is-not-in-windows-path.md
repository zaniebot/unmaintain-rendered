---
number: 15476
title: global python is not in windows path
type: issue
state: closed
author: GoFireStar
labels:
  - question
assignees: []
created_at: 2025-08-24T07:10:45Z
updated_at: 2025-08-24T12:14:53Z
url: https://github.com/astral-sh/uv/issues/15476
synced_at: 2026-01-07T13:12:19-06:00
---

# global python is not in windows path

---

_Issue opened by @GoFireStar on 2025-08-24 07:10_

### Question

uv python pin --global 3.11

where python
D:\DEV\Python\Python310\python.exe # local python
D:\Miniconda3\python.exe # conda python

I can not find  global python in python when I want use it by script

result = subprocess.run(['where', 'python'], capture_output=True, text=True, check=False)
          if result.returncode == 0 and result.stdout.strip():
              return result.stdout.splitlines()[0].strip()


### Platform

windows

### Version

uv 0.8.5 (ce3728681 2025-08-05)


---

_Label `question` added by @GoFireStar on 2025-08-24 07:10_

---
