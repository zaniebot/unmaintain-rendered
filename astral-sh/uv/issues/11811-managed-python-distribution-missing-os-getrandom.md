---
number: 11811
title: "Managed Python distribution missing `os.getrandom`"
type: issue
state: open
author: wbilal-c
labels:
  - bug
assignees: []
created_at: 2025-02-26T21:22:56Z
updated_at: 2025-02-26T21:46:51Z
url: https://github.com/astral-sh/uv/issues/11811
synced_at: 2026-01-10T01:25:11Z
---

# Managed Python distribution missing `os.getrandom`

---

_Issue opened by @wbilal-c on 2025-02-26 21:22_

### Summary

My actual system Python is 3.13.2. The Python environment I'm trying to install in my virtual environment is Python 3.12 with uv. 

I've followed these steps:
```zsh
uv python install "python3.12"
uv venv "virtualenv_dir" --python "3.12" --link-mode copy   
source virtualenv_dir/bin/activate 
```
This is my `pyvenv.vfg` file:
```zsh
❯ cat pyvenv.cfg
home = /home/name/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin
implementation = CPython
uv = 0.6.2
version_info = 3.12.9
include-system-site-packages = false
```

Then when trying to see if `os.getrandom` is working in my activated virtual env:
```zsh
❯ python
Python 3.12.9 (main, Feb  5 2025, 19:10:45) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.getrandom(256,1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'os' has no attribute 'getrandom'. Did you mean: 'urandom'?
>>> 
```
Checking `HAVE_GETRANDOM` as well:
```zsh
❯ python
Python 3.12.9 (main, Feb  5 2025, 19:10:45) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sysconfig
>>> sysconfig.get_config_vars("HAVE_GETRANDOM")
[0]
```
Is there a way to enable this option with uv?

Possible similar/related issue: https://github.com/astral-sh/uv/issues/8429

### Platform

6.12.10-arch1-1

### Version

uv 0.6.2

### Python version

Python 3.13.2

---

_Label `bug` added by @wbilal-c on 2025-02-26 21:22_

---

_Comment by @zanieb on 2025-02-26 21:46_

Looks like a duplicate of https://github.com/astral-sh/python-build-standalone/issues/193

---

_Renamed from "Creating virtual env with uv on Arch Linux" to "Managed Python distribution missing `os.getrandom`" by @zanieb on 2025-02-26 21:46_

---
