---
number: 16310
title: Hash mismatch for packages
type: issue
state: closed
author: NourEldin-Osama
labels:
  - bug
  - external
assignees: []
created_at: 2025-10-15T10:28:48Z
updated_at: 2025-10-17T21:10:33Z
url: https://github.com/astral-sh/uv/issues/16310
synced_at: 2026-01-07T13:12:19-06:00
---

# Hash mismatch for packages

---

_Issue opened by @NourEldin-Osama on 2025-10-15 10:28_

### Summary

Here is a simple pyproject.toml

```
[project]
name = "data-preprocessing"
version = "0.1.0"
description = "A data preprocessing pipeline for computer vision tasks."
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "cleanvision>=0.3.6",
    "opencv-python-headless>=4.12.0.88",
    "pydantic-settings>=2.11.0",
    "pyfastcopy>=1.0.3",
    "rich>=14.2.0",
    "supervision>=0.26.1",
    "torch>=2.8.0",
    "torchvision>=0.23.0",
    "typer>=0.19.2",
    "ultralytics>=8.3.213",
]

[project.scripts]
data-preprocessing = "data_preprocessing.main:app"

[tool.uv.sources]
torch = [
  { index = "pytorch-cu128", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
torchvision = [
  { index = "pytorch-cu128", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[tool.uv]
override-dependencies = [
    "opencv-python ; sys_platform == 'never'"
]

[tool.ruff]
lint.ignore = ["F541"]

[build-system]
requires = ["uv_build>=0.9.2,<0.10.0"]
build-backend = "uv_build"
```

```
>> uv sync 

Resolved 81 packages in 2ms
  × Failed to download `torchvision==0.24.0+cu128`
  ╰─▶ Hash mismatch for `torchvision==0.24.0+cu128`

      Expected:
        sha256:d594f61269cab0524a1e6f5f9e7e5cb26e4e0bed8ba059f64fd4acdf7cd76d53
        sha256:ff1c9be01024e6d419aa2551d2c604cec99cb867d39841ee66338fd60981a398
        sha256:0e485d987a1606c942a3e4a867cdd3f77991ddb5b561bae08f70314b7093a331
        sha256:2c341ebb8ccaa6e7767c0fa1f1442a6935691de92c003d98ed5f47c84f8439cb

      Computed:
        sha256:f82cd941bc36033ebdb2974c83caa2913cc37e6567fe97cdd69f5a568ff182c8
  help: `torchvision` (v0.24.0+cu128) was included because `data-preprocessing` (v0.1.0) depends on `torchvision`
```

### Platform

Windows 11 x86_64

### Version

uv 0.9.2 (141369ce7 2025-10-10)

### Python version

Python 3.13.8

---

_Label `bug` added by @NourEldin-Osama on 2025-10-15 10:28_

---

_Referenced in [pytorch/vision#9243](../../pytorch/vision/issues/9243.md) on 2025-10-15 11:13_

---

_Comment by @konstin on 2025-10-15 11:18_

This is a bug in the torch index, which declares the wrong hash: https://github.com/pytorch/vision/issues/9243

---

_Label `external` added by @konstin on 2025-10-15 11:18_

---

_Comment by @konstin on 2025-10-15 15:06_

Fixed on the torch side.

---

_Closed by @konstin on 2025-10-15 15:06_

---

_Comment by @NourEldin-Osama on 2025-10-17 21:10_

Thanks @konstin 

---
