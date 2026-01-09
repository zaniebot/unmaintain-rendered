---
number: 5280
title: "``uv python list`` will also display the installed Python version as ``download available``."
type: issue
state: closed
author: FishAlchemist
labels: []
assignees: []
created_at: 2024-07-22T06:08:40Z
updated_at: 2024-07-22T14:18:59Z
url: https://github.com/astral-sh/uv/issues/5280
synced_at: 2026-01-07T13:12:17-06:00
---

# ``uv python list`` will also display the installed Python version as ``download available``.

---

_Issue opened by @FishAlchemist on 2024-07-22 06:08_

**UV version:** uv 0.2.27 (833097b93 2024-07-19)
**Operating System:** Windows 11 23H2

 ``cpython-3.11.9``  and ``cpython-3.12.4`` are installed and paths are shown, but also shows ``download available``.

**Note:**  It also encountered this issue after testing with commit fb091af9 on the main branch.
### verbose output 
```powershell
DEBUG uv 0.2.27
warning: `uv python list` is experimental and may change without warning
DEBUG Searching for Python interpreter in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\user_name\AppData\Roaming\uv\data\python`
DEBUG Found managed Python `cpython-3.10.14-windows-x86_64-none`
DEBUG Found cpython 3.10.14 at `C:\Users\user_name\AppData\Roaming\uv\data\python\cpython-3.10.14-windows-x86_64-none\install\python.exe` (managed installations)
DEBUG Found managed Python `cpython-3.9.19-windows-x86_64-none`
DEBUG Found cpython 3.9.19 at `C:\Users\user_name\AppData\Roaming\uv\data\python\cpython-3.9.19-windows-x86_64-none\python.exe` (managed installations)
DEBUG Found cpython 3.12.4 at `C:\Users\user_name\AppData\Local\Programs\Python\Python312\python.exe` (`py` launcher output)
DEBUG Found cpython 3.11.9 at `C:\Users\user_name\AppData\Local\Programs\Python\Python311\python.exe` (`py` launcher output)
DEBUG Found cpython 3.10.11 at `C:\Users\user_name\AppData\Local\Programs\Python\Python310\python.exe` (`py` launcher output)
cpython-3.12.4-windows-x86_64-none     C:\Users\user_name\AppData\Local\Programs\Python\Python312\python.exe
cpython-3.12.4-windows-x86_64-none     <download available>
cpython-3.11.9-windows-x86_64-none     C:\Users\user_name\AppData\Local\Programs\Python\Python311\python.exe
cpython-3.11.9-windows-x86_64-none     <download available>
cpython-3.10.14-windows-x86_64-none    C:\Users\user_name\AppData\Roaming\uv\data\python\cpython-3.10.14-windows-x86_64-none\install\python.exe
cpython-3.10.11-windows-x86_64-none    C:\Users\user_name\AppData\Local\Programs\Python\Python310\python.exe
cpython-3.9.19-windows-x86_64-none     C:\Users\user_name\AppData\Roaming\uv\data\python\cpython-3.9.19-windows-x86_64-none\python.exe
cpython-3.8.19-windows-x86_64-none     <download available>
cpython-3.7.9-windows-x86_64-none      <download available>
```

---

_Comment by @zanieb on 2024-07-22 14:13_

This is expected — we show everything on your system but also show which versions are available to download a managed distribution instead. Do you think we should not?

---

_Comment by @FishAlchemist on 2024-07-22 14:18_

> e show everything on your system but also show which versions are available to download a managed distribution instead. Do you think we should not?

I don't have an opinion on this behavior, just that he is repeatedly showing the same version, which I originally thought was a BUG.

---

_Closed by @FishAlchemist on 2024-07-22 14:18_

---
