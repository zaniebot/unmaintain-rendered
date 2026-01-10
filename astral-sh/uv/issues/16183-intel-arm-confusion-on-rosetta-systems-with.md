---
number: 16183
title: Intel/ARM confusion on Rosetta systems with python.org Python installations
type: issue
state: open
author: hynek
labels:
  - bug
assignees: []
created_at: 2025-10-08T11:46:56Z
updated_at: 2025-10-09T17:22:54Z
url: https://github.com/astral-sh/uv/issues/16183
synced_at: 2026-01-10T01:26:04Z
---

# Intel/ARM confusion on Rosetta systems with python.org Python installations

---

_Issue opened by @hynek on 2025-10-08 11:46_

### Summary

Context is, once again, my Rosetta macOS (sorry).

I've always used Python 3.13 by installing it from python.org and it still works for both ARM and Intel. For some reason, uv only detects ARM for 3.14 and fails with:

```
error: No interpreter found for any-3.14-any-x86_64-any in search path

hint: A managed Python download is available for any-3.14-any-x86_64-any, but the Python preference is set to 'only system'
```

when trying to sync in an Intel64 project (`export UV_PYTHON=3.14-x86_64
`). But it totally exists:

```
❯ type python3.14-intel64
python3.14-intel64 is /usr/local/bin/python3.14-intel64
```

And setting `UV_PYTHON=3.14-x86_64` works perfectly fine, too (but comes with the problems I've reported elsewhere).

---

Please note below that it seems to find 3.13.8 _only_ as x84_64!? Could it be based on what it was used first? It seems to be still working regardless of the output. Something must have changed how these versions are handled?

# uv python list

<details>

```
uv python list
cpython-3.14.0-macos-aarch64-none                 /usr/local/bin/python3.14 -> ../../../Library/Frameworks/Python.framework/Versions/3.14/bin/python3.14
cpython-3.14.0-macos-aarch64-none                 <download available>
cpython-3.14.0+freethreaded-macos-aarch64-none    /usr/local/bin/python3.14t -> ../../../Library/Frameworks/PythonT.framework/Versions/3.14/bin/python3.14t
cpython-3.14.0+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.8-macos-aarch64-none                 <download available>
cpython-3.13.8+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.8-macos-x86_64-none                  /usr/local/bin/python3.13 -> ../../../Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.8-macos-x86_64-none                  /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.8-macos-x86_64-none                  /Library/Frameworks/Python.framework/Versions/3.13/bin/python3 -> python3.13
cpython-3.13.7-macos-aarch64-none                 /opt/homebrew/bin/python3.13 -> ../Cellar/python@3.13/3.13.7/bin/python3.13
cpython-3.13.7-macos-aarch64-none                 /opt/homebrew/bin/python3 -> ../Cellar/python@3.13/3.13.7/bin/python3
cpython-3.13.7-macos-x86_64-none                  .local/share/uv/python/cpython-3.13.7-macos-x86_64-none/bin/python3.13
cpython-3.13.3+freethreaded-macos-aarch64-none    .local/share/uv/python/cpython-3.13.3+freethreaded-macos-aarch64-none/bin/python3.13t
cpython-3.13.2-macos-aarch64-none                 .local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13
cpython-3.13.0-macos-aarch64-none                 .local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
cpython-3.13.0+freethreaded-macos-aarch64-none    .local/share/uv/python/cpython-3.13.0+freethreaded-macos-aarch64-none/bin/python3.13t
cpython-3.12.11-macos-aarch64-none                /opt/homebrew/bin/python3.12 -> ../Cellar/python@3.12/3.12.11_1/bin/python3.12
cpython-3.12.11-macos-aarch64-none                <download available>
cpython-3.12.10-macos-x86_64-none                 /usr/local/bin/python3.12 -> ../../../Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.11.13-macos-aarch64-none                /opt/homebrew/bin/python3.11 -> ../Cellar/python@3.11/3.11.13_1/bin/python3.11
cpython-3.11.13-macos-aarch64-none                <download available>
cpython-3.11.9-macos-x86_64-none                  /usr/local/bin/python3.11 -> ../../../Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.10.18-macos-aarch64-none                /opt/homebrew/bin/python3.10 -> ../Cellar/python@3.10/3.10.18_1/bin/python3.10
cpython-3.10.18-macos-aarch64-none                <download available>
cpython-3.10.11-macos-x86_64-none                 /usr/local/bin/python3.10 -> ../../../Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10
cpython-3.9.23-macos-aarch64-none                 /opt/homebrew/bin/python3.9 -> ../Cellar/python@3.9/3.9.23_1/bin/python3.9
cpython-3.9.23-macos-aarch64-none                 <download available>
cpython-3.9.13-macos-aarch64-none                 /usr/local/bin/python3.9 -> ../../../Library/Frameworks/Python.framework/Versions/3.9/bin/python3.9
cpython-3.9.6-macos-aarch64-none                  /usr/bin/python3
cpython-3.8.20-macos-aarch64-none                 .local/share/uv/python/cpython-3.8.20-macos-aarch64-none/bin/python3.8
cpython-3.8.10-macos-aarch64-none                 /usr/local/bin/python3.8 -> ../../../Library/Frameworks/Python.framework/Versions/3.8/bin/python3.8
cpython-3.7.9-macos-x86_64-none                   /usr/local/bin/python3.7 -> ../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7
pypy-3.11.13-macos-aarch64-none                   <download available>
pypy-3.10.16-macos-aarch64-none                   <download available>
pypy-3.10.14-macos-aarch64-none                   /opt/homebrew/bin/pypy3 -> ../Cellar/pypy3.10/7.3.17_1/bin/pypy3
pypy-3.9.19-macos-aarch64-none                    .local/share/uv/python/pypy-3.9.19-macos-aarch64-none/bin/pypy3.9
pypy-3.8.16-macos-aarch64-none                    <download available>
graalpy-3.12.0-macos-aarch64-none                 <download available>
graalpy-3.11.0-macos-aarch64-none                 <download available>
graalpy-3.10.0-macos-aarch64-none                 <download available>
graalpy-3.8.5-macos-aarch64-none                  <download available>
```

</details>

### Platform

macOS 15.7.1, ARM

### Version

uv 0.9.0 (39b688653 2025-10-07)

### Python version

_No response_

---

_Label `bug` added by @hynek on 2025-10-08 11:46_

---

_Comment by @zanieb on 2025-10-09 15:05_

I don't think we find interpreters with the `-intel64` suffix

---

_Comment by @zanieb on 2025-10-09 15:15_

> Please note below that it seems to find 3.13.8 only as x84_64!? Could it be based on what it was used first? 

We do cache the interpreter results by path, so if it was run emulated we probably cached it as x86-64?

---

_Comment by @hynek on 2025-10-09 16:38_

> I don't think we find interpreters with the `-intel64` suffix

I'm not sure what that all implies, but I've 100% used (and use) the Python.org interpreters both for ARM and Intel. 😅

My understanding tho, is that those builds are universal and the suffixed version is just a convenience helper that guarantees to run under Intel.

---

_Comment by @zanieb on 2025-10-09 17:22_

We don't find executables with arbitrary names, is my point. We search for a known subset of names on the `PATH`. We'll need to add that one.

I think generally we just don't have a good way to support universal interpreters in our caching model. We'll need to think harder about that.

---
