```yaml
number: 12739
title: "\"uv venv global packages isolation issue\""
type: issue
state: open
author: misterdojo777
labels:
  - question
assignees: []
created_at: 2025-04-08T09:04:57Z
updated_at: 2025-04-08T19:43:34Z
url: https://github.com/astral-sh/uv/issues/12739
synced_at: 2026-01-12T16:01:11Z
```

# "uv venv global packages isolation issue"

---

_@misterdojo777_

### Summary


When creating a virtual environment with uv venv, the environment still shows all globally installed system packages instead of being isolated. I tried many different methods but the issue persisted. When using "uv venv" it will always include the globally installed packages even in a fresh venv. 

However, standard Python venvs (python -m venv) work correctly. Is this expected behavior, or is there a flag/configuration to make UV virtual environments properly isolated from system packages?

###  TEMP FIX: 
The problem is _ likely_ my own python setup, not with UV (Please provide feedback if this the case or not). For others facing similar issues here is the (temp) fix:

The **PYTHONNOUSERSITE**  environment variable set to 1 forces Python to ignore user site packages succesfully: 

`
set PYTHONNOUSERSITE=1
`




### Platform

Windows 11 64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

Python 3.11.9

---

_Label `bug` added by @misterdojo777 on 2025-04-08 09:04_

---

_Renamed from ""uv venv global packages isolation issue"" to ""uv venv global packages isolation issue" // FIXED" by @misterdojo777 on 2025-04-08 09:21_

---

_Renamed from ""uv venv global packages isolation issue" // FIXED" to ""uv venv global packages isolation issue"" by @misterdojo777 on 2025-04-08 09:46_

---

_Comment by @charliermarsh on 2025-04-08 15:36_

> When using "uv venv" it will always include the globally installed packages even in a fresh venv.

I don't think this is the case. Can you explain how you're getting to that conclusion? What behavior are you seeing that's leading you to say this?

---

_Label `bug` removed by @charliermarsh on 2025-04-08 19:43_

---

_Label `question` added by @charliermarsh on 2025-04-08 19:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-08 19:43_

---
