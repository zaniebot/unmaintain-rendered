```yaml
number: 15334
title: "Python installed via uv doesn't work with tox"
type: issue
state: open
author: canepan
labels:
  - bug
assignees: []
created_at: 2025-08-17T22:36:17Z
updated_at: 2025-09-04T18:37:25Z
url: https://github.com/astral-sh/uv/issues/15334
synced_at: 2026-01-10T03:23:54Z
```

# Python installed via uv doesn't work with tox

---

_Issue opened by @canepan on 2025-08-17 22:36_

### Summary

Kernel: 6.12.10-76061203-generic

Repo: https://github.com/canepan/bot

Commands to reproduce (on a system without Python 3.11):
```
uv python install cpython3.11
uvx tox
```
Expecting tox to run correctly, but instead the following is shown for py311 (py310 from OS works OK):
```
py311: install_deps> python -I -m pip install 'flake8<5.0.0' 'importlib-metadata<5.0' pytest-black 'pytest-cov<6.0' 'pytest-flake8; python_version < "3.12"' pytest-mypy pytest-pudb 'pytest<7' setuptools types-mock
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = '/home/nicola/Documenti/dev/tools/.tox/py311/bin/python'
  isolated = 1
  environment = 0
  user site = 0
  safe_path = 1
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.11'
  sys._base_executable = '/home/nicola/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/bin/python3.11'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/home/nicola/Documenti/dev/tools/.tox/py311/bin/python'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python311.zip',
    '/install/lib/python3.11',
    '/install/lib/python3.11/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x00007df16616d740 (most recent call first):
  <no Python frame>
py311: exit 1 (0.02 seconds) /home/nicola/Documenti/dev/tools> python -I -m pip install 'flake8<5.0.0' 'importlib-metadata<5.0' pytest-black 'pytest-cov<6.0' 'pytest-flake8; python_version < "3.12"' pytest-mypy pytest-pudb 'pytest<7' setuptools types-mock pid=2468762
  py39: SKIP (0.04 seconds)
  py310: OK (10.06=setup[4.41]+cmd[5.65] seconds)
  py311: FAIL code 1 (0.02 seconds)
  evaluation failed :( (10.17 seconds)
```

### Platform

Pop!_OS Linux x86_64 22.04 LTS

### Version

uv 0.8.11

### Python version

Python 3.11.13

---

_Label `bug` added by @canepan on 2025-08-17 22:36_

---

_Comment by @zsol on 2025-08-19 16:17_

This is typically a sign of the CPython installation being completely broken. Does `uv run --no-project -p 3.11 python` work? If not, does `uv python install cpython3.11 --reinstall` help?

FWIW I can't reproduce the error on a Pop!_OS virtual machine.

---

_Comment by @canepan on 2025-08-20 19:51_

It works:
```
nicola@mobile-fun:~ $ uv run --no-project -p 3.11 python
Python 3.11.13 (main, Aug 14 2025, 16:25:01) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

---

_Comment by @canepan on 2025-08-20 21:19_

Something odd (from within the project):
```
$ /home/nicola/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/bin/python3.11 -m ensurepip
error: externally-managed-environment

× This environment is externally managed
╰─> This Python installation is managed by uv and should not be modified.

```

while:
```
$ uv run --no-project -p 3.11 python -m ensurepip
Looking in links: /tmp/tmpgfmq0lj5
Processing /tmp/tmpgfmq0lj5/setuptools-65.5.0-py3-none-any.whl
Processing /tmp/tmpgfmq0lj5/pip-24.0-py3-none-any.whl
Installing collected packages: setuptools, pip
Successfully installed pip-24.0 setuptools-65.5.0
```

This second command uses `./.venv/bin/python`, and after running the above `uvx tox -r` works and resolves the issue.
It looks like the only problem was the missing `pip` in the local `.venv`

---

_Comment by @konstin on 2025-08-26 08:39_

The underlying problem is https://github.com/astral-sh/python-build-standalone/issues/380, which tox seems to run into. Does it work if you use https://github.com/tox-dev/tox-uv instead?

---

_Comment by @BeRT2me on 2025-09-04 18:37_

Can confirm that using `tox-uv` instead works. 

---
