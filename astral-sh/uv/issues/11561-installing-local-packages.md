---
number: 11561
title: Installing local packages
type: issue
state: closed
author: Spacerulerwill
labels:
  - question
assignees: []
created_at: 2025-02-16T19:05:51Z
updated_at: 2025-02-16T22:06:58Z
url: https://github.com/astral-sh/uv/issues/11561
synced_at: 2026-01-10T01:25:07Z
---

# Installing local packages

---

_Issue opened by @Spacerulerwill on 2025-02-16 19:05_

### Question

I have the following setup:
```
project/
  common/
    __init__.py
    pyproject.toml
  main/
    main.py
    pyproject.toml
```
common's \_\_init\_\_,py is empty and pyproject.toml has the default generated for the uv init command. main/main.py contains `import common` and it's pyproject.toml looks like this:
```
[project]
name = "main"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "common @ file:///${PROJECT_ROOT}/../common"
]
```
with my working directory as `main`, I try and run `uv run main.py` only to get:
```
Traceback (most recent call last):
  File "main/main.py", line 1, in <module>
    import common
ModuleNotFoundError: No module named 'common'
```
I have inspected my uv virtual environment and in `.venv/lib/python3.12/site-packages` I can see the package metadata folder `common-0.1.0.dist-info` but not the folder for the package itself (as would appear when installing a pip package like pypi package repository. If I copy my common source code into  `.venv/lib/python3.12/site-packages` the import works. Why does it not do this already and is there a way to make it? The only way I can see to get around this is just to just add `project` above my to my python import path. Not an ideal solution. I also will encounter intellisense problems when opening the `main` folder as root folder in my editor instead of the `project` folder.

I am effectively trying to emulate how you can have adjacent crates in a rust project that can import things from eachother.

Anyway I can achieve this??

### Platform

Linux 6.8.0-53-generic x86_64 GNU/Linux

### Version

0.5.31

---

_Label `question` added by @Spacerulerwill on 2025-02-16 19:05_

---

_Comment by @charliermarsh on 2025-02-16 19:07_

A few things:

1. If you ran `uv init common`, you probably want to `rm -rf common` and then `uv init common --lib` to make sure that it creates a library you can import from.
2. I would suggest using workspaces for this instead of `${PROJECT_ROOT}`, which isn't really recommended. So in `main/pyproject.toml`:

```toml
[project]
name = "main"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "common"
]

[tool.uv.workspace]
members = ["../common"]

[tool.uv.sources]
common = { workspace = true }
```


---

_Comment by @Spacerulerwill on 2025-02-16 21:14_

Brilliant, it works. Thanks!

---

_Closed by @Spacerulerwill on 2025-02-16 21:14_

---

_Renamed from "Installing local pckages" to "Installing local packages" by @AlexWaygood on 2025-02-16 22:06_

---
