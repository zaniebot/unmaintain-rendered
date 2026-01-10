```yaml
number: 1327
title: Guard against deletion of the current environment
type: issue
state: closed
author: zanieb
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-15T19:10:52Z
updated_at: 2025-01-22T13:28:30Z
url: https://github.com/astral-sh/uv/issues/1327
synced_at: 2026-01-10T04:27:57Z
```

# Guard against deletion of the current environment

---

_Issue opened by @zanieb on 2024-02-15 19:10_

For example, `uv venv` will happily clear the current virtual environment.

This is most notably an issue when `uv` is installed in that environment.

---

_Comment by @MichaReiser on 2024-02-15 19:20_

This also an issue on Windows where the operation fails. 


```
uv venv
Using Python 3.12.2 interpreter at C:\Users\Micha\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: .venv
uv::venv::creation

  × Failed to create virtualenv
  ├─▶ failed to remove directory `.venv`
  ╰─▶ Access is denied. (os error 5)
```

---

_Label `bug` added by @charliermarsh on 2024-02-15 19:21_

---

_Label `windows` added by @charliermarsh on 2024-02-15 19:21_

---

_Comment by @charliermarsh on 2024-03-04 14:48_

@gaborbernat -- just curious, do `virtualenv` do anything special here (e.g., if you try to create a virtualenv with `--clear`, and the `virtualenv.exe` you're running is in the directory that needs to be removed)?

---

_Comment by @gaborbernat on 2024-03-04 15:55_

Nothing at all: https://github.com/pypa/virtualenv/blob/5cd543f/src/virtualenv/create/creator.py#L155-L157

---

_Comment by @charliermarsh on 2024-03-04 15:56_

Thanks!

---

_Comment by @bersbersbers on 2024-10-14 19:26_

Here's another/similar issue regarding `uv` trying to delete the current environment. I think the main issue is the mismatch between some leftover `.python-version` that I had and the (new) `.venv`. But ideally, `uv` would be smart enough to detect that it is currently running from said `.venv`, and should not try deleting it.

`bug.bat`:
```
@echo off
if not exist c:\ws\bug mkdir c:\ws\bug
pushd c:\ws\bug

if exist .venv rmdir .venv /s/q
echo 3.12.3 > .python-version
echo [project] > pyproject.toml
echo name = "Bug" >> pyproject.toml
echo version = "1" >> pyproject.toml

python -m venv .venv
call .venv\scripts\activate

pip install uv
uv sync --verbose

pause
```

Output:
```
Collecting uv
  Using cached uv-0.4.20-py3-none-win_amd64.whl.metadata (11 kB)
Using cached uv-0.4.20-py3-none-win_amd64.whl (14.2 MB)
Installing collected packages: uv
Successfully installed uv-0.4.20
DEBUG uv 0.4.20
DEBUG Found project root: `c:\ws\bug`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `c:\ws\bug\.python-version`
DEBUG The virtual environment's Python version does not satisfy `Python 3.12.3`
DEBUG Searching for Python 3.12.3 in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\bers\AppData\Roaming\uv\python`
DEBUG Found managed installation `cpython-3.12.3-windows-x86_64-none`
DEBUG Found `cpython-3.12.3-windows-x86_64-none` at `C:\Users\bers\AppData\Roaming\uv\python\cpython-3.12.3-windows-x86_64-none\python.exe` (managed installations)
Using CPython 3.12.3
error: failed to remove directory `c:\ws\bug\.venv`
  Caused by: Access is denied. (os error 5)
Press any key to continue . . .
```

---

_Comment by @arden13 on 2024-12-17 13:13_

Hey All,

I've been testing out uv for my work and am hitting what seems a related issue to this one. Running on a windows 11 machine I'm generating some base python environments and I hit this same error (os error 5) when running

`uv venv py311 --native-tls --python 3.11`

running this one time it fails. Running it a second time it succeeds.
```
(nipals_uv) PS C:\Users\username\git_repos\open_nipals> uv venv py311 --native-tls --python 3.11
  × Failed to copy to:
  │ C:\Users\username\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none
  ╰─▶ failed to rename file from C:\Users\username\AppData\Roaming\uv\python\.temp\.tmpu6Kxca\python
      to C:\Users\username\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none: Access    
      is denied. (os error 5)
(nipals_uv) PS C:\Users\username\git_repos\open_nipals> uv venv py311 --native-tls --python 3.11
Using CPython 3.11.11
Creating virtual environment at: py311
Activate with: py311\Scripts\activate
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 18:17_

---

_Closed by @charliermarsh on 2024-12-29 15:46_

---

_Closed by @charliermarsh on 2024-12-29 15:46_

---

_Comment by @i-esin on 2025-01-22 11:11_

Hi all,

I am having the similar issue. Could this be due to an antivirus?

```
$ uv add open-interpreter[local]
Resolved 184 packages in 482ms
  × Failed to download `torch==2.3.1`
  ├─▶ Failed to read from the distribution cache
  ╰─▶ failed to rename file from C:\Users\iesin\AppData\Local\uv\cache\.tmpr85ZTV to C:\Users\iesin\AppData\Local\uv\cache\archive-v0\7jAiiCuwwfNRLg1bifo55: Access is denied. (os error 5)
```

---

_Comment by @charliermarsh on 2025-01-22 13:28_

Yes, most likely.

---
