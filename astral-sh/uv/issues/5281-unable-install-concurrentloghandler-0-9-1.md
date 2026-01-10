---
number: 5281
title: unable install ConcurrentLogHandler==0.9.1
type: issue
state: closed
author: Funnor
labels: []
assignees: []
created_at: 2024-07-22T09:36:05Z
updated_at: 2024-07-22T14:00:31Z
url: https://github.com/astral-sh/uv/issues/5281
synced_at: 2026-01-10T01:23:47Z
---

# unable install ConcurrentLogHandler==0.9.1

---

_Issue opened by @Funnor on 2024-07-22 09:36_

UV version: uv 0.2.27 
Operating System: Ubuntu 20.04.3

verbose output

(venv) root@p87781v:/# uv pip install ConcurrentLogHandler==0.9.1 --verbose
DEBUG uv 0.2.27
DEBUG Searching for Python interpreter in system path
DEBUG Found cpython 3.8.10 at `/opt/venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.8.10 environment at opt/venv/bin/python3
DEBUG Acquired lock for `opt/venv`
DEBUG At least one requirement is not satisfied: concurrentloghandler==0.9.1
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.8.10
DEBUG Adding direct dependency: concurrentloghandler==0.9.1
DEBUG Found fresh response for: https://pypi.org/simple/concurrentloghandler/
DEBUG Searching for a compatible version of concurrentloghandler (==0.9.1)
DEBUG Selecting: concurrentloghandler==0.9.1 [compatible] (ConcurrentLogHandler-0.9.1.tar.gz)
DEBUG Acquired lock for `/root/.cache/uv/built-wheels-v3/pypi/concurrentloghandler/0.9.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fd/e5/0dc4f256bcc6484d454006b02f33263b20f762a433741b29d53875e0d763/ConcurrentLogHandler-0.9.1.tar.gz
DEBUG Preparing metadata for: concurrentloghandler==0.9.1
DEBUG No static `PKG-INFO` available for: concurrentloghandler==0.9.1 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG No static `pyproject.toml` available for: concurrentloghandler==0.9.1 (MissingPyprojectToml)
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.8.10
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==71.1.0 [compatible] (setuptools-71.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/51/a0/ee460cc54e68afcf33190d198299c9579a5eafeadef0016ae8563237ccb6/setuptools-71.1.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.004s
DEBUG Installing in setuptools==71.1.0 in /root/.cache/uv/builds-v0/.tmpnKtMiT
DEBUG Requirement already cached: setuptools==71.1.0
DEBUG Installing build requirement: setuptools==71.1.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
error: Failed to download and build `concurrentloghandler==0.9.1`
  Caused by: Failed to build: `concurrentloghandler==0.9.1`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
error in ConcurrentLogHandler setup command: use_2to3 is invalid.
---

---

_Comment by @charliermarsh on 2024-07-22 13:18_

It looks like the same error exists in pip:

```
❯ pip install ConcurrentLogHandler==0.9.1 --verbose
Using pip 24.0 from /Users/crmarsh/.local/share/rtx/installs/python/3.11.9/lib/python3.11/site-packages/pip (python 3.11)
Collecting ConcurrentLogHandler==0.9.1
  Downloading ConcurrentLogHandler-0.9.1.tar.gz (25 kB)
  Running command pip subprocess to install build dependencies
  Collecting setuptools>=40.8.0
    Downloading setuptools-71.1.0-py3-none-any.whl.metadata (6.6 kB)
  Downloading setuptools-71.1.0-py3-none-any.whl (2.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.3/2.3 MB 36.5 MB/s eta 0:00:00
  Installing collected packages: setuptools
  Successfully installed setuptools-71.1.0

  [notice] A new release of pip is available: 24.0 -> 24.1.2
  [notice] To update, run: pip install --upgrade pip
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  error in ConcurrentLogHandler setup command: use_2to3 is invalid.
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> See above for output.
```

I suggest filing an issue with the package, as this is a packaging issue, not a uv issue. Sorry!

---

_Closed by @charliermarsh on 2024-07-22 13:18_

---

_Comment by @FishAlchemist on 2024-07-22 13:24_

I took a look at the setup.py file for ConcurrentLogHandler 0.9.1 and noticed that it uses ``use_2to3=True``. Support for 2to3 was removed from setuptools in version 58.0.0 and will **fail fast** in 58.0.2.
https://setuptools.pypa.io/en/latest/history.html#v58-0-0
I checked the ConcurrentLogHandler website and it mentioned another repository.
![image](https://github.com/user-attachments/assets/844c978a-571d-4421-a9f7-41e791c6aeb5)
**Note:** I am not familiar with the code for UV. I have only made changes to the documentation.

---
