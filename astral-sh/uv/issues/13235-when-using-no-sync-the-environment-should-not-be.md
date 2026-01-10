---
number: 13235
title: "When using `--no-sync`, the environment should not be invalidated"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-04-30T15:50:50Z
updated_at: 2025-05-05T14:57:48Z
url: https://github.com/astral-sh/uv/issues/13235
synced_at: 2026-01-10T01:25:30Z
---

# When using `--no-sync`, the environment should not be invalidated

---

_Issue opened by @zanieb on 2025-04-30 15:50_

As reported in https://github.com/astral-sh/uv/issues/13233 — invalidating the environment during `uv run` is problematic if syncing is disabled.

I presumed this regressed in https://github.com/astral-sh/uv/pull/12884 — but it [doesn't look like it](https://github.com/astral-sh/uv/issues/13235#issuecomment-2842472598).

For example:

```
❯ UV_PYTHON=3.13 uv sync
Using CPython 3.13.3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 6 packages in 16ms
Installed 4 packages in 14ms
 + iniconfig==2.1.0
 + packaging==25.0
 + pluggy==1.5.0
 + pytest==8.3.5
❯ UV_PYTHON=3.14 uv run -v --no-sync pytest
DEBUG uv 0.7.1+3 (560ba105b 2025-04-30)
DEBUG Found project root: `/Users/zb/workspace/uv/example`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Discovered project `example` at: /Users/zb/workspace/uv/example
DEBUG Acquired lock for `/Users/zb/workspace/uv/example`
DEBUG Using Python request `3.14` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version does not satisfy the request: `Python 3.14`
DEBUG Searching for Python 3.14 in managed installations or search path
DEBUG Searching for managed installations at `/Users/zb/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.3-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.13.2-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.13.1-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.12.9-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.11.11-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.16-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.9.21-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.20-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.18-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.12-macos-aarch64-none`
DEBUG Found `cpython-3.13.3-macos-aarch64-none` at `/opt/homebrew/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.13/bin/python3.13` from first executable in the search path: does not satisfy request `3.14`
DEBUG Found `cpython-3.14.0a3-macos-aarch64-none` at `/usr/local/bin/python3.14` (search path)
Using CPython 3.14.0a3 interpreter at: /usr/local/bin/python3.14
Removed virtual environment at: .venv
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /usr/local/bin/python3.14
DEBUG Released lock at `/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/uv-cff78d0b34cfcb51.lock`
DEBUG Skipping environment synchronization due to `--no-sync`
DEBUG Using Python 3.14.0a3 interpreter at: /Users/zb/workspace/uv/example/.venv/bin/python
DEBUG Running `pytest`
error: Failed to spawn: `pytest`
  Caused by: No such file or directory (os error 2)
```

### Platform

macOS

### Version

`main`

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-04-30 15:50_

---

_Referenced in [astral-sh/uv#13234](../../astral-sh/uv/pulls/13234.md) on 2025-04-30 15:51_

---

_Comment by @zanieb on 2025-04-30 15:56_

This doesn't look like a regression, this behavior existed already

```
❯ UV_PYTHON=3.13 uvx uv@0.6.14 sync
Installed 1 package in 7ms
Using CPython 3.13.3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 6 packages in 11ms
Installed 4 packages in 7ms
 + iniconfig==2.1.0
 + packaging==25.0
 + pluggy==1.5.0
 + pytest==8.3.5
❯ UV_PYTHON=3.14 uvx uv@0.6.14 run --no-sync pytest
Installed 1 package in 2ms
Using CPython 3.14.0a3 interpreter at: /usr/local/bin/python3.14
Removed virtual environment at: .venv
Creating virtual environment at: .venv
error: Failed to spawn: `pytest`
  Caused by: No such file or directory (os error 2)
```

---

_Comment by @zanieb on 2025-04-30 17:45_

Presumably, we should warn the Python request will not be met? Or fail early? Unclear.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-04 15:53_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-05-04 16:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-04 21:43_

---

_Referenced in [astral-sh/uv#13287](../../astral-sh/uv/pulls/13287.md) on 2025-05-04 22:30_

---

_Closed by @charliermarsh on 2025-05-05 14:57_

---
