---
number: 10693
title: "PyTorch nightly inconsistency between `uv pip install` vs `uv sync` (missing nvidia packages)"
type: issue
state: open
author: tmm1
labels:
  - lock
assignees: []
created_at: 2025-01-16T20:06:07Z
updated_at: 2025-02-21T21:50:48Z
url: https://github.com/astral-sh/uv/issues/10693
synced_at: 2026-01-07T13:12:18-06:00
---

# PyTorch nightly inconsistency between `uv pip install` vs `uv sync` (missing nvidia packages)

---

_Issue opened by @tmm1 on 2025-01-16 20:06_

in https://github.com/astral-sh/uv/issues/2541#issuecomment-2487052583, `uv pip install` was fixed for torch-nightly:

```console
$ uv venv
$ uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu126
Resolved 27 packages in 637ms
Prepared 9 packages in 23.32s
Installed 27 packages in 42.07s
 + filelock==3.16.1
 + fsspec==2024.10.0
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.4.2
 + numpy==2.1.2
 + nvidia-cublas-cu12==12.6.4.1
 + nvidia-cuda-cupti-cu12==12.6.80
 + nvidia-cuda-nvrtc-cu12==12.6.77
 + nvidia-cuda-runtime-cu12==12.6.77
 + nvidia-cudnn-cu12==9.5.1.17
 + nvidia-cufft-cu12==11.3.0.4
 + nvidia-curand-cu12==10.3.7.77
 + nvidia-cusolver-cu12==11.7.1.2
 + nvidia-cusparse-cu12==12.5.4.2
 + nvidia-cusparselt-cu12==0.6.3
 + nvidia-nccl-cu12==2.21.5
 + nvidia-nvjitlink-cu12==12.6.85
 + nvidia-nvtx-cu12==12.6.77
 + pillow==11.0.0
 + pytorch-triton==3.2.0+git0d4682f0
 + sympy==1.13.1
 + torch==2.7.0.dev20250116+cu126
 + torchaudio==2.6.0.dev20250116+cu126
 + torchvision==0.22.0.dev20250116+cu126
 + typing-extensions==4.12.2
```

and in https://github.com/astral-sh/uv/issues/9651#issuecomment-2571833461, `uv sync` was fixed for torch-nightly:

```toml
[project]
name = "torchnightly"
requires-python = ">=3.11.9"
dependencies = [
    "torch",
    "torchao",
    "torchaudio",
    "torchvision",
]
dynamic = ["version"]

[tool.uv.sources]
torch = [{ index = "pytorch-nightly-cu126" }]
torchvision = [{ index = "pytorch-nightly-cu126" }]
torchaudio = [{ index = "pytorch-nightly-cu126" }]
torchao = [{ index = "pytorch-nightly-cu126" }]

[[tool.uv.index]]
name = "pytorch-nightly-cu126"
url = "https://download.pytorch.org/whl/nightly/cu126"
explicit = true
```

```console
$ uv sync
Using CPython 3.11.9
Creating virtual environment at: .venv
⠋ Resolving dependencies...
warning: Missing version constraint (e.g., a lower bound) for `torch`
warning: Missing version constraint (e.g., a lower bound) for `torchao`
warning: Missing version constraint (e.g., a lower bound) for `torchaudio`
warning: Missing version constraint (e.g., a lower bound) for `torchvision`
Resolved 19 packages in 8.01s
Prepared 3 packages in 1m 03s
Installed 14 packages in 38.06s
 + filelock==3.16.1
 + fsspec==2024.12.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + numpy==2.2.1
 + pillow==11.1.0
 + sympy==1.13.1
 + torch==2.7.0.dev20250116+cu126
 + torchao==0.8.0.dev20250116+cu126
 + torchaudio==2.6.0.dev20250116+cu126
 + torchvision==0.22.0.dev20250116+cu126
 + typing-extensions==4.12.2
```

however, notice how both produce different results

```diff
-Installed 27 packages in 42.07s
+Installed 14 packages in 38.06s
```

in the `uv sync`/`pyproject.toml` version, the nvidia packages are missing causing runtime failures.

cc @charliermarsh https://github.com/astral-sh/uv/issues/10119#issuecomment-2595958499

---

_Comment by @zanieb on 2025-01-16 20:09_

I'm not surprised these are different.

In the first, you set `--index-url https://download.pytorch.org/whl/nightly/cu126` which makes that the default index for all packages.

In the latter, you set

```
[tool.uv.sources]
torch = [{ index = "pytorch-nightly-cu126" }]
torchvision = [{ index = "pytorch-nightly-cu126" }]
torchaudio = [{ index = "pytorch-nightly-cu126" }]
torchao = [{ index = "pytorch-nightly-cu126" }]

[[tool.uv.index]]
name = "pytorch-nightly-cu126"
url = "https://download.pytorch.org/whl/nightly/cu126"
explicit = true
```

which only uses that index for those exact packages due to `explicit = true`.

The first also performs single-platform resolution while the latter does a universal resolution.

Can you share your lockfile?

What platform are you on?

Can you reproduce in a Dockerfile?

---

_Comment by @tmm1 on 2025-01-16 20:48_

I'm on linux. Not using docker. Removing `explicit = true` didn't make a difference.

<details>
<summary>uv.lock</summary>

```
version = 1
requires-python = ">=3.11.9"
resolution-markers = [
    "python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "(python_full_version >= '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version >= '3.12' and sys_platform != 'linux')",
    "python_full_version >= '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "python_full_version < '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "(python_full_version < '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version < '3.12' and sys_platform != 'linux')",
    "python_full_version < '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
]

[[package]]
name = "filelock"
version = "3.16.1"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/filelock-3.16.1-py3-none-any.whl" },
]

[[package]]
name = "fsspec"
version = "2024.10.0"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/fsspec-2024.10.0-py3-none-any.whl" },
]

[[package]]
name = "jinja2"
version = "3.1.4"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
dependencies = [
    { name = "markupsafe" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/Jinja2-3.1.4-py3-none-any.whl" },
    { url = "https://download.pytorch.org/whl/nightly/jinja2-3.1.4-py3-none-any.whl" },
]

[[package]]
name = "markupsafe"
version = "2.1.5"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
sdist = { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5.tar.gz" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_universal2.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp311-cp311-macosx_10_9_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl" },
    { url = "https://download.pytorch.org/whl/nightly/MarkupSafe-2.1.5-cp312-cp312-win_amd64.whl" },
]

[[package]]
name = "mpmath"
version = "1.3.0"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/mpmath-1.3.0-py3-none-any.whl" },
]

[[package]]
name = "networkx"
version = "3.4.2"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/networkx-3.4.2-py3-none-any.whl" },
]

[[package]]
name = "numpy"
version = "2.1.2"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-macosx_10_9_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-macosx_14_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-macosx_14_0_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-macosx_10_13_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-macosx_14_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-macosx_14_0_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp312-cp312-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-macosx_10_13_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-macosx_14_0_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313t-macosx_10_13_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313t-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313t-macosx_14_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313t-macosx_14_0_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/numpy-2.1.2-cp313-cp313t-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
]

[[package]]
name = "pillow"
version = "11.0.0"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-macosx_10_10_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp311-cp311-win_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-macosx_10_13_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp312-cp312-win_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-macosx_10_13_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313-win_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313t-macosx_10_13_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313t-macosx_11_0_arm64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313t-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313t-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313t-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/pillow-11.0.0-cp313-cp313t-win_arm64.whl" },
]

[[package]]
name = "setuptools"
version = "70.2.0"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/setuptools-70.2.0-py3-none-any.whl" },
]

[[package]]
name = "sympy"
version = "1.13.1"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
dependencies = [
    { name = "mpmath" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/sympy-1.13.1-py3-none-any.whl" },
]

[[package]]
name = "torch"
version = "2.7.0.dev20250116+cu126"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "setuptools", marker = "python_full_version >= '3.12'" },
    { name = "sympy" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp312-cp312-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp312-cp312-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp312-cp312-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313t-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313t-manylinux_2_28_x86_64.whl" },
]

[[package]]
name = "torchao"
version = "0.8.0.dev20250116"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
resolution-markers = [
    "(python_full_version >= '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version >= '3.12' and sys_platform != 'linux')",
    "python_full_version >= '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version < '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version < '3.12' and sys_platform != 'linux')",
    "python_full_version < '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchao-0.8.0.dev20250116-py3-none-any.whl" },
]

[[package]]
name = "torchao"
version = "0.8.0.dev20250116+cu126"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
resolution-markers = [
    "python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "python_full_version < '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchao-0.8.0.dev20250116%2Bcu126-cp39-abi3-manylinux_2_24_x86_64.manylinux_2_28_x86_64.whl" },
]

[[package]]
name = "torchaudio"
version = "2.6.0.dev20250116"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
resolution-markers = [
    "python_full_version >= '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "python_full_version < '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
]
dependencies = [
    { name = "torch", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116-cp311-cp311-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116-cp312-cp312-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116-cp313-cp313-linux_aarch64.whl" },
]

[[package]]
name = "torchaudio"
version = "2.6.0.dev20250116+cu126"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
resolution-markers = [
    "python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "(python_full_version >= '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version >= '3.12' and sys_platform != 'linux')",
    "python_full_version < '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "(python_full_version < '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version < '3.12' and sys_platform != 'linux')",
]
dependencies = [
    { name = "torch", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116%2Bcu126-cp311-cp311-linux_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116%2Bcu126-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116%2Bcu126-cp312-cp312-linux_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116%2Bcu126-cp312-cp312-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116%2Bcu126-cp313-cp313-linux_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchaudio-2.6.0.dev20250116%2Bcu126-cp313-cp313-win_amd64.whl" },
]

[[package]]
name = "torchnightly"
version = "0.0.0"
source = { virtual = "." }
dependencies = [
    { name = "torch" },
    { name = "torchao", version = "0.8.0.dev20250116", source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }, marker = "platform_machine != 'x86_64' or sys_platform != 'linux'" },
    { name = "torchao", version = "0.8.0.dev20250116+cu126", source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }, marker = "platform_machine == 'x86_64' and sys_platform == 'linux'" },
    { name = "torchaudio", version = "2.6.0.dev20250116", source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }, marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "torchaudio", version = "2.6.0.dev20250116+cu126", source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }, marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "torchvision", version = "0.22.0.dev20250116", source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }, marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "torchvision", version = "0.22.0.dev20250116+cu126", source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }, marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
]

[package.metadata]
requires-dist = [
    { name = "torch", index = "https://download.pytorch.org/whl/nightly/cu126" },
    { name = "torchao", index = "https://download.pytorch.org/whl/nightly/cu126" },
    { name = "torchaudio", index = "https://download.pytorch.org/whl/nightly/cu126" },
    { name = "torchvision", index = "https://download.pytorch.org/whl/nightly/cu126" },
]

[[package]]
name = "torchvision"
version = "0.22.0.dev20250116"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
resolution-markers = [
    "python_full_version >= '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "python_full_version < '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
]
dependencies = [
    { name = "numpy", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "pillow", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
    { name = "torch", marker = "platform_machine == 'aarch64' and sys_platform == 'linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116-cp311-cp311-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116-cp312-cp312-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116-cp313-cp313-linux_aarch64.whl" },
]

[[package]]
name = "torchvision"
version = "0.22.0.dev20250116+cu126"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
resolution-markers = [
    "python_full_version >= '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "(python_full_version >= '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version >= '3.12' and sys_platform != 'linux')",
    "python_full_version < '3.12' and platform_machine == 'x86_64' and sys_platform == 'linux'",
    "(python_full_version < '3.12' and platform_machine != 'aarch64' and platform_machine != 'x86_64') or (python_full_version < '3.12' and sys_platform != 'linux')",
]
dependencies = [
    { name = "numpy", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "pillow", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
    { name = "torch", marker = "platform_machine != 'aarch64' or sys_platform != 'linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116%2Bcu126-cp311-cp311-linux_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116%2Bcu126-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116%2Bcu126-cp312-cp312-linux_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116%2Bcu126-cp312-cp312-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116%2Bcu126-cp313-cp313-linux_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torchvision-0.22.0.dev20250116%2Bcu126-cp313-cp313-win_amd64.whl" },
]

[[package]]
name = "typing-extensions"
version = "4.12.2"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/typing_extensions-4.12.2-py3-none-any.whl" },
]
```
</details>

vs

<details>
<summary>uv pip freeze</summary>

```
filelock==3.16.1
fsspec==2024.10.0
jinja2==3.1.4
markupsafe==2.1.5
mpmath==1.3.0
networkx==3.4.2
numpy==2.1.2
nvidia-cublas-cu12==12.6.4.1
nvidia-cuda-cupti-cu12==12.6.80
nvidia-cuda-nvrtc-cu12==12.6.77
nvidia-cuda-runtime-cu12==12.6.77
nvidia-cudnn-cu12==9.5.1.17
nvidia-cufft-cu12==11.3.0.4
nvidia-curand-cu12==10.3.7.77
nvidia-cusolver-cu12==11.7.1.2
nvidia-cusparse-cu12==12.5.4.2
nvidia-cusparselt-cu12==0.6.3
nvidia-nccl-cu12==2.21.5
nvidia-nvjitlink-cu12==12.6.85
nvidia-nvtx-cu12==12.6.77
pillow==11.0.0
pytorch-triton==3.2.0+git0d4682f0
sympy==1.13.1
torch==2.7.0.dev20250116+cu126
torchaudio==2.6.0.dev20250116+cu126
torchvision==0.22.0.dev20250116+cu126
typing-extensions==4.12.2
```
</details>

---

_Comment by @zanieb on 2025-01-16 21:37_

Sharing a reproduction with a Dockerfile would be useful for us to isolate the issue.

Anyway... I can reproduce this on my x86-64 Linux machine

During `uv pip install`, it looks like those nvidia dependencies from `torch`

```
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.6.77, <12.6.77+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.6.77, <12.6.77+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.6.80, <12.6.80+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=9.5.1.17, <9.5.1.17+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.6.4.1, <12.6.4.1+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.3.0.4, <11.3.0.4+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=10.3.7.77, <10.3.7.77+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.7.1.2, <11.7.1.2+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.5.4.2, <12.5.4.2+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-cusparselt-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=0.6.3, <0.6.3+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=2.21.5, <2.21.5+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.6.77, <12.6.77+
DEBUG Adding transitive dependency for torch==2.7.0.dev20250116+cu126: nvidia-nvjitlink-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.6.85, <12.6.85+
```

These dependencies are missing from torch in the lockfile

```
[[package]]
name = "torch"
version = "2.7.0.dev20250116+cu126"
source = { registry = "https://download.pytorch.org/whl/nightly/cu126" }
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "setuptools", marker = "python_full_version >= '3.12'" },
    { name = "sympy" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp312-cp312-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp312-cp312-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp312-cp312-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313-manylinux_2_28_x86_64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313-win_amd64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313t-linux_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313t-manylinux_2_28_x86_64.whl" },
]
```

This occurs because pytorch has different metadata depending on the wheel.

e.g., the x86-64 wheel we use in `uv pip`

```
❯ wget https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313t-manylinux_2_28_x86_64.whl
❯ unzip torch-2.7.0.dev20250116+cu126-cp313-cp313t-manylinux_2_28_x86_64.whl
❯ cat ./torch-2.7.0.dev20250116+cu126.dist-info/METADATA | grep Requires-Dist
Requires-Dist: filelock
Requires-Dist: typing-extensions>=4.10.0
Requires-Dist: setuptools; python_version >= "3.12"
Requires-Dist: sympy==1.13.1; python_version >= "3.9"
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Requires-Dist: nvidia-cuda-nvrtc-cu12==12.6.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cuda-runtime-cu12==12.6.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cuda-cupti-cu12==12.6.80; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cudnn-cu12==9.5.1.17; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cublas-cu12==12.6.4.1; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cufft-cu12==11.3.0.4; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-curand-cu12==10.3.7.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusolver-cu12==11.7.1.2; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusparse-cu12==12.5.4.2; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusparselt-cu12==0.6.3; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nccl-cu12==2.21.5; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nvtx-cu12==12.6.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nvjitlink-cu12==12.6.85; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: pytorch-triton==3.2.0+git0d4682f0; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: optree>=0.13.0; extra == "optree"
Requires-Dist: opt-einsum>=3.3; extra == "opt-einsum"
```

But when we lock, we use an arbitrary wheel because it's a universal resolution

```
❯ uv lock -v 2>&1 | grep torch-2.7.0.dev20250116
DEBUG Selecting: torch==2.7.0.dev20250116+cu126 [compatible] (torch-2.7.0.dev20250116+cu126-cp311-cp311-linux_aarch64.whl)
❯ wget https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp311-cp311-linux_aarch64.whl
❯ unzip -q torch-2.7.0.dev20250116+cu126-cp311-cp311-linux_aarch64.whl
❯ cat torch-2.7.0.dev20250116+cu126.dist-info/METADATA | grep Requires-Dist
Requires-Dist: filelock
Requires-Dist: typing-extensions>=4.10.0
Requires-Dist: setuptools; python_version >= "3.12"
Requires-Dist: sympy==1.13.1; python_version >= "3.9"
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Requires-Dist: optree>=0.13.0; extra == "optree"
Requires-Dist: opt-einsum>=3.3; extra == "opt-einsum"
```

Frankly, it's really not ideal that they change their dependencies from wheel to wheel.

---

_Label `lock` added by @zanieb on 2025-01-16 21:39_

---

_Comment by @tmm1 on 2025-01-16 21:42_

Very interesting. This is presumably because the nvidia libraries only exist in x64 and not arm64?

Maybe I can force x64 resolution in my project file.

---

_Comment by @zanieb on 2025-01-16 21:46_

Yeah presumably. They could still put the dependencies there with the same platform markers they're using in the other wheel though... 🤷‍♀ 

You can force use of the x86-64 wheel with

```toml
[tool.uv.sources]
torch = [{ url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.7.0.dev20250116%2Bcu126-cp313-cp313t-manylinux_2_28_x86_64.whl" }]
```

---

_Comment by @tmm1 on 2025-01-16 21:52_

Ick, I was hoping for something like:

```toml
[tool.uv]
environments = [
    "platform_arch == 'x86_64'",
]
```

Let me see if I can start a thread upstream, maybe they're amenable to fixing up the wheels.

EDIT: I see now this was the same conclusion reached by @charliermarsh in https://github.com/astral-sh/uv/issues/10119#issuecomment-2559898792

---

_Comment by @tmm1 on 2025-01-16 23:33_

> Let me see if I can start a thread upstream, maybe they're amenable to fixing up the wheels.

The change upstream is pretty trivial, and root cause here may just be an oversight.

Funny it appears the cpu linux/arm64 wheels have the cuda dependencies listed.

---

_Referenced in [pytorch/pytorch#145021](../../pytorch/pytorch/pulls/145021.md) on 2025-01-17 00:12_

---

_Comment by @tmm1 on 2025-01-18 05:17_

I opened a PR on the pytorch repo to fix the metadata on the nightly wheels.

In the mean time, I had to specify the dependencies explicitly to get things working:

```toml
[project.optional-dependencies]
nvidia = [
    # workaround for pytorch/pytorch#145021 and astral-sh/uv#10693
    "nvidia-cuda-nvrtc-cu12 == 12.6.77",
    "nvidia-cuda-runtime-cu12 == 12.6.77",
    "nvidia-cuda-cupti-cu12 == 12.6.80",
    "nvidia-cudnn-cu12 == 9.5.1.17",
    "nvidia-cublas-cu12 == 12.6.4.1",
    "nvidia-cufft-cu12 == 11.3.0.4",
    "nvidia-curand-cu12 == 10.3.7.77",
    "nvidia-cusolver-cu12 == 11.7.1.2",
    "nvidia-cusparse-cu12 == 12.5.4.2",
    "nvidia-cusparselt-cu12 == 0.6.3",
    "nvidia-nccl-cu12 == 2.21.5",
    "nvidia-nvtx-cu12 == 12.6.77",
    "nvidia-nvjitlink-cu12 == 12.6.85",
]
torch = [
    "torch",
    "torchao",
    "torchaudio",
    "torchvision",
]

[tool.uv.sources]
torch = [{ index = "pytorch-nightly-cu126" }]
torchvision = [{ index = "pytorch-nightly-cu126" }]
torchaudio = [{ index = "pytorch-nightly-cu126" }]
torchao = [{ index = "pytorch-nightly-cu126" }]
nvidia-cuda-nvrtc-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cuda-runtime-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cuda-cupti-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cudnn-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cublas-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cufft-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-curand-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cusolver-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cusparse-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-cusparselt-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-nccl-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-nvtx-cu12 = [{ index = "pytorch-nightly-cu126" }]
nvidia-nvjitlink-cu12 = [{ index = "pytorch-nightly-cu126" }]

[[tool.uv.index]]
name = "pytorch-nightly-cu126"
url = "https://download.pytorch.org/whl/nightly/cu126"
explicit = true
```

---

_Comment by @charliermarsh on 2025-01-18 15:28_

Nice -- thank you for doing that.

---

_Comment by @warner-benjamin on 2025-02-21 21:50_

This is also an issue with the released PyTorch 2.6.0 Cuda 12.6 on x86. Maybe it's related to pytorch/pytorch#146679? If so, then it should be resolved for PyTorch 2.6.1.

@tmm1's [solution](https://github.com/astral-sh/uv/issues/10693#issuecomment-2599538154) resolves the issue.

---

_Referenced in [pytorch/pytorch#146679](../../pytorch/pytorch/issues/146679.md) on 2025-02-21 21:53_

---

_Referenced in [astral-sh/uv#12774](../../astral-sh/uv/issues/12774.md) on 2025-04-15 12:39_

---

_Referenced in [chronicler-ai/chronicle#124](../../chronicler-ai/chronicle/pulls/124.md) on 2025-10-14 04:57_

---
