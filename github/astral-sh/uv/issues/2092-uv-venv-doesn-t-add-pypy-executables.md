---
number: 2092
title: "`uv venv` doesn't add PyPy executables"
type: issue
state: closed
author: charliermarsh
labels:
  - pypy
assignees: []
created_at: 2024-02-29T18:05:37Z
updated_at: 2024-07-15T18:28:33Z
url: https://github.com/astral-sh/uv/issues/2092
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv venv` doesn't add PyPy executables

---

_Issue opened by @charliermarsh on 2024-02-29 18:05_

We don't officially support PyPy right now, this is more a note for the future, but we need to include a variety of `pypy` binaries / symlinks:

```
❯ ls .venv/bin
Activate.ps1  activate      activate.csh  activate.fish pip           pip3          pip3.9        pypy          pypy3         pypy3.9       python        python3       python3.9
```

---

_Label `pypy` added by @charliermarsh on 2024-02-29 18:05_

---

_Comment by @charliermarsh on 2024-02-29 18:46_

@gaborbernat - A related question: how does `virtualenv` "know" to create site-packages for PyPy at `.venv/lib/pypy3.10/site-packages` instead of `.venv/lib/python3.10/site-packages`? Is that encoded in `virtualenv` somewhere? Or is it read from the interpreter? Was trying to find it.

---

_Comment by @gaborbernat on 2024-02-29 18:52_

That's done here https://github.com/pypa/virtualenv/blob/main/src/virtualenv/create/via_global_ref/builtin/pypy/pypy3.py#L31 and https://github.com/pypa/virtualenv/blob/main/src/virtualenv/create/via_global_ref/builtin/pypy/common.py#L41. It knows to do that just for PyPy because of https://github.com/pypa/virtualenv/blob/main/src/virtualenv/create/via_global_ref/builtin/pypy/common.py#L13. The purelib/platlib paths are using the sysconfig information, which is inquired as part of https://github.com/pypa/virtualenv/blob/main/src/virtualenv/discovery/py_info.py#L80-L103. 

Note that `.venv/lib/pypy3.10/site-packages` is often refered to as purelib, and in CPython by default this aligns up with platlib, but CPython allows distributions to move this to their desired path. So ultimately, the question is: When you say `.venv/lib/pypy3.10/site-packages` are you refering to platlib or purelib?

---

_Comment by @charliermarsh on 2024-02-29 18:58_

Ah yeah, I'm referring to `purelib`. (Though I haven't really done the work yet to understand what happens when you, e.g., `pip uninstall` and `purelib` and `platlib` are not the same thing.)

If `sysconfig` is returning the `purelib` and `platlib` paths for the invoked interpreter, where / how do those get translated to paths for the virtualenv? E.g.:

```python
>>> sysconfig.get_paths(scheme="venv")
{'stdlib': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12', 'platstdlib': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12', 'purelib': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages', 'platlib': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/lib/python3.12/site-packages', 'include': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/include/python3.12', 'platinclude': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/include/python3.12', 'scripts': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0/bin', 'data': '/Users/crmarsh/.local/share/rtx/installs/python/3.12.0'}
```

---

_Comment by @gaborbernat on 2024-02-29 19:04_

That depends. You can get the schema and inject your config vars of sys.prefix to calculate where it should go.

---

_Comment by @gaborbernat on 2024-02-29 19:05_

See https://github.com/pypa/virtualenv/blob/main/src/virtualenv/discovery/py_info.py#L224

---

_Comment by @charliermarsh on 2024-02-29 20:36_

Thank you!

---

_Comment by @charliermarsh on 2024-02-29 20:48_

Filed some info in https://github.com/astral-sh/uv/issues/2095. (No obligation to chime in there.)

---

_Referenced in [astral-sh/uv#2096](../../astral-sh/uv/issues/2096.md) on 2024-02-29 20:58_

---

_Comment by @charliermarsh on 2024-03-09 20:05_

@edgarrmondragon - when you’re in a PyPy virtualenv, do you typically invoke Python as “pypy” or “python”? I’m trying to make some decisions around this with respect to #2316.

---

_Comment by @edgarrmondragon on 2024-03-11 05:16_

> @edgarrmondragon - when you’re in a PyPy virtualenv, do you typically invoke Python as “pypy” or “python”? I’m trying to make some decisions around this with respect to #2316.
 
Actually, always as `python` or `python3.x`

---

_Referenced in [astral-sh/uv#2386](../../astral-sh/uv/issues/2386.md) on 2024-03-12 17:57_

---

_Referenced in [astral-sh/uv#5047](../../astral-sh/uv/pulls/5047.md) on 2024-07-14 14:46_

---

_Closed by @zanieb on 2024-07-15 18:28_

---

_Closed by @zanieb on 2024-07-15 18:28_

---
