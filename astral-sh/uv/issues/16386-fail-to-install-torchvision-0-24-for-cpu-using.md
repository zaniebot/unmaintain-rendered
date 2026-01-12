```yaml
number: 16386
title: Fail to install torchvision 0.24 for CPU using accelerator extras (because no wheels for manylinux x86_64 can be found)
type: issue
state: open
author: nathanpainchaud
labels:
  - question
assignees: []
created_at: 2025-10-21T13:37:48Z
updated_at: 2026-01-09T23:52:07Z
url: https://github.com/astral-sh/uv/issues/16386
synced_at: 2026-01-12T16:02:30Z
```

# Fail to install torchvision 0.24 for CPU using accelerator extras (because no wheels for manylinux x86_64 can be found)

---

_@nathanpainchaud_

### Summary

## Description
Updating from torch 2.8 and torchvision 0.23 to torch 2.9 and torchvision 0.24 and following [uv's documentation on how to configure accelerators with optional dependencies](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies), I ran into the error below when trying to install a project for CPU:
```console
Distribution `torchvision==0.24.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_42_x86_64`), but `torchvision` (v0.24.0) only has wheels for the following platforms: `manylinux_2_28_aarch64`, `macosx_11_0_arm64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

The message seems clear that wheels are not available for my platform (manylinux x86_64), but manually inspecting [PyTorch's index of torchvision packages](https://download.pytorch.org/whl/torchvision/) reveals that a compatible wheel should be available: [`torchvision-0.24.0+cpu-cp312-cp312-manylinux_2_28_x86_64.whl`](https://download.pytorch.org/whl/cpu/torchvision-0.24.0%2Bcpu-cp312-cp312-manylinux_2_28_x86_64.whl#sha256=baa37f80155a3b055911d5c2418be1e16b95bb69f671dbb0d07d67504093e23d).

Therefore, I suspect there is an issue with how uv resolves the package (e.g. handling of markers?) which incorrectly leads it to believe there are no wheels available for this platform. I didn't have time to follow uv's resolution mechanism further to identify a more precise root cause.

## How to Reproduce
I first ran into the issue in a more complex project, but I managed to reproduce it in the following simple setup.

1. Create a new project by running `uv init PROJECT_NAME`, with managed Python `cpython-3.12.9-linux-x86_64-gnu` already installed
2. Copy the following content into the `pyproject.toml` (following [uv's documentation on how to configure accelerators with optional dependencies](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies)):
```toml
[project]
name = "PROJECT_NAME"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
[project.optional-dependencies]
cpu = [
  "torch>=2.9,<2.10",
  "torchvision>=0.24,<0.25",
]
cu126 = [
  "torch>=2.9,<2.10",
  "torchvision>=0.24,<0.25",
]
cu128 = [
  "torch>=2.9,<2.10",
  "torchvision>=0.24,<0.25",
]
cu130 = [
  "torch>=2.9,<2.10",
  "torchvision>=0.24,<0.25",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu126" },
    { extra = "cu128" },
    { extra = "cu130" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu126", extra = "cu126" },
  { index = "pytorch-cu128", extra = "cu128" },
  { index = "pytorch-cu130", extra = "cu130" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu126", extra = "cu126" },
  { index = "pytorch-cu128", extra = "cu128" },
  { index = "pytorch-cu130", extra = "cu130" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu130"
url = "https://download.pytorch.org/whl/cu130"
explicit = true
```
3. Try to install the cpu version with `uv sync --extra cpu` and you should see the error above


## More Context
* As mentioned at the beginning, the issue appeared when updating torch 2.8->2.9 and torchvision 0.23->0.24 (and adapting the `pyproject.toml` to drop `cu129` and add `cu130`). The same configuration for dependencies (minus the differences in CUDA target versions) was working fine before.
* As far as I could tell, I did not see any changes in the availability of wheels published to PyTorch's index with this update that could explain the lack of support for my platform.
* The error only happens when installing to target CPU, and the project installs fine when targeting GPU (e.g. `uv sync --extra cu126`)

### Platform

Linux 6.17.2-arch1-1 x86_64 GNU/Linux

### Version

uv 0.9.0

### Python version

Python 3.12.9

---

_Label `bug` added by @nathanpainchaud on 2025-10-21 13:37_

---

_Comment by @konstin on 2025-10-22 17:07_

Raised this upstream, we'll figure out if it's on the torch or uv side: https://github.com/pytorch/vision/issues/9249

---

_Comment by @nathanpainchaud on 2025-10-22 21:07_

Thanks @konstin for taking care of this. I've looked at the issue you opened with pytorch/vision, and I just want to point out some things I've observed:
- There are indeed no local (no `+`) versions of torchvision in the index for torchvision 0.24.0. Howevere, there are no such versions for torchvision<0.24 either, but uv was still able to resolve the environment for both cpu/gpu;
- I am not seeing the duplicate versions of the `manylinux_2_28_aarch64` wheels. The different versions of the wheels I'm seeing are for different python versions (3.10-3.14, with 3.13 and 3.14 also having free-threaded versions);
- Some torchvision wheels seem to have a hash after the `+`, and I've got no ideas what platform these wheels are supposed to target. Maybe that's an indicator of a bug on torch's side?

In any case, thanks again for picking this up, as it goes beyond my abilities to diagnose environment setup issues 😅 

---

_Comment by @ghpu on 2025-11-04 10:44_

There are now +cpu versions for torchvision 0.24.0 , but also +e437e35 versions of torchvision 0.24.0 only for Windows. When uv resolves torchvision 0.24.0, it selects the "latest" version (+e437e35) instead of the +cpu suffix version.

---

_Comment by @YoniChechik on 2025-11-18 11:48_

this hack fixed it for me: https://github.com/pilipolio/chess-sandbox/commit/67a773aa9fba945290e54fce062ca052d268e5d7

---

_Comment by @appleparan on 2026-01-08 21:56_

This is a `torchvision` wheel issue, not a `uv` issue.

Here is my fix by adding local version specifier `+cpu`. [It works!](https://github.com/appleparan/copier-modern-ml/blob/main/project/pyproject.toml.jinja)


```
[project.optional-dependencies]
cu130 = [
  "torch==2.9.1",
  "torchvision==0.24.1",
]
cu128 = [
  "torch==2.9.1",
  "torchvision==0.24.1",
]
cu126 = [
  "torch==2.9.1",
  "torchvision==0.24.1",
]
cpu = [
  "torch==2.9.1",
  # Linux x86_64
  "torchvision==0.24.1+cpu; sys_platform == 'linux' and platform_machine == 'x86_64'",
  # Windows x86_64 (AMD64)
  "torchvision==0.24.1+cpu; sys_platform == 'win32' and platform_machine == 'AMD64'",
  # Linux aarch64, macOS arm64: PyPI default wheel
  "torchvision==0.24.1; (sys_platform == 'linux' and platform_machine == 'aarch64') or (sys_platform == 'darwin' and platform_machine == 'arm64')",
]
```

---

_Label `bug` removed by @konstin on 2026-01-09 10:43_

---

_Label `question` added by @konstin on 2026-01-09 10:43_

---
