```yaml
number: 17205
title: Please add support for single-file packages
type: issue
state: open
author: gottadiveintopython
labels:
  - enhancement
assignees: []
created_at: 2025-12-21T10:18:19Z
updated_at: 2025-12-21T14:21:24Z
url: https://github.com/astral-sh/uv/issues/17205
synced_at: 2026-01-12T16:02:46Z
```

# Please add support for single-file packages

---

_@gottadiveintopython_

# Sammary


The `uv build` command doesn't seem to support a single-file module layout (i.e. a non-`__init__.py` module) like the following:

```
<project-root>
  ├── pyproject.toml
  └── src
       └── a_module.py
```

I would like `uv build` to support this layout.

# Steps to Reproduce

1. Run `$ uv init --lib mylib` to generate the following structure (git-related files omitted):

```
./mylib/README.md
./mylib/.python-version
./mylib/src/mylib/py.typed
./mylib/src/mylib/__init__.py
./mylib/pyproject.toml
```

2. Remove the package directory and replace it with a single-file module:

```shell
$ rm -R ./mylib/src/mylib
$ touch ./mylib/src/mylib.py
```

The resulting structure is:

```
./mylib/README.md
./mylib/.python-version
./mylib/src/mylib.py
./mylib/pyproject.toml
```

At this point, `$ uv sync` fails:

```
$ cd ./mylib
$ uv sync
```

```
Using CPython 3.13.8
Creating virtual environment at: .venv
Resolved 1 package in 3ms
  × Failed to build `mylib @ file:///path/to/mylib`
  ╰─▶ Expected a Python module at: src/mylib/__init__.py
```

`$ uv build` fails too:

```
Building source distribution (uv build backend)...
  × Failed to build `/path/to/mylib`
  ╰─▶ Expected a Python module at: src/mylib/__init__.py

```

3. But, adding the following option to the `pyproject.toml` makes `$ uv sync` work:

```
[tool.uv.build-backend]
namespace = true
```

```
Installed 1 package in 0.81ms
 ~ mylib==0.1.0 (from file:///path/to/mylib)
```
But `$ uv build` still fails:

```
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
  × Failed to build `/path/to/mylib`
  ├─▶ Failed to walk source tree: /home/my-username/.cache/uv/sdists-v9/.xxxxxxxxx/mylib-0.1.0
  ├─▶ IO error for operation on /home/my-username/.cache/uv/sdists-v9/.xxxxxxxxx/mylib-0.1.0/src/mylib: No such file or directory (os error 2)
  ╰─▶ No such file or directory (os error 2)
```

# Tested Environment
 
- uv 0.9.2
- Linux Mint 22.1


### Example

_No response_

---

_Label `enhancement` added by @gottadiveintopython on 2025-12-21 10:18_

---

_Comment by @zanieb on 2025-12-21 14:21_

Related

- https://github.com/astral-sh/uv/pull/11325

---
