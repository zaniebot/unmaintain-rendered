```yaml
number: 11411
title: Pytorch install not working on linux
type: issue
state: closed
author: spkgyk
labels:
  - bug
assignees: []
created_at: 2025-02-10T23:31:18Z
updated_at: 2025-02-11T00:12:13Z
url: https://github.com/astral-sh/uv/issues/11411
synced_at: 2026-01-12T16:00:36Z
```

# Pytorch install not working on linux

---

_@spkgyk_

### Summary

Here is my config:

```
[project]
name = "project-name"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "monai>=1.4.0",
    "torch>=2.6.0",
    "torchaudio>=2.6.0",
    "torchvision>=0.21.0",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu126", marker = "sys_platform == 'win32' or sys_platform == 'linux'" },
]
torchvision = [
  { index = "pytorch-cu126", marker = "sys_platform == 'win32' or sys_platform == 'linux'" },
]

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

When i run this, and import torch, i get the error 

```
uv sync && uv run local/test.py
Resolved 19 packages in 9ms
Audited 15 packages in 0.02ms
Traceback (most recent call last):
  File "/local/test.py", line 1, in <module>
    import torch
  File "/.venv/lib/python3.12/site-packages/torch/__init__.py", line 405, in <module>
    from torch._C import *  # noqa: F403
    ^^^^^^^^^^^^^^^^^^^^^^
ImportError: libcusparseLt.so.0: cannot open shared object file: No such file or directory
```

However, if my config is simply

```
[project]
name = "project-name"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "monai>=1.4.0",
    "torch>=2.6.0",
    "torchaudio>=2.6.0",
    "torchvision>=0.21.0",
]
```

Everything works no problem. I believe I have followed the docs correctly, so I'm not sure why this doesn't work. Is anyone else experiencing this issue?

I noticed that when I run the simplified config without the index url, I get all the correct nvidia deps installing:
```
nvidia-cusolver-cu12 ------------------------------ 98.70 MiB/122.01 MiB
nvidia-cusparselt-cu12 ------------------------------ 98.64 MiB/143.11 MiB
nvidia-nccl-cu12 ------------------------------ 99.87 MiB/179.91 MiB
nvidia-cusparse-cu12 ------------------------------ 99.51 MiB/197.84 MiB
nvidia-cufft-cu12 ------------------------------ 100.21 MiB/201.66 MiB
triton     ------------------------------ 98.60 MiB/241.43 MiB
nvidia-cublas-cu12 ------------------------------ 99.00 MiB/346.60 MiB
nvidia-cudnn-cu12 ------------------------------ 98.57 MiB/633.96 MiB
etc
```

### Platform

Linux 6.8.0-1020-gcp x86_64 GNU/Linux

### Version

uv 0.5.29

### Python version

Python 3.12.3

---

_Label `bug` added by @spkgyk on 2025-02-10 23:31_

---

_Comment by @charliermarsh on 2025-02-10 23:33_

I'm guessing you might have a different CUDA version installed? I think the PyPI wheels are CUDA 12.4.

---

_Comment by @spkgyk on 2025-02-10 23:36_

wget https://developer.download.nvidia.com/compute/cuda/12.8.0/local_installers/cuda_12.8.0_570.86.10_linux.run
This is the NVIDIA drivers used.

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cudnn
This is the cudnn version used

I think it's strange that specifying the tool.uv.index causes things to fail

---

_Comment by @charliermarsh on 2025-02-10 23:38_

I'm a bit confused. Using `tool.uv.index` results in you downloading and using different wheels. It makes sense that it would fail -- it's not the same package.

---

_Comment by @spkgyk on 2025-02-10 23:42_

I have cuda 12.8 installed, surely if I don't specify the wheel it uses 12.6 - the closest version - by default? 

---

_Comment by @charliermarsh on 2025-02-10 23:43_

No, the Python packaging ecosystem doesn't know anything about your installed CUDA version. The PyTorch team can only upload wheels to PyPI that are compatible with a single CUDA version, and it looks like they publish for 12.4. That's why they have their own indexes: to publish wheels for more accelerator variants.

---

_Comment by @spkgyk on 2025-02-10 23:44_

https://pytorch.org/

the publish for 12.6. their install command for pip is:

pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126

---

_Comment by @charliermarsh on 2025-02-10 23:48_

That command downloads from the PyTorch index, not from PyPI. It's similar to your first `pyproject.toml`, and not the same as your second `pyproject.toml`.

---

_Comment by @spkgyk on 2025-02-10 23:53_

I see, so the second pyproject.toml is downloading from PyPI, and that correctly ships the nvidia dependencies, but the PytorchIndex doesn't do this correctly and that's why it fails. Thanks for the help! 

Interestingly, if i use this pyproject.toml:

```
[project]
name = "inference-server"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "monai>=1.4.0",
    "torch>=2.6.0",
    "torchaudio>=2.6.0",
    "torchvision>=0.21.0",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu124", marker = "sys_platform == 'win32' or sys_platform == 'linux'" },
]
torchvision = [
  { index = "pytorch-cu124", marker = "sys_platform == 'win32' or sys_platform == 'linux'" },
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

```

I don't have any issues - I guess the 12.6 version is a little buggy as it's new. Thank you for the help!

---

_Closed by @spkgyk on 2025-02-10 23:53_

---

_Comment by @charliermarsh on 2025-02-11 00:12_

No worries, happy to help!

---
