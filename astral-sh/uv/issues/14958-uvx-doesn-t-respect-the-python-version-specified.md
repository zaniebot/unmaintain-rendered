```yaml
number: 14958
title: "uvx doesn't respect the python version specified in the pyproject.toml"
type: issue
state: closed
author: jbullion-astra
labels:
  - question
assignees: []
created_at: 2025-07-29T18:25:42Z
updated_at: 2025-07-31T02:05:39Z
url: https://github.com/astral-sh/uv/issues/14958
synced_at: 2026-01-12T16:02:00Z
```

# uvx doesn't respect the python version specified in the pyproject.toml

---

_@jbullion-astra_

### Summary

mypackage specifies `requires-python = ">=3.11,<3.12"`.


This version of pyzmq==27.0.0 has a wheel available for my platform python 3.11.11. But when I try to run mypackage via uv tool it tries to build a different version of pyzmq.
```
$ uv pip install pyzmq
Resolved 1 package in 1.13s
Prepared 1 package in 399ms
Installed 1 package in 48ms
 + pyzmq==27.0.0

$ uvx mypackage @myversion
   Building pyzmq==26.0.3
^C
$ uvx mypackage @myversion --python 3.11.11
   Building pyzmq==26.0.3
^C

```

That build of pyzmq fails for my environment. 

Manually uninstalling all but the intended version of python worked for me.
```
$ uv python list
cpython-3.14.0rc1-windows-x86_64-none                 <download available>
cpython-3.14.0rc1+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.5-windows-x86_64-none                    <download available>
cpython-3.13.5+freethreaded-windows-x86_64-none       <download available>
cpython-3.13.0-windows-x86_64-none                    ...\cpython-3.13.0-windows-x86_64-none\python.exe
cpython-3.12.11-windows-x86_64-none                   <download available>
cpython-3.12.7-windows-x86_64-none                    ...\cpython-3.12.7-windows-x86_64-none\python.exe
cpython-3.11.13-windows-x86_64-none                   <download available>
cpython-3.11.11-windows-x86_64-none                  ...\cpython-3.11.11-windows-x86_64-none\python.exe
cpython-3.10.18-windows-x86_64-none                   <download available>
cpython-3.10.8-windows-x86_64-none                    C:\Program Files\Python310\python.exe
```

```
$ uv python list
cpython-3.14.0rc1-windows-x86_64-none                 <download available>
cpython-3.14.0rc1+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.5-windows-x86_64-none                    <download available>
cpython-3.13.5+freethreaded-windows-x86_64-none       <download available>
cpython-3.13.0-windows-x86_64-none                   <download available>
cpython-3.12.11-windows-x86_64-none                   <download available>
cpython-3.12.7-windows-x86_64-none                    <download available>
cpython-3.11.13-windows-x86_64-none                   <download available>
cpython-3.11.11-windows-x86_64-none                  ...\cpython-3.11.11-windows-x86_64-none\python.exe
cpython-3.10.18-windows-x86_64-none                   <download available>
cpython-3.10.8-windows-x86_64-none                    C:\Program Files\Python310\python.exe
```
```
$ uvx mypackage@myversion
Installed 65 packages in 497ms
Usage: mypackage [OPTIONS] COMMAND [ARGS]...
...
```

I am building mypackage via `uv build`:
```
[build-system]
requires = ["setuptools>=68"]
build-backend = "setuptools.build_meta"
```

Is this not capturing the requires python key? Why doesn't specifying the python version work? 

### Platform

Windows 11 x86

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

3.11.11

---

_Label `bug` added by @jbullion-astra on 2025-07-29 18:25_

---

_Comment by @zanieb on 2025-07-29 18:36_

See https://github.com/astral-sh/uv/issues/14711 and similar

---

_Comment by @jbullion-astra on 2025-07-29 18:49_

That is interesting @zanieb. But why would uvx ignore my ` --python 3.11.11`. 

---

_Comment by @zanieb on 2025-07-29 19:09_

In `uvx mypackage@myversion --python 3.11.11` you're passing that flag to `mypackage`. Use `uvx --python 3.11.11 mypackage@myversion` instead.

---

_Label `bug` removed by @zanieb on 2025-07-29 19:09_

---

_Label `question` added by @zanieb on 2025-07-29 19:09_

---

_Closed by @charliermarsh on 2025-07-31 02:05_

---
