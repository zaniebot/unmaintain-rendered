```yaml
number: 2547
title: py3.9 and 3.10 error if typing is importable from cwd
type: issue
state: closed
author: raylu
labels:
  - bug
assignees: []
created_at: 2024-03-19T18:40:54Z
updated_at: 2024-03-20T02:56:04Z
url: https://github.com/astral-sh/uv/issues/2547
synced_at: 2026-01-10T05:40:32Z
```

# py3.9 and 3.10 error if typing is importable from cwd

---

_Issue opened by @raylu on 2024-03-19 18:40_

```
$ docker run -it --rm python:3.10-slim bash
root@b6e27da47b8a:/# pip install uv
root@b6e27da47b8a:/# uv --version
uv 0.1.22
root@b6e27da47b8a:/# cd opt
root@b6e27da47b8a:/opt# touch typing.py
root@b6e27da47b8a:/opt# uv venv venv
  × Querying Python at `/usr/local/bin/python3` failed with status exit status: 1 with exit status: 1
  │ --- stdout:

  │ --- stderr:
  │ Traceback (most recent call last):
  │   File "/usr/local/lib/python3.10/runpy.py", line 196, in _run_module_as_main
  │     return _run_code(code, main_globals, None,
  │   File "/usr/local/lib/python3.10/runpy.py", line 86, in _run_code
  │     exec(code, run_globals)
  │   File "/root/.cache/uv/.tmpqYO0rn/python/get_interpreter_info.py", line 525, in <module>
  │     "platform": get_operating_system_and_architecture(),
  │   File "/root/.cache/uv/.tmpqYO0rn/python/get_interpreter_info.py", line 429, in get_operating_system_and_architecture
  │     from .packaging._manylinux import _get_glibc_version
  │   File "/root/.cache/uv/.tmpqYO0rn/python/packaging/_manylinux.py", line 10, in <module>
  │     from typing import Generator, Iterator, NamedTuple, Sequence
  │ ImportError: cannot import name 'Generator' from 'typing' (/opt/typing.py)
  │ ---
```
however, if you move to a directory where `typing` isn't importable, the `~/.cache/uv/interpreter-v0` cache gets populated, allowing you to use `uv` from whichever directory you like
```
root@b6e27da47b8a:/opt# cd /
root@b6e27da47b8a:/# uv venv opt/venv
Using Python 3.10.13 interpreter at: /usr/local/bin/python3
Creating virtualenv at: opt/venv
root@b6e27da47b8a:/# cd opt
root@b6e27da47b8a:/opt# uv pip list
Package    Version
---------- -------
pip        23.0.1
setuptools 65.5.1
uv         0.1.22
wheel      0.43.0
```
until you try a new interpreter
```
root@b6e27da47b8a:/opt# source venv/bin/activate
(venv) root@b6e27da47b8a:/opt# uv pip list
error: Querying Python at `/opt/venv/bin/python` failed with status exit status: 1 with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "/usr/local/lib/python3.10/runpy.py", line 196, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/usr/local/lib/python3.10/runpy.py", line 86, in _run_code
    exec(code, run_globals)
  File "/root/.cache/uv/.tmpsEXOzS/python/get_interpreter_info.py", line 525, in <module>
    "platform": get_operating_system_and_architecture(),
  File "/root/.cache/uv/.tmpsEXOzS/python/get_interpreter_info.py", line 429, in get_operating_system_and_architecture
    from .packaging._manylinux import _get_glibc_version
  File "/root/.cache/uv/.tmpsEXOzS/python/packaging/_manylinux.py", line 10, in <module>
    from typing import Generator, Iterator, NamedTuple, Sequence
ImportError: cannot import name 'Generator' from 'typing' (/opt/typing.py)
---
```

this happens in 3.9 and 3.10 but not 3.8 or 3.11

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-19 18:42_

---

_Comment by @charliermarsh on 2024-03-19 18:43_

Thanks. We use `PYTHONSAFEPATH` which was added in 3.11. I'll fix.

---

_Label `bug` added by @charliermarsh on 2024-03-19 18:43_

---

_Comment by @charliermarsh on 2024-03-19 21:48_

Did this happen to you in a real project? I'm trying to figure out how to solve it but adding `typing.py` breaks all `python -m` invocations anyway. E.g., `touch typing.py` and then `python -m pip list` also breaks.

---

_Comment by @charliermarsh on 2024-03-19 21:49_

Specifically, `runpy` doesn't work anymore once you do that.

---

_Comment by @raylu on 2024-03-19 22:05_

yes. in our project, we have
```
.
├── subdir1
│   ├── requirements1.txt
│   ├── some_code.py
│   └── typing
│       └── some_types.py
└── subdir2
    └── requirements2.txt
```
and our code runs from the root dir, so `subdir1/some_code.py` has `import subdir1.typing.some_types` and never runs into the import issue
we can `uv pip install -r subdir1/requirements1.txt`
but it'd be nice to be able to `cd subdir1 && uv pip install requirements1.txt` without having to prepopulate the cache
(actually, we have a tool that wraps `uv` that detects which subdir you're in and installs other requirements files differently)

---

_Closed by @charliermarsh on 2024-03-20 00:19_

---

_Comment by @raylu on 2024-03-20 02:52_

so speedy! thanks!

---

_Comment by @charliermarsh on 2024-03-20 02:56_

Any time :)

---
