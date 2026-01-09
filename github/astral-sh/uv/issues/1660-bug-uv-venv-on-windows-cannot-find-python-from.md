---
number: 1660
title: "Bug: `uv venv` on windows cannot find python from pyenv"
type: issue
state: closed
author: Warchant
labels:
  - bug
  - windows
  - virtualenv
assignees: []
created_at: 2024-02-18T17:51:35Z
updated_at: 2024-02-22T07:47:35Z
url: https://github.com/astral-sh/uv/issues/1660
synced_at: 2026-01-07T13:12:16-06:00
---

# Bug: `uv venv` on windows cannot find python from pyenv

---

_Issue opened by @Warchant on 2024-02-18 17:51_

```powershell
(latest windows 11)
$ pyenv version
* 3.10.8 (set by C:\Users\warch\.pyenv\pyenv-win\version)
$ python --version
Python 3.10.8
$ pipx install uv
$ uv --version
uv 0.1.4
```

On Windows I use pyenv to manage python versions. Currently 3.10.8 is installed. And I never installed python via downloadable installer.

On clean windows `py` and `python` point to a shim that Windows created at `C:\Users\warch\AppData\Local\Microsoft\WindowsApps\python.exe` - that thing is not a real python - if you run `python` or `py` Windows opens Microsoft Store, and will propose to install python from the Store.

Bug description: 
```powershell
$ uv venv venv2
  x failed to canonicalize path `C:\Users\warch\AppData\Local\Microsoft\WindowsApps\python.exe`
  `-> The file cannot be accessed by the system. (os error 1920)
$ rm C:\Users\warch\AppData\Local\Microsoft\WindowsApps\python.exe
$ uv venv venv2
  x Failed to run `py --list-paths` to find Python installations. Is Python installed?
  `-> program not found
$ py
py : The term 'py' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a
path was included, verify that the path is correct and try again.
At line:1 char:1
+ py
+ ~~
    + CategoryInfo          : ObjectNotFound: (py:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

$ uv venv venv2 -p python
  x Querying Python at `C:\Users\warch\.pyenv\pyenv-win\shims\python.bat` failed with status exit code: 1:
  | --- stdout:

  | --- stderr:
  | File "<string>", line 1
  |     import json ||  goto :error
  |                 ^
  | SyntaxError: invalid syntax
  | ---

```

On Windows `uv venv` uses `py --list-versions` to get python, and I think `uv` could do more checks to ensure `py` exists, to check if `pyenv` is installed, etc:

(very rough python implementation of possible checks):
```python
def get_python_path_windows():
  # try to use `py` to find `python`
  py = shutil.which("py")
  if py and check_if_valid_win32_app(py):
    return get_python_from_py(py)

  # maybe `python` exists in PATH?
  python = shutil.which("python")
  if python and check_if_valid_win32_app(python):
    # check_if_valid_win32_app would return False on `python.bat` created by pyenv
    return python
  
  # check if pyenv is installed
  pyenv = shutil.which("pyenv")
  if pyenv:
    return get_python_from_pyenv(pyenv)
  
  return None


def get_python_from_pyenv(pyenv):
  # pyenv which python returns path to python.exe
  return subprocess.check_output([pyenv, "which", "python"]).strip().decode("utf-8")
```

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Label `bug` added by @zanieb on 2024-02-18 20:59_

---

_Comment by @zanieb on 2024-02-18 21:00_

Thanks for the report. We have some bugs around our handling of `pyenv` shims right now. Windows is especially complicated for Python version discovery :(

---

_Label `windows` added by @zanieb on 2024-02-18 21:00_

---

_Comment by @ofek on 2024-02-18 23:13_

https://github.com/astral-sh/uv/issues/1310

---

_Referenced in [astral-sh/uv#1711](../../astral-sh/uv/pulls/1711.md) on 2024-02-20 14:27_

---

_Closed by @MichaReiser on 2024-02-22 07:47_

---
