---
number: 15098
title: "UV doesn't recognize torch in .venv"
type: issue
state: closed
author: xiaoheiyue
labels:
  - bug
assignees: []
created_at: 2025-08-06T03:41:07Z
updated_at: 2025-08-06T06:06:47Z
url: https://github.com/astral-sh/uv/issues/15098
synced_at: 2026-01-07T13:12:19-06:00
---

# UV doesn't recognize torch in .venv

---

_Issue opened by @xiaoheiyue on 2025-08-06 03:41_

### Summary

uv pip install --upgrade auto_gptq
Resolved 66 packages in 1.25s
  × Failed to build `auto-gptq==0.7.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      Building cuda extension requires PyTorch (>=1.13.0) being installed, please install PyTorch first: No
      module named 'torch'

      hint: This usually indicates a problem with the package or the build environment.

uv pip show torch
Name: torch
Version: 2.9.0.dev20250805+cu129
Location: /home/qyht/projects/Quantization/.venv/lib/python3.12/site-packages
Requires: filelock, fsspec, jinja2, networkx, nvidia-cublas-cu12, nvidia-cuda-cupti-cu12, nvidia-cuda-nvrtc-cu12, nvidia-cuda-runtime-cu12, nvidia-cudnn-cu12, nvidia-cufft-cu12, nvidia-cufile-cu12, nvidia-curand-cu12, nvidia-cusolver-cu12, nvidia-cusparse-cu12, nvidia-cusparselt-cu12, nvidia-nccl-cu12, nvidia-nvjitlink-cu12, nvidia-nvshmem-cu12, nvidia-nvtx-cu12, pytorch-triton, setuptools, sympy, typing-extensions
Required-by: torchaudio, torchvision


### Platform

Ubuntu 24.04 x86_64

### Version

uv 0.8.5

### Python version

python 3.12.3

---

_Label `bug` added by @xiaoheiyue on 2025-08-06 03:41_

---

_Closed by @xiaoheiyue on 2025-08-06 06:06_

---
