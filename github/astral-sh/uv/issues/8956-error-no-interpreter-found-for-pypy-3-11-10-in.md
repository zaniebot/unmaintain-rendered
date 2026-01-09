---
number: 8956
title: "error: No interpreter found for PyPy 3.11.10 in virtual environments, managed installations, or search path"
type: issue
state: closed
author: loke-x
labels:
  - question
assignees: []
created_at: 2024-11-08T20:27:59Z
updated_at: 2024-12-14T15:29:24Z
url: https://github.com/astral-sh/uv/issues/8956
synced_at: 2026-01-07T13:12:18-06:00
---

# error: No interpreter found for PyPy 3.11.10 in virtual environments, managed installations, or search path

---

_Issue opened by @loke-x on 2024-11-08 20:27_

I installed the python 3.11 using uv and pined it the directory and run uv init and got this issue

```bash
uv init                   
error: No interpreter found for PyPy 3.11.10 in virtual environments, managed installations, or search path
```

```bash
uv init --verbose
DEBUG uv 0.5.0
DEBUG Reading Python requests from version file at `/home/lokesh/Documents/0-lab/ox-ai/x-act/.python-version`
DEBUG Searching for PyPy 3.11.10 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/lokesh/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.0-linux-x86_64-gnu`
DEBUG Found managed installation `cpython-3.11.10-linux-x86_64-gnu`
DEBUG Found `cpython-3.11.10-linux-x86_64-gnu` at `/home/lokesh/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/bin/python3.11` (managed installations)
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.11.10`
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.11.10`
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/bin/python` (search path)
DEBUG Skipping interpreter at `/bin/python` from search path: does not satisfy request `3.11.10`
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/bin/python3` (search path)
DEBUG Skipping interpreter at `/bin/python3` from search path: does not satisfy request `3.11.10`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/lokesh/.local/share/uv/python`
DEBUG Released lock at `/home/lokesh/.local/share/uv/python/.lock`
error: No interpreter found for PyPy 3.11.10 in virtual environments, managed installations, or search path
```



---

_Comment by @zanieb on 2024-11-08 23:13_

Only PyPy 3.8, 3.9, and 3.10 exist https://github.com/pypy/pypy/milestone/15

---

_Comment by @zanieb on 2024-11-08 23:14_

Did you pin to `pypy3.11` by accident?

---

_Label `question` added by @charliermarsh on 2024-11-09 02:12_

---

_Comment by @loke-x on 2024-11-13 04:50_

> Did you pin to `pypy3.11` by accident?

Yup I guess when I pinned the python to 3.11 I did it 

After I role back the issue is resolved 

Thankyou so much 

---

_Closed by @loke-x on 2024-11-13 04:51_

---

_Comment by @NomiJ on 2024-12-14 10:10_

> Did you pin to `pypy3.11` by accident?

I am a first-time user and I also encountered the same problem. 

It's written here https://docs.astral.sh/uv/#python-management

>>Use a specific Python version in the current directory:

uv python pin pypy@3.11

---

_Comment by @zanieb on 2024-12-14 15:29_

Oh thanks @NomiJ — that shouldn't be in the example. See https://github.com/astral-sh/uv/pull/9896

---
