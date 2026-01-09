---
number: 14557
title: difference between python version for installing uv and python version called
type: issue
state: closed
author: amrtn30
labels:
  - question
assignees: []
created_at: 2025-07-11T06:33:08Z
updated_at: 2025-07-21T09:57:34Z
url: https://github.com/astral-sh/uv/issues/14557
synced_at: 2026-01-07T13:12:18-06:00
---

# difference between python version for installing uv and python version called

---

_Issue opened by @amrtn30 on 2025-07-11 06:33_

### Question

I have a weird situation.

I have several versions of `python` on a server I use. Some don't have `pip` installed.

In my case, I installed it with `ensure pip` for `python` 3.12. I then installed `uv` using `pip`.

When creating a new project with `uv init example`, the python version used is 3.11, which is also installed.

Why ?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @amrtn30 on 2025-07-11 06:33_

---

_Comment by @mabdullahsoyturk on 2025-07-14 12:48_

See: https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions

---

_Closed by @charliermarsh on 2025-07-21 01:59_

---

_Comment by @charliermarsh on 2025-07-21 01:59_

The above docs should help, but also note that the version of Python you use when installing uv shouldn't really affect the versions you use when creating environments, etc. uv is installed independently of Python, so even if you install it with a Python 3.12 pip, you can still create virtual environments for any other Python version.

---

_Comment by @amrtn30 on 2025-07-21 06:15_

Thank you for your responses.

Indeed, from all installed python, the one used by default by uv is 3.11 (`uv python find`). Is it possible to impose the a specific version of python already installed ?

---

_Comment by @konstin on 2025-07-21 09:57_

Are you looking for `uv python pin --global`?

---
