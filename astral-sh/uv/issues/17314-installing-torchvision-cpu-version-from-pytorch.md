```yaml
number: 17314
title: "Installing torchvision CPU version from Pytorch PyPi registry is failing as it's targeting the wrong distribution"
type: issue
state: open
author: mc51
labels:
  - bug
  - external
assignees: []
created_at: 2026-01-04T11:19:36Z
updated_at: 2026-01-08T21:56:50Z
url: https://github.com/astral-sh/uv/issues/17314
synced_at: 2026-01-10T03:11:36Z
```

# Installing torchvision CPU version from Pytorch PyPi registry is failing as it's targeting the wrong distribution

---

_Issue opened by @mc51 on 2026-01-04 11:19_

### Summary

It seems that trying to install torchvision as cpu only version from the index `https://download.pytorch.org/whl/cpu` is currently broken.

This is caused by the [PyTorch PyPi registry for torchvision](https://download.pytorch.org/whl/torchvision/), that includes some patch versions of the wheels. There's an [issue](https://github.com/pytorch/vision/issues/9253) for that already.

Here's a minimal `pyproject.toml` that replicates this for Linux:

```toml
[project]
name = "uv-torchvision"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "torchvision>=0.2",
]

[[tool.uv.index]]
name     = "pytorch-cpu"
url      = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv.sources]
torchvision = [{ index = "pytorch-cpu" }]
```

The error on `uv sync` is:

> ~/dev/tmp/uv-torchvision main*​  ❯ uv sync
Using CPython 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
Resolved 31 packages in 1.83s
error: Distribution `torchvision==0.24.1 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
> hint: You're on Linux (`manylinux_2_39_x86_64`), but `torchvision` (v0.24.1) only has wheels for the following platforms: `manylinux_2_28_aarch64`, `macosx_11_0_arm64`; consider adding "sys_platform == 'linux' and platform_machine == 'x86_64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels

A solution for me was to pin the version: `"torchvision==0.24.1+cpu"`


### Platform

Linux Mint 22 amd64

### Version

uv 0.9.21

### Python version

Python 3.12.3

---

_Label `bug` added by @mc51 on 2026-01-04 11:19_

---

_Renamed from "torchvision cpu is trying to install the wrong distribution" to "Installing torchvision CPU version from Pytorch PyPi registry is failing as it's targeting the wrong distribution" by @mc51 on 2026-01-04 11:25_

---

_Label `external` added by @charliermarsh on 2026-01-04 18:49_

---

_Comment by @udjuraev-ipa on 2026-01-07 14:00_

In my case I was trying to install all CUDA versions, however the torchvision kept being installed using the CPU-version that did work well with `transformers`. What I ended up doing is `UV_TORCH_BACKEND=cu130 uv add torch torchvision`. 

---

_Comment by @appleparan on 2026-01-08 21:56_

Try [this](https://github.com/astral-sh/uv/issues/16386#issuecomment-3726000085)

---
