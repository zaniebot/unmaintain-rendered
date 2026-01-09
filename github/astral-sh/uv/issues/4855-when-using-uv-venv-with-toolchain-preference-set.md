---
number: 4855
title: "When using ``uv venv`` with ``toolchain-preference`` set to ``managed``, it will fetch the wrong platform on ``uv 0.2.21``."
type: issue
state: closed
author: FishAlchemist
labels:
  - duplicate
assignees: []
created_at: 2024-07-07T11:21:51Z
updated_at: 2024-07-07T17:06:03Z
url: https://github.com/astral-sh/uv/issues/4855
synced_at: 2026-01-07T13:12:17-06:00
---

# When using ``uv venv`` with ``toolchain-preference`` set to ``managed``, it will fetch the wrong platform on ``uv 0.2.21``.

---

_Issue opened by @FishAlchemist on 2024-07-07 11:21_

**OS:** Windows 11 23H2
**UV Version:** uv 0.2.21 (ebfe6d8fc 2024-07-03)
### Command 
```powershell
 uv venv --toolchain-preference managed -p 3.10.14 --verbose
```
### Verbose Output
```powershell
DEBUG uv 0.2.21
DEBUG Acquired lock for `C:\Users\user_name\AppData\Roaming\uv\data\toolchains`
DEBUG Using request timeout of 30s
INFO Fetching requested toolchain...
  × Python interpreter not found at `C:\Users\user_name\AppData\Roaming\uv\data\toolchains\cpython-3.10.14-macos-aarch64-none\install\python.exe`
  ```
### toolchain list
```powershell
warning: `uv toolchain list` is experimental and may change without warning.
cpython-3.12.3-windows-x86_64-none      <download available>
cpython-3.11.9-windows-x86_64-none      C:\Users\user_name\AppData\Local\Programs\Python\Python311\python.exe
cpython-3.11.9-windows-x86_64-none      <download available>
cpython-3.10.14-windows-x86_64-none     C:\Users\user_name\AppData\Roaming\uv\data\toolchains\cpython-3.10.14-windows-x86_64-none\install\python.exe
cpython-3.10.11-windows-x86_64-none     C:\Users\user_name\AppData\Local\Programs\Python\Python310\python.exe
cpython-3.9.19-windows-x86_64-none      <download available>
cpython-3.8.19-windows-x86_64-none      <download available>
cpython-3.7.9-windows-x86_64-none       <download available>
```
If ``toolchain-preference`` set to ``installed``, it will run successfully.


---

_Renamed from "When using ``uv venv`` with ``toolchain-preference`` set to ``managed``, it will fetch the wrong platform." to "When using ``uv venv`` with ``toolchain-preference`` set to ``managed``, it will fetch the wrong platform on ``uv 0.2.21``." by @FishAlchemist on 2024-07-07 11:32_

---

_Comment by @zanieb on 2024-07-07 16:50_

I think we fixed this over in https://github.com/astral-sh/uv/pull/4810 and it'll be out in the next release.

Duplicate of https://github.com/astral-sh/uv/issues/4800


---

_Closed by @zanieb on 2024-07-07 16:50_

---

_Comment by @FishAlchemist on 2024-07-07 17:04_

> I think we fixed this over in #4810 and it'll be out in the next release.
> 
> Duplicate of #4800

I just tested the issue after building it from source, and the issue is indeed fixed.
I was busy with other things today, so I didn't test the issue by building it from source code. Instead, I just newed the issue without thinking that it might already be fixed.

---

_Comment by @zanieb on 2024-07-07 17:05_

No problem. Thanks for confirming it's fixed, I appreciate it!

---

_Label `duplicate` added by @zanieb on 2024-07-07 17:06_

---
