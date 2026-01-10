```yaml
number: 10399
title: How should I handle cross compatibility with the torch dependency?
type: issue
state: open
author: MattTheCuber
labels: []
assignees: []
created_at: 2025-01-08T16:30:20Z
updated_at: 2025-01-24T16:12:07Z
url: https://github.com/astral-sh/uv/issues/10399
synced_at: 2026-01-10T04:27:58Z
```

# How should I handle cross compatibility with the torch dependency?

---

_Issue opened by @MattTheCuber on 2025-01-08 16:30_

Note, this seems to be an question with all Python dependency tools. However, I wasn't able to find any documentation on the problem online.

I use the uv pip method with `requirements.txt`. When I add `torch` to my `pyproject.toml` -> `project.dependencies`, then run `uv pip compile`, it creates a `requirements.txt` with a bunch of Linux-specific dependencies for nvidia packages such as `nvidia-cublas-cu12` and `nvidia-nccl-cu12`. This works and makes sense for Linux. However, if you are building a cross-platform compatible tool, this does not work on Windows. When running `uv pip install torch` on Windows, those nvidia dependencies are not installed. This makes `uv pip sync` fail on Windows.

The only solutions I came up with are to remove the dependencies from the requirements file manually or use `--no-emit-package`.

---

_Comment by @MattTheCuber on 2025-01-08 16:45_

This is what I mean for the `--no-emit-package`.

```toml
[tool.uv.pip]
no-emit-package = [
    "nvidia-cublas-cu12",
    "nvidia-cuda-cupti-cu12",
    "nvidia-cuda-nvrtc-cu12",
    "nvidia-cuda-runtime-cu12",
    "nvidia-cudnn-cu12",
    "nvidia-cufft-cu12",
    "nvidia-curand-cu12",
    "nvidia-cusolver-cu12",
    "nvidia-cusparse-cu12",
    "nvidia-nccl-cu12",
    "nvidia-nvjitlink-cu12",
    "nvidia-nvtx-cu12",
]
```

It works, but I feel like their is a better way of handling this that I don't know about.

EDIT:
This does not work when re-running `pip sync` on Linux because those packages are required.

---

_Comment by @MattTheCuber on 2025-01-08 17:26_

I think this may work?

```toml
[project]
...
dependencies = [
    "nvidia-cublas-cu12; sys_platform=='linux'",
    "nvidia-cuda-cupti-cu12; sys_platform=='linux'",
    "nvidia-cuda-nvrtc-cu12; sys_platform=='linux'",
    "nvidia-cuda-runtime-cu12; sys_platform=='linux'",
    "nvidia-cudnn-cu12; sys_platform=='linux'",
    "nvidia-cufft-cu12; sys_platform=='linux'",
    "nvidia-curand-cu12; sys_platform=='linux'",
    "nvidia-cusolver-cu12; sys_platform=='linux'",
    "nvidia-cusparse-cu12; sys_platform=='linux'",
    "nvidia-nccl-cu12; sys_platform=='linux'",
    "nvidia-nvjitlink-cu12; sys_platform=='linux'",
    "nvidia-nvtx-cu12; sys_platform=='linux'",
]
```

Maybe as a constraint?

---

_Comment by @AKuederle on 2025-01-23 11:59_

This is because the torch packages differ based on the system you install it on. Have a look at the installation matrix on the official torch website (https://pytorch.org). The version of torch that you get on linux when using pypi as index is a version with GPU suppor tbuild against CUDA 12.4. However, the official Pypi version refers to the CPU only version when installed on Windows or MacOS.

To use a GPU enabled version of torch on windows (or a version build against a different cuda version), requires to install the packages from the official pytorch index instead of Pypi.

The uv guide on pytorch suggests some solutions for this: https://docs.astral.sh/uv/guides/integration/pytorch/



---

_Comment by @MattTheCuber on 2025-01-24 16:12_

This is great, thank you! I don't know why I didn't find that guide.

How does this work for `uv pip compile`? Do I need to compile requirements from each environment (with CUDA for Linux, with CPU for Windows, and a special one for CI using CPU)?

---
