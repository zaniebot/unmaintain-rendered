---
number: 12743
title: uv python cannot find PYTHONHOME when symlinked from inside of directory with pyvenv.cfg with wrong home value
type: issue
state: closed
author: hoodmane
labels:
  - bug
  - duplicate
assignees: []
created_at: 2025-04-08T13:38:17Z
updated_at: 2025-04-08T19:10:58Z
url: https://github.com/astral-sh/uv/issues/12743
synced_at: 2026-01-07T13:12:18-06:00
---

# uv python cannot find PYTHONHOME when symlinked from inside of directory with pyvenv.cfg with wrong home value

---

_Issue opened by @hoodmane on 2025-04-08 13:38_

### Summary

I'm sorry this is a bit of a weird bug report. I swear it has a real use case, see https://github.com/pyodide/pyodide-build/issues/143.

```console
$ uv python install 3.12
$ echo "home = junk" > pyveng.cfg
$ ln -s /usr/bin/python3.12 system-python-symlink
$ ln -s $(uv python find python3.12) uv-python-symlink
$ ./system-python-symlink -c 'print("hi!")'
hi!
$ ./uv-python-symlink -c 'print("hi!")'
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = './uv-python-symlink'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = '/home/rchatham/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/home/rchatham/Documents/programming/tmp/uv-symlink-bug/uv-python-symlink'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python312.zip',
    '/install/lib/python3.12',
    '/install/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x0000745b37819740 (most recent call first):
  <no Python frame>
```


### Platform

Ubuntu 22

### Version

uv 0.5.23

### Python version

Either Python 3.12 or Python 3.13

---

_Label `bug` added by @hoodmane on 2025-04-08 13:38_

---

_Comment by @zanieb on 2025-04-08 17:33_

Duplicate of https://github.com/astral-sh/uv/issues/8821

Unfortunately this is a known problem.

Lots of discussion in https://github.com/astral-sh/python-build-standalone/issues/380 and https://github.com/python/cpython/issues/128670 too



---

_Label `duplicate` added by @zanieb on 2025-04-08 17:33_

---

_Comment by @hoodmane on 2025-04-08 19:10_

Thanks!

---

_Closed by @hoodmane on 2025-04-08 19:10_

---
