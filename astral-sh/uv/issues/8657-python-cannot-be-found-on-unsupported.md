---
number: 8657
title: Python cannot be found on unsupported architectures
type: issue
state: closed
author: lengau
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-10-29T11:57:56Z
updated_at: 2024-10-29T14:11:41Z
url: https://github.com/astral-sh/uv/issues/8657
synced_at: 2026-01-10T01:24:31Z
---

# Python cannot be found on unsupported architectures

---

_Issue opened by @lengau on 2024-10-29 11:57_

On unsupported architectures (e.g. riscv64), `uv` cannot find the local Python because the machine is not in a list of known items:

<details><summary>uv output</summary>

```
# uv python find --verbose
DEBUG uv 0.4.19
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Failed to inspect Python interpreter from search path at `/usr/bin/python3`
DEBUG Skipping bad interpreter at /usr/bin/python3 from search path: Querying Python at `/usr/bin/python3` did not return the expected data
unknown variant `riscv64`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix","platform_machine": "riscv64", "platform_python_implementation": "CPython", "platform_release": "6.11.0-8-generic", "platform_system": "Linux", "platform_version": "#8.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Oct  1 11:40:56 UTC 2024", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "sys_base_prefix": "/usr", "sys_base_exec_prefix": "/usr", "sys_prefix": "/usr", "sys_base_executable": "/usr/bin/python3", "sys_executable": "/usr/bin/python3", "sys_path": ["/root/.cache/uv/.tmpvWackB", "/usr/lib/python312.zip", "/usr/lib/python3.12", "/usr/lib/python3.12/lib-dynload", "/usr/local/lib/python3.12/dist-packages", "/usr/lib/python3/dist-packages"], "stdlib": "/usr/lib/python3.12", "scheme": {"platlib": "/usr/local/lib/python3.12/dist-packages", "purelib": "/usr/local/lib/python3.12/dist-packages", "include": "/usr/include/python3.12", "scripts": "/usr/local/bin", "data": "/usr/local"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 39}, "arch": "riscv64"}, "manylinux_compatible": true, "gil_disabled": false, "pointer_size": "64"}
--- stderr:

---
DEBUG Failed to inspect Python interpreter from search path at `/usr/bin/python3.12`
DEBUG Skipping bad interpreter at /usr/bin/python3.12 from search path: Querying Python at `/usr/bin/python3.12` did not return the expected data
unknown variant `riscv64`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix","platform_machine": "riscv64", "platform_python_implementation": "CPython", "platform_release": "6.11.0-8-generic", "platform_system": "Linux", "platform_version": "#8.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Oct  1 11:40:56 UTC 2024", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "sys_base_prefix": "/usr", "sys_base_exec_prefix": "/usr", "sys_prefix": "/usr", "sys_base_executable": "/usr/bin/python3.12", "sys_executable": "/usr/bin/python3.12", "sys_path": ["/root/.cache/uv/.tmp84TuZT", "/usr/lib/python312.zip", "/usr/lib/python3.12", "/usr/lib/python3.12/lib-dynload", "/usr/local/lib/python3.12/dist-packages", "/usr/lib/python3/dist-packages"], "stdlib": "/usr/lib/python3.12", "scheme": {"platlib": "/usr/local/lib/python3.12/dist-packages", "purelib": "/usr/local/lib/python3.12/dist-packages", "include": "/usr/include/python3.12", "scripts": "/usr/local/bin", "data": "/usr/local"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 39}, "arch": "riscv64"}, "manylinux_compatible": true, "gil_disabled": false, "pointer_size": "64"}
--- stderr:

---
DEBUG Failed to inspect Python interpreter from search path at `/bin/python3`
DEBUG Skipping bad interpreter at /bin/python3 from search path: Querying Python at `/bin/python3` did not return the expected data
unknown variant `riscv64`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix","platform_machine": "riscv64", "platform_python_implementation": "CPython", "platform_release": "6.11.0-8-generic", "platform_system": "Linux", "platform_version": "#8.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Oct  1 11:40:56 UTC 2024", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "sys_base_prefix": "/usr", "sys_base_exec_prefix": "/usr", "sys_prefix": "/usr", "sys_base_executable": "/bin/python3", "sys_executable": "/bin/python3", "sys_path": ["/root/.cache/uv/.tmpxChyuH", "/usr/lib/python312.zip", "/usr/lib/python3.12", "/usr/lib/python3.12/lib-dynload", "/usr/local/lib/python3.12/dist-packages", "/usr/lib/python3/dist-packages"], "stdlib": "/usr/lib/python3.12", "scheme": {"platlib": "/usr/local/lib/python3.12/dist-packages", "purelib": "/usr/local/lib/python3.12/dist-packages", "include": "/usr/include/python3.12", "scripts": "/usr/local/bin", "data": "/usr/local"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 39}, "arch": "riscv64"}, "manylinux_compatible": true, "gil_disabled": false, "pointer_size": "64"}
--- stderr:

---
DEBUG Failed to inspect Python interpreter from search path at `/bin/python3.12`
DEBUG Skipping bad interpreter at /bin/python3.12 from search path: Querying Python at `/bin/python3.12` did not return theexpected data
unknown variant `riscv64`, expected one of `aarch64`, `armv6l`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.3", "os_name": "posix","platform_machine": "riscv64", "platform_python_implementation": "CPython", "platform_release": "6.11.0-8-generic", "platform_system": "Linux", "platform_version": "#8.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Oct  1 11:40:56 UTC 2024", "python_full_version": "3.12.3", "python_version": "3.12", "sys_platform": "linux"}, "sys_base_prefix": "/usr", "sys_base_exec_prefix": "/usr", "sys_prefix": "/usr", "sys_base_executable": "/bin/python3.12", "sys_executable": "/bin/python3.12", "sys_path": ["/root/.cache/uv/.tmpzRiSA8", "/usr/lib/python312.zip", "/usr/lib/python3.12", "/usr/lib/python3.12/lib-dynload", "/usr/local/lib/python3.12/dist-packages", "/usr/lib/python3/dist-packages"], "stdlib": "/usr/lib/python3.12", "scheme": {"platlib": "/usr/local/lib/python3.12/dist-packages", "purelib": "/usr/local/lib/python3.12/dist-packages", "include": "/usr/include/python3.12", "scripts": "/usr/local/bin", "data": "/usr/local"}, "virtualenv": {"purelib": "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 39}, "arch": "riscv64"}, "manylinux_compatible": true, "gil_disabled": false,"pointer_size": "64"}
--- stderr:

---
error: No interpreter found in virtual environments, managed installations, or system path
```
</details>

If the `platform_machine` of a local Python matches the machine type from `uname`, uv should also accept it.

---

_Label `bug` added by @zanieb on 2024-10-29 13:30_

---

_Label `uv python` added by @zanieb on 2024-10-29 13:30_

---

_Comment by @zanieb on 2024-10-29 13:35_

Thanks for the report! It seems fine to just add this to the list.

---

_Referenced in [astral-sh/uv#8660](../../astral-sh/uv/pulls/8660.md) on 2024-10-29 13:40_

---

_Comment by @zanieb on 2024-10-29 13:41_

Added in https://github.com/astral-sh/uv/pull/8660

---

_Closed by @zanieb on 2024-10-29 13:48_

---

_Closed by @zanieb on 2024-10-29 13:48_

---

_Comment by @lengau on 2024-10-29 14:11_

Thank you so much!

---
