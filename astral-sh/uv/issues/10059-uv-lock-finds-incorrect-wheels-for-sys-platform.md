---
number: 10059
title: "`uv lock` finds incorrect wheels for sys_platform == 'darwin' (pytorch)"
type: issue
state: closed
author: patrickhulce
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-12-20T15:36:40Z
updated_at: 2024-12-20T19:14:58Z
url: https://github.com/astral-sh/uv/issues/10059
synced_at: 2026-01-10T01:24:49Z
---

# `uv lock` finds incorrect wheels for sys_platform == 'darwin' (pytorch)

---

_Issue opened by @patrickhulce on 2024-12-20 15:36_

**Background**
I'm trying to run the same project on macOS and Ubuntu. For that, I have two sources for pytorch as described in the [uv docs](https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index). I'm using uv version `uv 0.5.11 (c4d0caaee 2024-12-19)` on macOS 14.4.1


**Steps to Reproduce**
```bash
uv init
cat > pyproject.toml <<EOF
[project]
name = "testuv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "torch",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cu118", marker = "sys_platform != 'darwin'" },
    { index = "pytorch-cpu", marker = "sys_platform == 'darwin'" },
]


[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
EOF
uv lock
uv sync
```

**What Happens**

uv incorrectly resolves the macOS to version `2.5.1+cpu` (only available on linux) instead of `2.5.1`. When you attempt to `uv sync` then it fails with this message.

```
error: Distribution `torch==2.5.1+cpu @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
``` 

Specifically, the lock file contains this block.

```
[[package]]
name = "torch"
version = "2.5.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "sys_platform == 'darwin'",
]
dependencies = [
    { name = "filelock", marker = "sys_platform == 'darwin'" },
    { name = "fsspec", marker = "sys_platform == 'darwin'" },
    { name = "jinja2", marker = "sys_platform == 'darwin'" },
    { name = "networkx", marker = "sys_platform == 'darwin'" },
    { name = "setuptools", marker = "sys_platform == 'darwin'" },
    { name = "sympy", marker = "sys_platform == 'darwin'" },
    { name = "typing-extensions", marker = "sys_platform == 'darwin'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp312-cp312-linux_x86_64.whl", hash = "sha256:4856f9d6925121d13c2df07aa7580b767f449dfe71ae5acde9c27535d5da4840" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp313-cp313-linux_x86_64.whl", hash = "sha256:5dbbdf83caa90d0bcaa50e4933ca424889133b35226db79000877d4ec5d9ea37" },
]
```

**What Did I Expect to Happen**
I expected uv to resolve the version to `2.5.1` and the wheels to 

```
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:8c712df61101964eb11910a846514011f0b6f5920c55dbf567bff8a34163d5b1" },
]
```


**Workarounds**

If I manually replace the `version` and `wheels` properties with the correct values, then`uv sync` works as expected.

---

_Label `bug` added by @charliermarsh on 2024-12-20 16:48_

---

_Label `duplicate` added by @charliermarsh on 2024-12-20 16:48_

---

_Comment by @charliermarsh on 2024-12-20 16:48_

This is tracked in https://github.com/astral-sh/uv/issues/9711. There are a variety of linked issues that are describing the same problem.

---

_Referenced in [astral-sh/uv#9711](../../astral-sh/uv/issues/9711.md) on 2024-12-20 16:51_

---

_Comment by @charliermarsh on 2024-12-20 19:14_

Fixed by https://github.com/astral-sh/uv/pull/10046.

---

_Closed by @charliermarsh on 2024-12-20 19:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-20 19:14_

---
