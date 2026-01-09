---
number: 12521
title: uv venv will default to 3.13.0 rather than latest 3.13
type: issue
state: closed
author: gaborbernat
labels:
  - question
assignees: []
created_at: 2025-03-28T02:01:15Z
updated_at: 2025-03-28T14:30:57Z
url: https://github.com/astral-sh/uv/issues/12521
synced_at: 2026-01-07T13:12:18-06:00
---

# uv venv will default to 3.13.0 rather than latest 3.13

---

_Issue opened by @gaborbernat on 2025-03-28 02:01_

### Summary

```
mkdir demo
cd demo

~/w/get on  main [$] ❯ uv venv --verbose
DEBUG uv 0.6.10
DEBUG Found project root: `/home/jovyan/w`
DEBUG No workspace root found, using project root
DEBUG Using Python request `==3.13` from `requires-python` metadata
DEBUG Searching for Python ==3.13 in managed installations or search path
DEBUG Searching for managed installations at `/home/jovyan/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.2-linux-aarch64-gnu`
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/home/jovyan/.local/bin/python3` (first executable in the search path)
DEBUG Ignoring Python interpreter at `/home/jovyan/.local/share/uv/tools/jupyter-core/bin/python`: system interpreter required
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `==3.13`
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `==3.13`
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/usr/bin/python3.13` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3.13` from search path: does not satisfy request `==3.13`
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/bin/python3` (search path)
DEBUG Skipping interpreter at `/bin/python3` from search path: does not satisfy request `==3.13`
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/bin/python` (search path)
DEBUG Skipping interpreter at `/bin/python` from search path: does not satisfy request `==3.13`
DEBUG Found `cpython-3.13.2-linux-aarch64-gnu` at `/bin/python3.13` (search path)
DEBUG Skipping interpreter at `/bin/python3.13` from search path: does not satisfy request `==3.13`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/jovyan/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
Downloading cpython-3.13.0-linux-aarch64-gnu (16.8MiB)
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-aarch64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/jovyan/.local/share/uv/python/.temp/.tmpbGSJ6n
DEBUG Extracting cpython-3.13.0%2B20241016-aarch64-unknown-linux-gnu-install_only_stripped.tar.gz
 Downloaded cpython-3.13.0-linux-aarch64-gnu
DEBUG Moving /home/jovyan/.local/share/uv/python/.temp/.tmpbGSJ6n/python to /home/jovyan/.local/share/uv/python/cpython-3.13.0-linux-aarch64-gnu
DEBUG Released lock at `/home/jovyan/.local/share/uv/python/.lock`
Using CPython 3.13.0
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /home/jovyan/.local/share/uv/python/cpython-3.13.0-linux-aarch64-gnu/bin/python3.13
DEBUG Using base executable for virtual environment: /home/jovyan/.local/share/uv/python/cpython-3.13.0-linux-aarch64-gnu/bin/python3.13
```

Noticed this because I already had 3.13.2 downloaded and uv venv was hanging for a while.

### Example

_No response_

---

_Label `enhancement` added by @gaborbernat on 2025-03-28 02:01_

---

_Comment by @zanieb on 2025-03-28 02:28_

> Using Python request `==3.13` from `requires-python` metadata

`==3.13` means `3.13.0` per the specification, you want `==3.13.*` (or some other specifier).

---

_Comment by @zanieb on 2025-03-28 02:29_

We have a dedicated warning for this in some contexts

https://github.com/astral-sh/uv/blob/9af989e30c86391767830aa588c32fcb7c9a76b5/crates/uv/src/commands/project/lock.rs#L535

---

_Label `enhancement` removed by @zanieb on 2025-03-28 02:29_

---

_Label `question` added by @zanieb on 2025-03-28 02:29_

---

_Closed by @gaborbernat on 2025-03-28 14:30_

---
