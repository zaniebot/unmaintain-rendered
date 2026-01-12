```yaml
number: 929
title: Use tempfile to prevent install io race crashes
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/add-locks-on-installation-fixup-to-avoid-rages
created_at: 2024-01-15T10:24:41Z
updated_at: 2024-01-16T21:07:40Z
url: https://github.com/astral-sh/uv/pull/929
synced_at: 2026-01-12T16:04:18Z
```

# Use tempfile to prevent install io race crashes

---

_@konstin_

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

We switch to a new strategy: When the target files exists, we don't remove it, but instead hardlink the source file to a tempfile first, then renaming the tempfile to the target file. Renaming is considered an atomic operation.

I've put the logging on debug level because they cases indicate a conflict between two packages while being rare. 

Closes #925

---

_Label `bug` added by @konstin on 2024-01-15 10:24_

---

_Converted to draft by @konstin on 2024-01-15 10:50_

---

_@charliermarsh reviewed on 2024-01-15 14:44_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:409 on 2024-01-15 14:44_

Could we instead: hardlink to a temporary file, then rename the temporary file to the target? That should be safe and doesn't require any deletes.

---

_Renamed from "Add locks to prevent install fallback race condition" to "Use tempfile to prevent install io race crashes" by @konstin on 2024-01-16 10:37_

---

_Comment by @konstin on 2024-01-16 10:39_

I've changed the strategy to tempfile-hardlink-and-rename and updated the description.

---

_@charliermarsh reviewed on 2024-01-16 17:45_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:430 on 2024-01-16 17:45_

Make this an `.expect`?

---

_@charliermarsh reviewed on 2024-01-16 17:47_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:430 on 2024-01-16 17:47_

We could perhaps do https://docs.rs/tempfile/latest/tempfile/struct.NamedTempFile.html to avoid creating a dir... but don't feel strongly.

---

_@charliermarsh approved on 2024-01-16 17:47_

---

_@charliermarsh reviewed on 2024-01-16 17:47_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:430 on 2024-01-16 17:47_

What you have is probably more secure.

---

_Marked ready for review by @charliermarsh on 2024-01-16 21:02_

---

_Merged by @charliermarsh on 2024-01-16 21:07_

---

_Closed by @charliermarsh on 2024-01-16 21:07_

---

_Branch deleted on 2024-01-16 21:07_

---
