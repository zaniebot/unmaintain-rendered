---
number: 2803
title: "Python 32 bit on windows overflowed on installing `greenlet`."
type: issue
state: closed
author: T-256
labels:
  - bug
assignees: []
created_at: 2024-04-03T11:47:27Z
updated_at: 2024-05-19T21:15:44Z
url: https://github.com/astral-sh/uv/issues/2803
synced_at: 2026-01-10T01:23:22Z
---

# Python 32 bit on windows overflowed on installing `greenlet`.

---

_Issue opened by @T-256 on 2024-04-03 11:47_

```shell
>uv venv --python "C:\Python311x86\python.exe"
Using Python 3.11.9 interpreter at: C:\Python311x86\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate

>uv pip install playwright
Resolved 4 packages in 3.41s
Building greenlet==3.0.3
░░░░░░░░░░░░░░░░░░░░ [0/1] Fetching packages...
thread 'main' has overflowed its stack

>.venv\Scripts\python.exe --version
Python 3.11.9

>uv --version
uv 0.1.28 (471cb2bfd 2024-04-03)
```

Windows 11 (23H2) - Python 3.11.9 (32 bit)

---

_Label `bug` added by @konstin on 2024-04-03 14:41_

---

_Comment by @konstin on 2024-04-03 14:42_

Is there a specific reasons you are using 32-bit python (i hope i didn't miss the answer to that question in another thread)? As far as i can tell, there is no 32-bit build of windows 11

---

_Comment by @T-256 on 2024-04-03 17:49_

> As far as i can tell, there is no 32-bit build of windows 11

I'm using Python 32 bit on Windows 64 bit

---

_Comment by @zanieb on 2024-04-03 17:51_

Yeah but why not 64bit Python? :)

---

_Comment by @hmc-cs-mdrissi on 2024-04-03 17:55_

For a long time it was first choice if you went to python.org, https://discuss.python.org/t/consider-downgrading-windows-32-bit-from-tier-1-to-tier-2-or-tier-3-in-python-3-13/33719/4

I think that’s changed recently but until year ago many windows users just picked 32 bit without thinking about which one.

---

_Comment by @T-256 on 2024-04-03 18:18_

> Yeah but why not 64bit Python? :)


Because I'm accessing some external apps memory where it needs to be 32bit to be able to access.


---

_Comment by @zanieb on 2024-04-03 18:19_

Thanks for the context. We have problems with stack size on Windows... #941 

---

_Comment by @T-256 on 2024-05-19 20:48_

```shell
>uv pip install playwright
Resolved 4 packages in 4.84s
   Built greenlet==3.0.3
Downloaded 4 packages in 1m 48s
Installed 4 packages in 350ms
 + greenlet==3.0.3
 + playwright==1.44.0
 + pyee==11.1.0
 + typing-extensions==4.11.0

>uv --version
uv 0.1.44 (d417daad7 2024-05-14)
```

---

_Closed by @T-256 on 2024-05-19 20:48_

---

_Comment by @zanieb on 2024-05-19 21:15_

Thank you for following up!

---
