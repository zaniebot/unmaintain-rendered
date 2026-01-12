```yaml
number: 15770
title: Cannot install Pyperclip with uv
type: issue
state: open
author: cleb98
labels:
  - needs-mre
assignees: []
created_at: 2025-09-10T17:33:49Z
updated_at: 2025-09-14T12:56:26Z
url: https://github.com/astral-sh/uv/issues/15770
synced_at: 2026-01-12T16:02:17Z
```

# Cannot install Pyperclip with uv

---

_@cleb98_

### Summary

### Description

On Windows 10 I can successfully create venvs and install most packages (e.g. `pandas`).  
However, when I try to install `pyperclip` or package that has it as dependecy (tested with version 1.9.0), the installation fails with an **"Access is denied (os error 5)"** error while trying to read the cached wheel file in `builds-v0`.

The strange part is that:
- In one venv I was able to install `pyperclip` without any problem.
- In all other new venvs, I consistently get the same error for `pyperclip` only, while other packages (like `pandas`) work fine.
- I also tried uninstalling and reinstalling `uv`, but the behavior remains the same.

---

### Steps to reproduce

```bash
# create a new venv
uv venv venv2
source venv2/Scripts/activate   # or .\venv2\Scripts\activate on PowerShell

# try to install pyperclip
## current ouput
Cristian.BELLUCCI@AK74752 MINGW64 ~/Desktop/python_exp/venv-test/venv2
$ uv pip install pyperclip
Resolved 1 package in 49ms
  x Failed to download and build `pyperclip==1.9.0`
  |-> Failed to read from the distribution cache
  `-> failed to open file
      `C:\Users\Cristian.BELLUCCI\AppData\Local\uv\cache\builds-v0\.tmpYgNgHP\py
perclip-1.9.0-py3-none-any.whl`:
      Access is denied. (os error 5)
```
same behaviour using
```bash
 uv sync 
```
with a pyproject.toml:
``` pyproject.toml:

[project]
name = "venv2"
version = "0.1.0"
description = "Ambiente con solo pyperclip"
requires-python = ">=3.12,<3.13"

dependencies = [
    "pyperclip==1.9.0"
]
```

### Additional notes 
- The issue seems isolated to `pyperclip`; other packages install fine. 
- I verified file permissions in `C:\Users\<username>\AppData\Local\uv\cache\builds-v0`.
-  One venv created earlier can still install `pyperclip`, but any new venv fails. (precisely is the unique venv where pyperclip is instaled without any problem)
-  Antivirus or Controlled Folder Access might not be the cause (other wheels are read without problem). 
-  Tried clearing/recreating the cache and reinstalling `uv`, but the error persists.”

### OS
Windows 10

### Platform

MINGW64_NT-10.0-19045 3.6.3-7674c51e.x86_64 x86_64 Msys

### Version

uv 0.8.16 (2de677b0d 2025-09-09)

### Python version

Python 3.12.9

---

_Label `bug` added by @cleb98 on 2025-09-10 17:33_

---

_Label `bug` removed by @konstin on 2025-09-14 12:55_

---

_Label `needs-mre` added by @konstin on 2025-09-14 12:55_

---

_Comment by @konstin on 2025-09-14 12:56_

I can't reproduce this on Windows 11, are you sure this isn't a permission problem, e.g. from mixing admin and regular terminal, or some overzealous security software?

---
