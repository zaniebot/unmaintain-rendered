---
number: 11927
title: Symlinking Python interpreter fails due to different filesystem
type: issue
state: closed
author: Snaptraks
labels:
  - bug
assignees: []
created_at: 2025-03-03T16:06:40Z
updated_at: 2025-03-18T19:20:40Z
url: https://github.com/astral-sh/uv/issues/11927
synced_at: 2026-01-07T13:12:18-06:00
---

# Symlinking Python interpreter fails due to different filesystem

---

_Issue opened by @Snaptraks on 2025-03-03 16:06_

### Summary

When creating a new virtual environment in a directory that is inside a different filesystem than the Python interpreter, ``uv sync`` fails even with link mode set as copy.

The way our infrastructure is set, the directory ``smb_share/`` inside the ``/home/redacted/`` is actually a CIFS mount to a Windows based file system. Creating the virtual environment fails at the linking of the Python interpreter, since the executable is inside the "main" file system.

How would I go about installing the Python executable either inside the project's ``.venv``, or on the same file system? 

The verbose debug log when syncing the environment, if it helps:

```
> uv sync -v --link-mode copy
DEBUG uv 0.6.3
DEBUG Found project root: `/home/redacted/smb_share/TeamMembers/redacted/uv_project`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/redacted/smb_share/TeamMembers/redacted/uv_project`
DEBUG Reading Python requests from version file at `/home/francoishardy/redacted/TeamMembers/redacted/uv_project/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/home/redacted/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.11.11-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.9.21-linux-x86_64-gnu`
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/redacted/miniforge3/bin/python3.12` (first executable in the search path)
Using CPython 3.12.4 interpreter at: /home/redacted/miniforge3/bin/python3.12
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /home/redacted/miniforge3/bin/python3.12
DEBUG Released lock at `/tmp/uv-01e8608fdf1bc070.lock`
error: Operation not supported (os error 95)
```

### Platform

Ubuntu 22.04.5 LTS x86_64

### Version

uv 0.6.3

### Python version

Python 3.12.4

---

_Label `bug` added by @Snaptraks on 2025-03-03 16:06_

---

_Comment by @zanieb on 2025-03-03 17:53_

> Creating the virtual environment fails at the linking of the Python interpreter, since the executable is inside the "main" file system.

How are you determining this? We shouldn't be hard-linking the Python interpreter, we always use symlinks on Unix

https://github.com/astral-sh/uv/blob/53d1a7aa6e6ec4cadf0272218ea7d36ef9bc9f5a/crates/uv-virtualenv/src/virtualenv.rs#L160-L184

---

_Comment by @Snaptraks on 2025-03-03 18:58_

A symlink across such file systems (the host with the usual ext4 file system) and the CIFS one (under the ``smb_share/`` directory in my example) returns the same OS Error 95, operation not supported. 

```py
Python 3.12.4 | (main, Jun 17 2024, 10:23:07) [GCC 12.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from pathlib import Path
>>> Path("/home/redacted/smb_share/TeamMembers/redacted/uv_project/test.txt").symlink_to(Path("/home/redacted/test.txt"))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/redacted/miniforge3/lib/python3.12/pathlib.py", line 1386, in symlink_to
    os.symlink(target, self, target_is_directory)
OSError: [Errno 95] Operation not supported: '/home/francoishardy/test.txt' -> '/home/redacted/smb_share/TeamMembers/redacted/uv_project/test.txt'
```

I figured the issue was with the Python symlink, since I get the exact same error when running ``uv venv --python 3.12 -v`` to change the Python version inside the virtual environment.

```
> uv venv --python 3.12 -v
DEBUG uv 0.6.3
DEBUG Found project root: `/home/redacted/smb_share/TeamMembers/redacted/uv_project`
DEBUG No workspace root found, using project root
DEBUG Using Python request `3.12` from explicit request
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/home/redacted/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.9-linux-x86_64-gnu`
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/redacted/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12` (managed installations)
Using CPython 3.12.9
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /home/redacted/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Using base executable for virtual environment: /home/redacted/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
uv::venv::creation

  × Failed to create virtualenv
  ╰─▶ Operation not supported (os error 95)
```

---

_Comment by @charliermarsh on 2025-03-03 19:01_

Does `python -m venv` work in that environment? That also uses symlinks, as is standard.

---

_Comment by @Snaptraks on 2025-03-03 19:05_

> Does `python -m venv` work in that environment? That also uses symlinks, as is standard.

Sadly no, would it make it not be supported then?
```
> python -m venv .venv
Error: [Errno 95] Operation not supported: 'lib' -> '/home/redacted/smb_share/TeamMembers/redacted/uv_project/.venv/lib64'
```

---

_Comment by @charliermarsh on 2025-03-03 19:06_

Does `python -m venv --copies .venv` work?

---

_Comment by @Snaptraks on 2025-03-03 19:10_

> Does `python -m venv --copies .venv` work?

Same error once again
```
> python -m venv --copies .venv
Error: [Errno 95] Operation not supported: 'lib' -> '/home/redacted/smb_share/TeamMembers/redacted/uv_project/.venv/lib64'
```

---

_Comment by @zanieb on 2025-03-03 19:44_

It sounds like you need https://github.com/astral-sh/uv/issues/7865

---

_Comment by @charliermarsh on 2025-03-03 20:17_

I don't think that's sufficient (it might be necessary though). I think the error they're seeing here is that the distro symlinks `lib` to `lib64` (`purelib` and `platlib`, or something -- can't remember the details).

---

_Renamed from "Hardlinking Python interpreter fails due to different filesystem" to "Symlinking Python interpreter fails due to different filesystem" by @Snaptraks on 2025-03-04 14:26_

---

_Comment by @Snaptraks on 2025-03-04 14:31_

Specifying the Python isntallation directory did not seem to fix the problem (or this is a different problem). I set the installation directory as one inside the ``smb_share`` / CIFS file system.

```
> uv python install -i /home/redacted/smb_share/TeamMembers/redacted/pycache/ --no-cache -v 3.12
DEBUG uv 0.6.3
DEBUG Reading Python requests from version file at `/home/redacted/smb_share/TeamMembers/redacted/uv_project/.python-version`
DEBUG Acquired lock for `/home/redacted/smb_share/TeamMembers/redacted/pycache/`
DEBUG No installation found for request `Python 3.12`
DEBUG Found download `cpython-3.12.9-linux-x86_64-gnu` for request `Python 3.12`
DEBUG Using request timeout of 30s
Downloading cpython-3.12.9-linux-x86_64-gnu (20.3MiB)
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250212/cpython-3.12.9%2B20250212-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/redacted/smb_share/TeamMembers/redacted/pycache/.temp/.tmpMcMLar
DEBUG Extracting cpython-3.12.9%2B20250212-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
error: Failed to install cpython-3.12.9-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.12.9%2B20250212-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: failed to unpack `/home/redacted/smb_share/TeamMembers/redacted/pycache/.temp/.tmpMcMLar/python/bin/2to3`
  Caused by: Operation not supported (os error 95)
DEBUG Released lock at `/home/redacted/smb_share/TeamMembers/redacted/pycache/.lock`
```

---

_Comment by @zanieb on 2025-03-04 14:34_

There are symlinks in the Python installation itself, yeah, e.g.

```
❯ ls -lah /Users/zb/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin
total 152
drwxr-xr-x  14 zb  staff   448B Feb 24 21:28 .
drwxr-xr-x   6 zb  staff   192B Feb 24 21:28 ..
lrwxr-xr-x   1 zb  staff     8B Feb 24 21:28 idle3 -> idle3.13
-rwxr-xr-x   1 zb  staff   156B Feb 24 21:28 idle3.13
-rwxr-xr-x   1 zb  staff   286B Feb 24 21:28 pip
-rwxr-xr-x   1 zb  staff   286B Feb 24 21:28 pip3
-rwxr-xr-x   1 zb  staff   286B Feb 24 21:28 pip3.13
lrwxr-xr-x   1 zb  staff     9B Feb 24 21:28 pydoc3 -> pydoc3.13
-rwxr-xr-x   1 zb  staff   141B Feb 24 21:28 pydoc3.13
lrwxr-xr-x   1 zb  staff    10B Feb 24 21:28 python -> python3.13
lrwxr-xr-x   1 zb  staff    10B Feb 24 21:28 python3 -> python3.13
lrwxr-xr-x   1 zb  staff    17B Feb 24 21:28 python3-config -> python3.13-config
-rwxr-xr-x   1 zb  staff    49K Feb 24 21:28 python3.13
-rwxr-xr-x   1 zb  staff   2.1K Feb 24 21:28 python3.13-config
```

Unfortunately this seems painful to fix. I guess we'd need to add opt-in copy fallback to the tar crate?

---

_Comment by @Snaptraks on 2025-03-04 14:38_

I understand my specific usecase is very uncommon, if this is out of scope of the project I would understand and advise to move the project outside of the CIFS file system.

---

_Comment by @zanieb on 2025-03-18 19:20_

I think we'll consider this out of scope for now. Thanks for understanding.

---

_Closed by @zanieb on 2025-03-18 19:20_

---

_Referenced in [astral-sh/uv#13302](../../astral-sh/uv/issues/13302.md) on 2025-05-05 15:27_

---
