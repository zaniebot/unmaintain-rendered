---
number: 11289
title: "[Question] uv add installs a python version instead of using /usr/bin/python"
type: issue
state: closed
author: WorldTeacher
labels:
  - question
assignees: []
created_at: 2025-02-06T19:25:57Z
updated_at: 2025-02-06T21:08:17Z
url: https://github.com/astral-sh/uv/issues/11289
synced_at: 2026-01-10T01:25:03Z
---

# [Question] uv add installs a python version instead of using /usr/bin/python

---

_Issue opened by @WorldTeacher on 2025-02-06 19:25_

### Question

I recently switched to using uv for my projects, but have noticed that in some cases `uv add [package] will create a .venv and install a python version (in this case 3.12.8) to the ~/.local/share/bin/uv folder, then use that python version instead of the installed one. 

Is this behavior intended or did I miss something in the docs?

Any answer is appreciated.

### Platform

Linux 6.13.1-arch1-1 x86_64 GNU/Linux

### Version

uv 0.5.27

---

_Label `question` added by @WorldTeacher on 2025-02-06 19:25_

---

_Comment by @zanieb on 2025-02-06 19:39_

What version is the Python at `/usr/bin/python`? We'll use it if it's compatible with your project, perhaps your project requires 3.12+?

---

_Comment by @WorldTeacher on 2025-02-06 20:07_

Python in /usr/bin is 3.13.1

when I create a new project (in this case a library) the .python-version is automatically set to 3.12 without any input on my side

---

_Comment by @WorldTeacher on 2025-02-06 20:19_

I updated uv to 0.5.29, same thing happens

---

_Comment by @zanieb on 2025-02-06 20:42_

You can request a Python version with `uv init -p 3.11`.

If you share verbose logs for `uv init -v` I could see why we're selecting 3.12 as the default version. 

---

_Comment by @WorldTeacher on 2025-02-06 20:52_

here's the log for `uv init`
```
DEBUG uv 0.5.29
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/user/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.8-linux-x86_64-gnu`
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/home/userlocal/share/uv/python/cpython-3.12.8-linux-x86_64-gnu/bin/python3.12` (managed installations)
DEBUG Writing Python versions to `/home/usertests/.python-version`
Initialized project `tests`
``` 

i then deleted the 3.12.8 and created a new folder, then it goes like this:

```
DEBUG uv 0.5.29
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/user/local/share/uv/python`
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/usr/bin/python` (first executable in the search path)
DEBUG Writing Python versions to `/home/usertest1/.python-version`
Initialized project `test1`
```

if i understand this correctly, uv will prefer the cpython present in .local/share/uv/python, even if another installation is present

---

_Comment by @zanieb on 2025-02-06 20:58_

Yes, the default `python-preference` is to use the managed distributions.

https://docs.astral.sh/uv/concepts/python-versions/#adjusting-python-version-preferences

---

_Comment by @WorldTeacher on 2025-02-06 21:08_

thanks, I have set the `python-preference` to system  in uv.toml and now it use system even with the 3.12.8 present in local/share





---

_Closed by @WorldTeacher on 2025-02-06 21:08_

---
