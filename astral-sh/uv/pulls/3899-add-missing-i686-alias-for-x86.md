```yaml
number: 3899
title: Add missing i686 alias for x86
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/arch-i686
created_at: 2024-05-29T08:55:03Z
updated_at: 2024-05-29T15:31:40Z
url: https://github.com/astral-sh/uv/pull/3899
synced_at: 2026-01-10T13:59:34Z
```

# Add missing i686 alias for x86

---

_Pull request opened by @konstin on 2024-05-29 08:55_

This alias is required for the 32-bit x86 manylinux builder.

I've also changed the reporting level for unusable python interpreter errors to debug so it shows up with `-v`, otherwise we can't see what's broken:

```console
# uv venv py312 --python python3.12 -vv
    0.000461s DEBUG uv_interpreter::discovery Searching for Python 3.12 in search path
  × No interpreter found for Python 3.12 in search path
```

Reproduction:

```console
$ docker run --rm -it quay.io/pypa/manylinux2014_i686
# curl -LsSf https://astral.sh/uv/install.sh | s
# source $HOME/.cargo/env
# RUST_LOG=trace uv venv py312 --python python3.12
DEBUG Searching for Python 3.12 in search path
TRACE Searching PATH for executables: python3.12, python3, python
TRACE Checking `PATH` directory for interpreters: /root/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /opt/rh/devtoolset-10/root/usr/bin
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python3.12
TRACE Querying interpreter executable at /usr/local/bin/python3.12
TRACE Querying Python at `/usr/local/bin/python3.12` did not return the expected data
unknown variant `i686`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix", "platform_machine": "i686", "platform_python_implementation": "CPython", "platform_release": "6.1.0-10-amd64", "platform_system": "Linux", "platform_version": "#1 SMP PREEMPT_DYNAMIC Debian 6.1.37-1 (2023-07-03)", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "base_prefix": "/opt/_internal/cpython-3.12.3", "base_exec_prefix": "/opt/_internal/cpython-3.12.3", "prefix": "/opt/_internal/cpython-3.12.3", "base_executable": "/usr/local/bin/python3.12", "sys_executable": "/usr/local/bin/python3.12", "sys_path": ["/root/.cache/uv/.tmp7k64ZY", "/opt/_internal/cpython-3.12.3/lib/python312.zip", "/opt/_internal/cpython-3.12.3/lib/python3.12", "/opt/_internal/cpython-3.12.3/lib/python3.12/lib-dynload", "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages"], "stdlib": "/opt/_internal/cpython-3.12.3/lib/python3.12", "scheme": {"platlib": "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages", "purelib": "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages", "include": "/opt/_internal/cpython-3.12.3/include/python3.12", "scripts": "/opt/_internal/cpython-3.12.3/bin", "data": "/opt/_internal/cpython-3.12.3"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 17}, "arch": "i686"}, "gil_disabled": false, "pointer_size": "32"}
--- stderr:

---
TRACE Skipping bad interpreter at /usr/local/bin/python3.12
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python
TRACE Querying interpreter executable at /usr/bin/python
TRACE Can't use Python at `/usr/bin/python`
TRACE Skipping bad interpreter at /usr/bin/python
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /bin
TRACE Found possible Python executable: /bin/python
TRACE Querying interpreter executable at /bin/python
TRACE Can't use Python at `/bin/python`
TRACE Skipping bad interpreter at /bin/python
```


---

_Label `bug` added by @konstin on 2024-05-29 08:55_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:378 on 2024-05-29 13:22_

Why does this get dropped? Why is tracing it necessary?

---

_@charliermarsh reviewed on 2024-05-29 13:22_

---

_@charliermarsh reviewed on 2024-05-29 13:24_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:378 on 2024-05-29 13:24_

Like what is the output before and after this change alone?

---

_@henryiii reviewed on 2024-05-29 14:35_

---

_Review comment by @henryiii on `crates/uv-interpreter/src/discovery.rs`:378 on 2024-05-29 14:35_

Before was:
```console
$ uv venv py312 --python python3.12 -vv
    0.000461s DEBUG uv_interpreter::discovery Searching for Python 3.12 in search path
  × No interpreter found for Python 3.12 in search path
```

---

_@konstin reviewed on 2024-05-29 15:30_

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:378 on 2024-05-29 15:30_

After it becomes:

```console
$ uv venv --python python3.12 -v
DEBUG Searching for Python 3.12 in search path
DEBUG Querying Python at `/usr/local/bin/python3.12` did not return the expected data
unknown variant `i686`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix", "platform_machine": "i686", "platform_python_implementation": "CPython", "platform_release": "6.8.0-31-generic", "platform_system": "Linux", "platform_version": "#31-Ubuntu SMP PREEMPT_DYNAMIC Sat Apr 20 00:40:06 UTC 2024", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "base_prefix": "/opt/_internal/cpython-3.12.3", "base_exec_prefix": "/opt/_internal/cpython-3.12.3", "prefix": "/opt/_internal/cpython-3.12.3", "base_executable": "/usr/local/bin/python3.12", "sys_executable": "/usr/local/bin/python3.12", "sys_path": ["/root/.cache/uv/.tmpvgwwbk", "/opt/_internal/cpython-3.12.3/lib/python312.zip", "/opt/_internal/cpython-3.12.3/lib/python3.12", "/opt/_internal/cpython-3.12.3/lib/python3.12/lib-dynload", "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages"], "stdlib": "/opt/_internal/cpython-3.12.3/lib/python3.12", "scheme": {"platlib": "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages", "purelib": "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages", "include": "/opt/_internal/cpython-3.12.3/include/python3.12", "scripts": "/opt/_internal/cpython-3.12.3/bin", "data": "/opt/_internal/cpython-3.12.3"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 17}, "arch": "i686"}, "gil_disabled": false, "pointer_size": "32"}
--- stderr:

---
DEBUG Can't use Python at `/usr/bin/python`
DEBUG Can't use Python at `/bin/python`
  × No interpreter found for Python 3.12 in search path
```

We now show (with `-v`) when we rejected a python interpreter, so that users can retrace what happened.

---

_@charliermarsh approved on 2024-05-29 15:31_

---

_Merged by @charliermarsh on 2024-05-29 15:31_

---

_Closed by @charliermarsh on 2024-05-29 15:31_

---

_Branch deleted on 2024-05-29 15:31_

---
