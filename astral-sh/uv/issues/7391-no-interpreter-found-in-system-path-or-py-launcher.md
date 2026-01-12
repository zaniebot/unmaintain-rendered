```yaml
number: 7391
title: "No interpreter found in system path or `py` launcher"
type: issue
state: closed
author: michelkluger
labels:
  - question
assignees: []
created_at: 2024-09-14T14:16:34Z
updated_at: 2024-09-27T07:50:59Z
url: https://github.com/astral-sh/uv/issues/7391
synced_at: 2026-01-12T15:59:13Z
```

# No interpreter found in system path or `py` launcher

---

_@michelkluger_

❯ uv venv env
  × No interpreter found in system path or `py` launcher

❯ uv python install 3.11
warning: `uv python install` is experimental and may change without warning
Searching for Python versions matching: Python 3.11
Found existing installation for Python 3.11: cpython-3.11.9-windows-x86_64-none

❯ uv venv env
  × No interpreter found in system path or `py` launcher
  
details, uninstalled previous python from computer (also did not work with)
windows 11,  uv==0.4.10


---

_Comment by @charliermarsh on 2024-09-14 14:23_

I think you're on an older version of uv (unintentionally). "warning: uv python install is experimental and may change without warning" was removed in 0.3.

---

_Label `question` added by @charliermarsh on 2024-09-14 14:23_

---

_Comment by @charliermarsh on 2024-09-14 14:23_

How did you install uv? I'd suggest uninstalling it and reinstalling.

---

_Comment by @michelkluger on 2024-09-14 14:27_

> How did you install uv? I'd suggest uninstalling it and reinstalling.

let me try that, thank you for the prompt answer

---

_Comment by @michelkluger on 2024-09-14 14:49_

solved most of my issue

but still I have issues with sync that I dont with uv pip 

❯ uv sync
warning: `VIRTUAL_ENV=env` does not match the project environment path `.venv` and will be ignored
error: Querying Python at `\\?\C:\Users\MichelKluger\mypath\myproj\.venv\Scripts\python.exe` failed with exit status exit code: 103
--- stdout:

--- stderr:
No Python at '"C:\Users\MichelKluger\AppData\Roaming\uv\data\python\cpython-3.11.9-windows-x86_64-none\python.exe'
---


❯ uv pip install -r .\requirements-dev.txt
Audited 98 packages in 41ms


---

_Comment by @michelkluger on 2024-09-14 14:52_

❯ uv python list
cpython-3.13.0rc2-windows-x86_64-none    <download available>
cpython-3.12.6-windows-x86_64-none       <download available>
cpython-3.11.10-windows-x86_64-none      C:\Users\MichelKluger\AppData\Roaming\uv\python\cpython-3.11.10-windows-x86_64-none\python.exe
cpython-3.10.15-windows-x86_64-none      <download available>
cpython-3.9.20-windows-x86_64-none       <download available>
cpython-3.8.20-windows-x86_64-none       <download available>
cpython-3.7.9-windows-x86_64-none        <download available>
pypy-3.10.14-windows-x86_64-none         <download available>
pypy-3.9.19-windows-x86_64-none          <download available>
pypy-3.8.16-windows-x86_64-none          <download available>
pypy-3.7.13-windows-x86_64-none          <download available>

---

_Comment by @charliermarsh on 2024-09-25 21:53_

Can you try removing the `.venv` directory? They're likely using different Pythons.

---

_Comment by @michelkluger on 2024-09-26 08:10_

❯ uv sync
warning: `VIRTUAL_ENV=env` does not match the project environment path `.venv` and will be ignored
error: No interpreter found for Python ==3.11 in managed installations, system path, or `py` launcher

❯ ls .venv
Get-ChildItem: Cannot find path 'C:\Users\MichelKluger\mypath\myproj\\.venv' because it does not exist.

---

_Comment by @zanieb on 2024-09-26 11:55_

If you want to change the path of the target for `uv sync`, you need to set `UV_PROJECT_ENVIRONMENT=env`. However, I'd generally not recommend this. Is there a reason you need a non-default name?

---

_Comment by @michelkluger on 2024-09-26 12:15_

> If you want to change the path of the target for `uv sync`, you need to set `UV_PROJECT_ENVIRONMENT=env`. However, I'd generally not recommend this. Is there a reason you need a non-default name?

sorry, I dont understand the question

I am just trying to use uv sync, but is not working as expected on windows (maybe I dont understand something), uv pip install works

---

_Comment by @zanieb on 2024-09-26 14:21_

`uv sync` does not allow syncing to arbitrary active virtual environments, it targets the _project_ virtual environment which is at a fixed path (unless you configure the variable I shared). That's why you are getting a warning.

---

_Comment by @michelkluger on 2024-09-27 07:50_

> `uv sync` does not allow syncing to arbitrary active virtual environments, it targets the _project_ virtual environment which is at a fixed path (unless you configure the variable I shared). That's why you are getting a warning.

make sense, thank you for the clarifications

---

_Closed by @michelkluger on 2024-09-27 07:50_

---
