---
number: 10120
title: Module rich with UV is not installed corectly
type: issue
state: closed
author: Mirjax2000
labels:
  - question
  - windows
assignees: []
created_at: 2024-12-23T13:14:06Z
updated_at: 2025-04-16T17:41:32Z
url: https://github.com/astral-sh/uv/issues/10120
synced_at: 2026-01-07T13:12:18-06:00
---

# Module rich with UV is not installed corectly

---

_Issue opened by @Mirjax2000 on 2024-12-23 13:14_

If i install rich with UV it is not working. If i installed with PIP it is working fine.
and i have this error:
Traceback (most recent call last):
  File "C:\WORK\Python\EDU\Udemy\100_days_of_code\udm_rock_paper_scissors.py", line 3, in <module>
    from rich.console import Console
  File "C:\WORK\Python\EDU\Udemy\100_days_of_code\.venv\Lib\site-packages\rich\console.py", line 46, in <module>
    from . import errors, themes
  File "C:\WORK\Python\EDU\Udemy\100_days_of_code\.venv\Lib\site-packages\rich\themes.py", line 1, in <module>
    from .default_styles import DEFAULT_STYLES
  File "C:\WORK\Python\EDU\Udemy\100_days_of_code\.venv\Lib\site-packages\rich\default_styles.py", line 3, in <module>
    from .style import Style
  File "C:\WORK\Python\EDU\Udemy\100_days_of_code\.venv\Lib\site-packages\rich\style.py", line 6, in <module>
    from main import randint
ModuleNotFoundError: No module named 'main'

uv.lock has this paths:
[[package]]
name = "rich"
version = "13.9.4"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "markdown-it-py" },
    { name = "pygments" },
]
sdist = { url = "https://files.pythonhosted.org/packages/ab/3a/0316b28d0761c6734d6bc14e770d85506c986c85ffb239e688eeaab2c2bc/rich-13.9.4.tar.gz", hash = "sha256:439594978a49a09530cff7ebc4b5c7103ef57baf48d5ea3184f21d9a2befa098", size = 223149 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl", hash = "sha256:6049d5e6ec054bf2779ab3358186963bac2ea89175919d699e378b99738c2a90", size = 242424 },
]


---

_Comment by @charliermarsh on 2024-12-23 13:45_

I'm confused because that line `from main import randint` doesn't exist in Rich. Can you try recreating the virtual environment?

---

_Label `question` added by @charliermarsh on 2024-12-23 13:45_

---

_Label `windows` added by @charliermarsh on 2024-12-23 13:45_

---

_Comment by @Mirjax2000 on 2024-12-23 13:48_

yes i did it many times. And it is not .venv issue. i think there is missing some dependecies. 

---

_Comment by @charliermarsh on 2024-12-23 13:50_

Try clearing the cache before you install? `uv cache clean`. It's not a case of missing dependencies -- the file that's being referenced just isn't part of Rich.

---

_Comment by @Mirjax2000 on 2024-12-23 14:05_

wow ```UV cache clean``` made it.
Thank you.

---

_Comment by @charliermarsh on 2024-12-23 14:07_

I'm not sure what happened. The cache must've gotten corrupted somehow? If you run into it repeatedly and reproducibly, just let me know.

---

_Closed by @charliermarsh on 2024-12-23 14:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-23 14:07_

---

_Comment by @kabouzeid on 2025-04-16 15:37_

Would it make sense to make all files in the cache read-only by default?

Accidentally modifying a file in a venv currently requires running `uv cache clean` and then `uv sync --reinstall` in every venv that uses the affected dependency. This can happen easily: for example, via "Go to Definition" in an IDE, followed by an accidental paste or deletion.

Read-only cache files would help prevent this. Alternatively, a command to verify the integrity of the cache or a specific venv could offer peace of mind when unsure if something was modified.

---

_Comment by @zanieb on 2025-04-16 17:41_

We use copy-on-write if a file system supports it, which I think is the ideal fix here. I think read only would be too disruptive.

---
