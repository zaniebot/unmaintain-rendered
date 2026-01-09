---
number: 925
title: jax fails to install due to race condition
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-01-15T09:25:56Z
updated_at: 2024-01-16T21:07:40Z
url: https://github.com/astral-sh/uv/issues/925
synced_at: 2026-01-07T13:12:16-06:00
---

# jax fails to install due to race condition

---

_Issue opened by @konstin on 2024-01-15 09:25_

On ubuntu and python 3.10,

```
cargo run -q -- pip-install --find-links https://storage.googleapis.com/jax-releases/jax_cuda_releases.html "jax[cuda12_pip]==0.4.23"
```

non-deterministically but for me consistently fails to install with messages such as 

```
error: Failed to install: nvidia_nccl_cu12-2.19.3-py3-none-manylinux1_x86_64.whl (nvidia-nccl-cu12==2.19.3)
  Caused by: failed to remove file `/home/konsti/projects/puffin/.venv/lib/python3.10/site-packages/nvidia/__init__.py`
  Caused by: No such file or directory (os error 2)
```

```
error: Failed to install: nvidia_cublas_cu12-12.3.4.1-py3-none-manylinux1_x86_64.whl (nvidia-cublas-cu12==12.3.4.1)
  Caused by: Replacing an existing file or directory failed
```

```
error: Failed to install: nvidia_cuda_nvcc_cu12-12.3.107-py3-none-manylinux1_x86_64.whl (nvidia-cuda-nvcc-cu12==12.3.107)
  Caused by: failed to hardlink file from /home/konsti/.cache/puffin/wheels-v0/pypi/nvidia-cuda-nvcc-cu12/nvidia_cuda_nvcc_cu12-12.3.107-py3-none-manylinux1_x86_64/nvidia/__init__.py to /home/konsti/projects/puffin/.venv/lib/python3.10/site-packages/nvidia/__init__.py
  Caused by: File exists (os error 17)
```

We install a lot of nvidia package, that all contain `nvidia/__init__.py`, since they all install themselves into the `nvidia` module:

```
nvidia-cublas-cu12==12.3.4.1
nvidia-cuda-cupti-cu12==12.3.101
nvidia-cuda-nvcc-cu12==12.3.107
nvidia-cuda-nvrtc-cu12==12.3.107
nvidia-cuda-runtime-cu12==12.3.101
nvidia-cudnn-cu12==8.9.7.29
nvidia-cufft-cu12==11.0.12.1
nvidia-cusolver-cu12==11.5.4.101
nvidia-cusparse-cu12==12.2.0.103
nvidia-nccl-cu12==2.19.3
nvidia-nvjitlink-cu12==12.3.101
```

```
$  tree -L 1 .venv/lib/python3.10/site-packages/nvidia
.venv/lib/python3.10/site-packages/nvidia
├── cublas
├── cuda_cupti
├── cuda_nvcc
├── cuda_nvrtc
├── cuda_runtime
├── cudnn
├── cufft
├── cusolver
├── cusparse
├── __init__.py
├── nccl
└── nvjitlink
```

When installing we get a race condition, each package installation is its own thread:
* Installer Thread 1 creates `nvidia/__init__.py`
* Installer Thread 2 sees an existing  `nvidia/__init__.py`
* Installer Thread 3 sees an existing  `nvidia/__init__.py`
* Installer Thread 2 removes `nvidia/__init__.py`
* Installer Thread 3 tries to remove `nvidia/__init__.py`, it doesn't exist anymore -> failure.

We could ignore failures when removing the files, but then we would get the same error when trying to create the file again. Instead, we have to lock the remove-and-create step to avoid this TOCTOU problem. 

---

_Label `bug` added by @konstin on 2024-01-15 09:25_

---

_Renamed from "Installing nvidia_nccl_cu12-2.19.3-py3-none-manylinux1_x86_64.whl though jax fails" to "jax fails to install due to race condition" by @konstin on 2024-01-15 10:09_

---

_Referenced in [astral-sh/uv#929](../../astral-sh/uv/pulls/929.md) on 2024-01-15 10:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-16 05:53_

---

_Comment by @charliermarsh on 2024-01-16 05:53_

@konstin - I don't mind taking this one over.

---

_Assigned to @konstin by @charliermarsh on 2024-01-16 19:15_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-01-16 19:15_

---

_Closed by @charliermarsh on 2024-01-16 21:07_

---
