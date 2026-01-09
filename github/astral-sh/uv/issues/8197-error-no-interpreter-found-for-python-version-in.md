---
number: 8197
title: "error: No interpreter found for Python =={version} in managed installations, system path, or `py` launcher"
type: issue
state: closed
author: fraser-langton
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-10-15T04:20:51Z
updated_at: 2024-10-15T06:55:15Z
url: https://github.com/astral-sh/uv/issues/8197
synced_at: 2026-01-07T13:12:17-06:00
---

# error: No interpreter found for Python =={version} in managed installations, system path, or `py` launcher

---

_Issue opened by @fraser-langton on 2024-10-15 04:20_

Am I missing something here? What else do I need to configure?

```
> py -3.8 -m uv lock
error: No interpreter found for Python ==3.8 in managed installations, system path, or `py` launcher

> py -0p
 -V:3.13 *        C:\Users\fraser\AppData\Local\Programs\Python\Python313\python.exe
 -V:3.12          C:\Users\fraser\AppData\Local\Programs\Python\Python312\python.exe
 -V:3.11          C:\Users\fraser\AppData\Local\Programs\Python\Python311\python.exe
 -V:3.10          C:\Users\fraser\AppData\Local\Programs\Python\Python310\python.exe
 -V:3.8           C:\Users\fraser\AppData\Local\Programs\Python\Python38\python.exe

> uv python list
cpython-3.13.0-windows-x86_64-none     C:\Users\fraser\AppData\Local\Programs\Python\Python313\python.exe
cpython-3.13.0-windows-x86_64-none     <download available>
cpython-3.12.7-windows-x86_64-none     <download available>
cpython-3.12.2-windows-x86_64-none     C:\Users\fraser\AppData\Local\Programs\Python\Python312\python.exe
cpython-3.11.10-windows-x86_64-none    <download available>
cpython-3.11.4-windows-x86_64-none     C:\Users\fraser\AppData\Local\Programs\Python\Python311\python.exe
cpython-3.10.15-windows-x86_64-none    <download available>
cpython-3.10.11-windows-x86_64-none    C:\Users\fraser\AppData\Local\Programs\Python\Python310\python.exe
cpython-3.9.20-windows-x86_64-none     <download available>
cpython-3.8.20-windows-x86_64-none     <download available>
cpython-3.8.10-windows-x86_64-none     C:\Program Files\Python38\python.exe
pypy-3.10.14-windows-x86_64-none       <download available>
pypy-3.9.19-windows-x86_64-none        <download available>
pypy-3.8.16-windows-x86_64-none        <download available>
pypy-3.7.13-windows-x86_64-none        <download available>

```

---

_Comment by @fraser-langton on 2024-10-15 04:33_

Not even working if installing directly using uv

```
> uv python install 3.8
Searching for Python versions matching: Python 3.8
Installed Python 3.8.20 in 9.36s
 + cpython-3.8.20-windows-x86_64-none

> uv lock
error: No interpreter found for Python ==3.8 in managed installations, system path, or `py` launcher
```

---

_Comment by @zanieb on 2024-10-15 04:50_

Please see

- https://github.com/astral-sh/uv/issues/8169
- #7426

---

_Comment by @zanieb on 2024-10-15 04:51_

When you use ==3.8, per PEP 440 it means 3.8.0 not 3.8.* — you'll need to update the `requires-python` in your project.

---

_Label `duplicate` added by @zanieb on 2024-10-15 04:52_

---

_Label `question` added by @zanieb on 2024-10-15 04:52_

---

_Comment by @fraser-langton on 2024-10-15 05:04_

🤦‍♂️ahh thanks

---

_Closed by @fraser-langton on 2024-10-15 05:04_

---

_Comment by @zanieb on 2024-10-15 05:05_

We'll add a dedicated warning for this soon. Sorry, I know it's surprising.

---

_Comment by @fraser-langton on 2024-10-15 06:55_

I'm just used to pipenv. 

---
