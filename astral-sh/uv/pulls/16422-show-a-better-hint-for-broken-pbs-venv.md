```yaml
number: 16422
title: " Show a better hint for broken PBS venv"
type: pull_request
state: open
author: konstin
labels:
  - error messages
assignees: []
base: main
head: konsti/hint-at-broken-interpreter
created_at: 2025-10-23T15:40:40Z
updated_at: 2025-12-11T15:42:31Z
url: https://github.com/astral-sh/uv/pull/16422
synced_at: 2026-01-12T16:12:15Z
```

#  Show a better hint for broken PBS venv

---

_@konstin_

For some reason, I'm encountering broken PBS venvs again (https://github.com/astral-sh/python-build-standalone/issues/380). Instead of showing the inscrutable error to the user, we point them to the PBS bug and ask them to run `uv venv`.

Fixed a duplicate "exit code" in the first commit.

**Before**

```
$ uv sync
error: Querying Python at `/tmp/uv/tests/.tmpwXZRTI/temp/.venv/bin/python3` failed with exit status exit status: 1

[stderr]
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = '/tmp/uv/tests/.tmpwXZRTI/temp/.venv/bin/python3'
  isolated = 1
  environment = 0
  user site = 0
  safe_path = 1
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = '/home/konsti/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/tmp/uv/tests/.tmpwXZRTI/temp/.venv/bin/python3'
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

Current thread 0x000078e48b835740 (most recent call first):
  <no Python frame>
```


**After**

```
$ uv sync
  error: Can't use Python at `/tmp/uv/tests/.tmpwXZRTI/temp/.venv/bin/python3`
    Caused by: Python is missing PYTHONHOME. If you are using a managed Python interpreter, this is a known bug (https://github.com/astral-sh/python-build-standalone/issues/380). You can recreate the virtual environment with `uv venv`.
```

---

_Label `error messages` added by @konstin on 2025-10-23 15:40_

---

_@konstin reviewed on 2025-10-23 15:41_

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:3987 on 2025-10-23 15:41_

This isn't ideal but it's consistent with the existing logic.

---

_Comment by @zanieb on 2025-10-23 15:49_

cc @geofft 

---

_Review requested from @zanieb by @konstin on 2025-12-02 15:18_

---

_Review requested from @geofft by @konstin on 2025-12-02 15:18_

---

_@zanieb reviewed on 2025-12-03 13:14_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:3987 on 2025-12-03 13:14_

Just to confirm, are we skipping this interpreter right now? or is this a change due to the changed error kind?

---

_@zanieb reviewed on 2025-12-03 13:17_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:907 on 2025-12-03 13:17_

It'd be nice to specialize based on whether this actually is a managed Python interpreter. I think we could do that by abstracting this logic to operate on a path

https://github.com/astral-sh/uv/blob/60a811e715d1d1919afd73b0e4dec4c4be8aa3f9/crates/uv-python/src/interpreter.rs#L301-L317

and then passing in the canonicalized path here and sys_base_prefix in the existing code?

This doesn't need to be blocking feedback, but if you don't do it, I probably would..

---

_@zanieb reviewed on 2025-12-03 13:18_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:3987 on 2025-12-03 13:18_

(It looks like we do for `UnexpectedResponse` so yes)

---

_@zanieb approved on 2025-12-03 13:18_

---
