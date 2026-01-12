```yaml
number: 552
title: "Failure to install `rfernet==0.3.1`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-04T22:03:55Z
updated_at: 2023-12-12T14:46:39Z
url: https://github.com/astral-sh/uv/issues/552
synced_at: 2026-01-12T15:58:23Z
```

# Failure to install `rfernet==0.3.1`

---

_@charliermarsh_

Gives:

```text
error: Failed to download distributions
  Caused by: Failed to build: rfernet==0.3.1
  Caused by: Build backend failed to build wheel through `build_wheel()`:
--- stdout:
Running `maturin pep517 build-wheel -i /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpKt677L/.venv/bin/python --compatibility off`
--- stderr:
Traceback (most recent call last):
  File "<string>", line 6, in <module>
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpKt677L/.venv/lib/python3.10/site-packages/maturin/__init__.py", line 111, in build_wheel
    return _build_wheel(wheel_directory, config_settings, metadata_directory)
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpKt677L/.venv/lib/python3.10/site-packages/maturin/__init__.py", line 90, in _build_wheel
    result = subprocess.run(command, stdout=subprocess.PIPE)
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.13/lib/python3.10/subprocess.py", line 503, in run
    with Popen(*popenargs, **kwargs) as process:
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.13/lib/python3.10/subprocess.py", line 971, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.13/lib/python3.10/subprocess.py", line 1863, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'maturin'
---
```

---

_Label `bug` added by @charliermarsh on 2023-12-04 22:04_

---

_Comment by @charliermarsh on 2023-12-04 22:04_

\cc @konstin in case you have ideas.

---

_Assigned to @konstin by @konstin on 2023-12-05 12:51_

---

_Closed by @konstin on 2023-12-12 14:46_

---
