```yaml
number: 3963
title: "fix: add missing ppc64le alias for powerpc64le"
type: pull_request
state: merged
author: mayeut
labels:
  - bug
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-06-02T07:03:45Z
updated_at: 2024-06-02T17:15:25Z
url: https://github.com/astral-sh/uv/pull/3963
synced_at: 2026-01-10T13:59:34Z
```

# fix: add missing ppc64le alias for powerpc64le

---

_Pull request opened by @mayeut on 2024-06-02 07:03_

## Summary

Same as #3899 but for ppc64le, there were no tests added there so I wouldn't know where to begin to do so.

Using image docker image `quay.io/pypa/manylinux2014_ppc64le` (uv 0.2.4)

```
[root@e3ff544d1337 ~]# python3.12 -V
Python 3.12.3
[root@e3ff544d1337 ~]# RUST_LOG="trace" uv venv py312 --python python3.12
DEBUG Searching for Python 3.12 in search path
TRACE Searching PATH for executables: python3.12, python3, python
TRACE Checking `PATH` directory for interpreters: /opt/rh/devtoolset-10/root/usr/bin
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python3.12
TRACE Querying interpreter executable at /usr/local/bin/python3.12
TRACE Querying Python at `/usr/local/bin/python3.12` did not return the expected data
unknown variant `ppc64le`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix", "platform_machine": "ppc64le", "platform_python_implementation": "CPython", "platform_release": "6.6.26-linuxkit", "platform_system": "Linux", "platform_version": "#1 SMP Sat Apr 27 04:13:19 UTC 2024", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "base_prefix": "/opt/_internal/cpython-3.12.3", "base_exec_prefix": "/opt/_internal/cpython-3.12.3", "prefix": "/opt/_internal/cpython-3.12.3", "base_executable": "/usr/local/bin/python3.12", "sys_executable": "/usr/local/bin/python3.12", "sys_path": ["/root/.cache/uv/.tmpBnM4PN", "/opt/_internal/cpython-3.12.3/lib/python312.zip", "/opt/_internal/cpython-3.12.3/lib/python3.12", "/opt/_internal/cpython-3.12.3/lib/python3.12/lib-dynload", "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages"], "stdlib": "/opt/_internal/cpython-3.12.3/lib/python3.12", "scheme": {"platlib": "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages", "purelib": "/opt/_internal/cpython-3.12.3/lib/python3.12/site-packages", "include": "/opt/_internal/cpython-3.12.3/include/python3.12", "scripts": "/opt/_internal/cpython-3.12.3/bin", "data": "/opt/_internal/cpython-3.12.3"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 17}, "arch": "ppc64le"}, "gil_disabled": false, "pointer_size": "64"}
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
  × No interpreter found for Python 3.12 in search path
```



---

_Comment by @codspeed-hq[bot] on 2024-06-02 07:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/mayeut:patch-1)

### Merging #3963 will **not alter performance**

<sub>Comparing <code>mayeut:patch-1</code> (ffa0f1a) with <code>main</code> (1eb968f)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_@charliermarsh approved on 2024-06-02 16:46_

---

_Label `bug` added by @charliermarsh on 2024-06-02 16:49_

---

_Merged by @charliermarsh on 2024-06-02 17:15_

---

_Closed by @charliermarsh on 2024-06-02 17:15_

---
