```yaml
number: 4980
title: Windows Server 2019 uv access is denied when executing
type: issue
state: closed
author: Andrew-Chen-Wang
labels:
  - bug
  - windows
assignees: []
created_at: 2024-07-10T22:05:45Z
updated_at: 2024-11-08T02:57:40Z
url: https://github.com/astral-sh/uv/issues/4980
synced_at: 2026-01-12T15:58:53Z
```

# Windows Server 2019 uv access is denied when executing

---

_@Andrew-Chen-Wang_

I installed pyenv 3.12.4 in a Windows Server 2019 using AWS EC2. Created a virtualenv then tried running `uv pip install -r requirements.txt`. This results in 

```
(.venv) PS C:\Users\andrew\Downloads> uv pip install bin/requirements/requirements-dev.txt
? `bin/requirements/requirements-dev.txt` looks like a local requirements file but was passed as a package name. Did you✔ `bin/requirements/requirements-dev.txt` looks like a local requirements file but was passed as a package name. Did you mean `-r bin/requirements/requirements-dev.txt`? · yes
Resolved 54 packages in 900ms
   Built tasks @ file:///C:/Users/andrew/Downloads/projects/tools/tasks
Prepared 2 packages in 7.40s
error: failed to remove file `C:\Users\andrew\Downloads\.venv\Lib\site-packages\../../Scripts/uv.exe`
  Caused by: Access is denied. (os error 5)
```

Running `uv help` works just fine.

### Interesting solution

Renaming uv.exe from `.venv/Scripts/uv.exe` actually makes installation work

---

_Comment by @charliermarsh on 2024-07-10 22:06_

Did you install uv into the virtual environment itself? It looks like uv is trying to uninstall itself.

---

_Comment by @charliermarsh on 2024-07-10 22:06_

I believe this is the same as #1368.

---

_Comment by @Andrew-Chen-Wang on 2024-07-10 22:06_

Yes, after creating a virtualenv, we did `pip install uv`

---

_Comment by @charliermarsh on 2024-07-10 22:07_

Typically you would install uv globally, since uv doesn't need to be in the virtual environment in order to create it or install into it. (But it's also a known issue on Windows that uv cannot uninstall itself.)

---

_Label `bug` added by @charliermarsh on 2024-07-10 22:08_

---

_Label `windows` added by @charliermarsh on 2024-07-10 22:08_

---

_Comment by @charliermarsh on 2024-07-10 22:08_

I'm gonna merge into that other issue, but happy to answer questions there.

---

_Closed by @charliermarsh on 2024-07-10 22:08_

---

_Comment by @Andrew-Chen-Wang on 2024-07-10 22:08_

Ahh that's not really clear from the instructions. I assumed it's like `pip` where it needs to be installed in the virtualenv!

<img width="757" alt="Screenshot 2024-07-10 at 18 08 31" src="https://github.com/astral-sh/uv/assets/60190294/4d3056fc-2f45-4a73-8299-48806f862ef5">


---

_Comment by @charliermarsh on 2024-07-10 22:10_

Yeah uv will install into a currently-activated virtualenv even if it's not installed "in" the environment itself.

---

_Closed by @charliermarsh on 2024-11-08 02:57_

---
