```yaml
number: 12155
title: "`uv python list` breaks when used from within WSL when Python installed via `pyenv-win`"
type: issue
state: closed
author: brysonwu
labels:
  - bug
  - windows
assignees: []
created_at: 2025-03-13T22:24:48Z
updated_at: 2025-08-18T15:42:49Z
url: https://github.com/astral-sh/uv/issues/12155
synced_at: 2026-01-10T03:32:45Z
```

# `uv python list` breaks when used from within WSL when Python installed via `pyenv-win`

---

_Issue opened by @brysonwu on 2025-03-13 22:24_

### Summary

Probably a very niche scenario but thought I should let folks know this issue exists.

## Reproduction

* Install uv in WSL and pyenv-win in Windows.
* `uv python list` will work at this point.
* Install a Python through `pyenv-win` (E.g.: run `pyenv install` in PowerShell to get latest Python)
* `uv python list` will abort and complain:
  ```sh
  error: Failed to inspect Python interpreter from search path at `/mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python`
  Caused by: Failed to query Python interpreter at `/mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python`
  Caused by: No such file or directory (os error 2)
  ```
* `uv python list` will work again after uninstalling `pyenv-win`-installed Pythons (Run `pyenv uninstall -a` in PowerShell)

Note that `uv python list` in PowerShell works fine with `pyenv-win`.

<details>
<summary> Verbose Error Log </summary>

```sh
DEBUG uv 0.6.6
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `../lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Ubuntu GLIBC 2.39-0ubuntu8.4) stable release version 2.39.\nCopyright (C) 2024 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.39 in stdout of `ldd --version`
DEBUG Searching for any Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/<user>/.local/share/uv/python`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `../lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Ubuntu GLIBC 2.39-0ubuntu8.4) stable release version 2.39.\nCopyright (C) 2024 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.39 in stdout of `ldd --version`
DEBUG Found managed installation `cpython-3.13.1-linux-x86_64-gnu`
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/bin/python3.13
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/bin/python3.13` (managed installations)
DEBUG Found managed installation `cpython-3.11.11-linux-x86_64-gnu`
TRACE Cached interpreter info for Python 3.11.11, skipping probing: /home/<user>/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/home/<user>/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11` (managed installations)
DEBUG Found managed installation `cpython-3.10.16-linux-x86_64-gnu`
TRACE Cached interpreter info for Python 3.10.16, skipping probing: /home/<user>/.local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu/bin/python3.10
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/<user>/.local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu/bin/python3.10` (managed installations)
DEBUG Found managed installation `cpython-3.9.21-linux-x86_64-gnu`
TRACE Cached interpreter info for Python 3.9.21, skipping probing: /home/<user>/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/bin/python3.9
DEBUG Found `cpython-3.9.21-linux-x86_64-gnu` at `/home/<user>/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/bin/python3.9` (managed installations)
DEBUG Found managed installation `cpython-3.8.20-linux-x86_64-gnu`
TRACE Cached interpreter info for Python 3.8.20, skipping probing: /home/<user>/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu/bin/python3.8
DEBUG Found `cpython-3.8.20-linux-x86_64-gnu` at `/home/<user>/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu/bin/python3.8` (managed installations)
TRACE Searching PATH for executables: python, python3, cpython, cpython3, pypy, pypy3, graalpy, graalpy3
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/plugins/pyenv-virtualenv/shims
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/shims
TRACE Found possible Python executable: /home/<user>/.pyenv/shims/python
TRACE Querying interpreter executable at /home/<user>/.pyenv/shims/python
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/<user>/.pyenv/shims/python` (first executable in the search path)
TRACE Found possible Python executable: /home/<user>/.pyenv/shims/python3
TRACE Querying interpreter executable at /home/<user>/.pyenv/shims/python3
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/<user>/.pyenv/shims/python3` (search path)
TRACE Found possible Python executable: /home/<user>/.pyenv/shims/python3.8
TRACE Querying interpreter executable at /home/<user>/.pyenv/shims/python3.8
DEBUG Failed to inspect Python interpreter from search path at `/home/<user>/.pyenv/shims/python3.8` 
DEBUG Skipping bad interpreter at /home/<user>/.pyenv/shims/python3.8 from search path: Querying Python at `/home/<user>/.pyenv/shims/python3.8` failed with exit status exit status: 127

[stderr]
/home/<user>/.pyenv/libexec/pyenv-exec: line 48: /mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python3.8: cannot execute: required file not found

TRACE Found possible Python executable: /home/<user>/.pyenv/shims/python3.13
TRACE Querying interpreter executable at /home/<user>/.pyenv/shims/python3.13
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.pyenv/shims/python3.13` (search path)
TRACE Found possible Python executable: /home/<user>/.pyenv/shims/python3.10
TRACE Querying interpreter executable at /home/<user>/.pyenv/shims/python3.10
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/<user>/.pyenv/shims/python3.10` (search path)
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/bin
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/plugins/pyenv-virtualenv/shims
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/bin
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/bin
TRACE Checking `PATH` directory for interpreters: /home/<user>/.local/bin
TRACE Found possible Python executable: /home/<user>/.local/bin/python
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/bin/python
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/bin/python` (search path)
TRACE Found possible Python executable: /home/<user>/.local/bin/python3
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/bin/python3
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/bin/python3` (search path)
TRACE Found possible Python executable: /home/<user>/.local/bin/python3.13
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/bin/python3.13
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/bin/python3.13` (search path)
TRACE Found possible Python executable: /home/<user>/.local/bin/python3.10
TRACE Cached interpreter info for Python 3.10.16, skipping probing: /home/<user>/.local/bin/python3.10
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/<user>/.local/bin/python3.10` (search path)
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/plugins/pyenv-virtualenv/shims
TRACE Checking `PATH` directory for interpreters: /home/<user>/.pyenv/bin
TRACE Checking `PATH` directory for interpreters: /home/<user>/.nvm/versions/node/v22.14.0/bin
TRACE Checking `PATH` directory for interpreters: /home/<user>/.local/bin
TRACE Found possible Python executable: /home/<user>/.local/bin/python
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/bin/python
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/bin/python` (search path)
TRACE Found possible Python executable: /home/<user>/.local/bin/python3
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/bin/python3
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/bin/python3` (search path)
TRACE Found possible Python executable: /home/<user>/.local/bin/python3.13
TRACE Cached interpreter info for Python 3.13.1, skipping probing: /home/<user>/.local/bin/python3.13
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/home/<user>/.local/bin/python3.13` (search path)
TRACE Found possible Python executable: /home/<user>/.local/bin/python3.10
TRACE Cached interpreter info for Python 3.10.16, skipping probing: /home/<user>/.local/bin/python3.10
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/<user>/.local/bin/python3.10` (search path)
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /usr/bin/python3
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
TRACE Found possible Python executable: /usr/bin/python3.12
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /usr/bin/python3.12
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3.12` (search path)
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /bin
TRACE Found possible Python executable: /bin/python3
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /bin/python3
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/bin/python3` (search path)
TRACE Found possible Python executable: /bin/python3.12
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /bin/python3.12
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/bin/python3.12` (search path)
TRACE Checking `PATH` directory for interpreters: /usr/games
TRACE Checking `PATH` directory for interpreters: /usr/local/games
TRACE Checking `PATH` directory for interpreters: /usr/lib/wsl/lib
TRACE Checking `PATH` directory for interpreters: /mnt/c/ProgramData/scoop/shims
TRACE Checking `PATH` directory for interpreters: /mnt/c/WINDOWS/system32
TRACE Checking `PATH` directory for interpreters: /mnt/c/WINDOWS
TRACE Checking `PATH` directory for interpreters: /mnt/c/WINDOWS/System32/Wbem
TRACE Checking `PATH` directory for interpreters: /mnt/c/WINDOWS/System32/WindowsPowerShell/v1.0/
TRACE Checking `PATH` directory for interpreters: /mnt/c/WINDOWS/System32/OpenSSH/
TRACE Checking `PATH` directory for interpreters: /mnt/c/Users/<user>/.pyenv/pyenv-win/bin
TRACE Checking `PATH` directory for interpreters: /mnt/c/Users/<user>/.pyenv/pyenv-win/shims
TRACE Found possible Python executable: /mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python
TRACE Querying interpreter executable at /mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python
DEBUG Failed to inspect Python interpreter from search path at `/mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python` 
error: Failed to inspect Python interpreter from search path at `/mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python`
  Caused by: Failed to query Python interpreter at `/mnt/c/Users/<user>/.pyenv/pyenv-win/shims/python`
  Caused by: No such file or directory (os error 2)
```
</details>

### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux (Ubuntu 24.04.1 LTS)

### Version

uv 0.6.6

### Python version

Python 3.13.1

---

_Label `bug` added by @brysonwu on 2025-03-13 22:24_

---

_Comment by @charliermarsh on 2025-03-13 22:25_

Thanks for filing and for the clear repro. We should fix it.

---

_Label `windows` added by @charliermarsh on 2025-03-13 22:25_

---

_Comment by @vdeolali on 2025-08-15 19:36_

Is this fixed yet? I still have this issue from within WSL - when I have pyenv-win installed on PSH

---

_Comment by @zanieb on 2025-08-15 20:36_

@vdeolali would you be willing to test https://github.com/astral-sh/uv/pull/15315 ? There are binaries attached to the CI summary (at the bottom).

---

_Closed by @zanieb on 2025-08-18 15:42_

---
