---
number: 13544
title: Calling venv python via exec with process name substitution fails
type: issue
state: open
author: mxwell-dev
labels:
  - bug
assignees: []
created_at: 2025-05-19T20:51:18Z
updated_at: 2025-05-20T14:13:13Z
url: https://github.com/astral-sh/uv/issues/13544
synced_at: 2026-01-10T01:25:34Z
---

# Calling venv python via exec with process name substitution fails

---

_Issue opened by @mxwell-dev on 2025-05-19 20:51_

### Summary

I'm transitioning from anaconda to UV / virtualenvs and am encountering a weird issue when calling a module directly with python from the virtual environment created by UV. I'm launching with a wrapper script that eventually calls:

`PYTHONPATH=/local/lib exec -a myname /home/johndoe/.virtualenvs/myenv-3.12/bin/python -m mymodule`

which produces an error similar to those mentioned in issues referenced in #8429, but I am unsure if the root-cause is related. The issue does *not* appear when the program is not renamed (i.e. when `-a myname` is omitted, and weirdly also when using `-a test` it also works, but for no other name).

```
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = '/local/libs'
  program name = 'myname'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = ''
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = ''
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/local/libs',
    '/install/lib/python312.zip',
    '/install/lib/python3.12',
    '/install/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x000078a3391f6740 (most recent call first):
  <no Python frame>
```

### Platform

Linux 6.11.0-25-generic x86_64 GNU/Linux (Ubuntu 24.10)

### Version

0.7.5

### Python version

3.12, 3.9

---

_Label `bug` added by @mxwell-dev on 2025-05-19 20:51_

---

_Comment by @konstin on 2025-05-20 10:53_

It seems that our Pythons lose their `PYTHONHOME` when run through `exec`

CC @geofft 

---

_Assigned to @geofft by @geofft on 2025-05-20 14:13_

---
