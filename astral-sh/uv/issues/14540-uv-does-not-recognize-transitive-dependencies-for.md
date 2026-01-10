---
number: 14540
title: "`uv` does not recognize transitive dependencies for optional PyTorch GPU dependency"
type: issue
state: closed
author: j-adamczyk
labels:
  - bug
  - external
assignees: []
created_at: 2025-07-10T12:34:02Z
updated_at: 2025-07-14T01:16:55Z
url: https://github.com/astral-sh/uv/issues/14540
synced_at: 2026-01-10T01:25:46Z
---

# `uv` does not recognize transitive dependencies for optional PyTorch GPU dependency

---

_Issue opened by @j-adamczyk on 2025-07-10 12:34_

### Summary

I have the following `pyproject.toml`, configuring PyTorch GPU for training and CPU for inference:
```
[project]
name = "ml"
version = "1.0.0"
description = ""
authors = [{ name = "ML team" }]
readme = "README.md"
requires-python = ">=3.12,<3.13"

dependencies = [
    "torch==2.6.0",
]

# Optional CPU/GPU variant of PyTorch
pytorch_variant = {train_text_models = "pytorch-gpu", inference = "pytorch-cpu" }

[dependency-groups]
dev = [
    "jupyter",
    "pre-commit",
    "pytest",
    "ruff"
]

train_text_models = [
    "datasets==3.*",
    "pandas==2.*",
    "pydantic-settings==2.*",
    "scikit-learn==1.7.*",
    "torch==2.6.*",
    "tqdm",
    "transformers[torch]==4.52.*"
]

inference = [
    "captum==0.8.*",
    "clean-text==0.6.*",
    "dill",
    "fastapi==0.115.*",
    "lightgbm==4.6.*",
    "opentelemetry-api==1.34.*",
    "opentelemetry-exporter-otlp==1.34.*",
    "opentelemetry-instrumentation-fastapi==0.55b1",
    "opentelemetry-sdk==1.34.*",
    "pandas==2.*",
    "pydantic-settings==2.*",
    "scikit-learn==1.7.*",
    "sentry-sdk[fastapi]==2.*",
    "shap==0.48.0",
    "statsforecast",
    "torch==2.6.*",
    "transformers[torch]==4.52.*",
    "uvicorn==0.34.*"
]

[project.optional-dependencies]
pytorch-cpu = ["torch==2.6.*"]
pytorch-gpu = ["torch==2.6.*"]

[tool.uv]
conflicts = [
  [
    { extra = "pytorch-cpu" },
    { extra = "pytorch-gpu" }
  ]
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "pytorch-cpu" },
  { index = "pytorch-gpu", extra = "pytorch-gpu" }
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-gpu"
url = "https://download.pytorch.org/whl/cu126"  # sync with PyTorch version
explicit = true
```

When I do `uv lock` and then `uv sync --all-groups --extra pytorch-gpu`, it does not install `nvidia-cudnn-cu12` and other required libraries, so PyTorch errors out on import:
```
>>> import torch
Traceback (most recent call last):
  File "/home/jakub/PycharmProjects/ml/.venv/lib/python3.12/site-packages/torch/__init__.py", line 318, in _load_global_deps
    ctypes.CDLL(global_deps_lib_path, mode=ctypes.RTLD_GLOBAL)
  File "/home/jakub/.pyenv/versions/3.12.11/lib/python3.12/ctypes/__init__.py", line 379, in __init__
    self._handle = _dlopen(self._name, mode)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^
OSError: libcudart.so.12: cannot open shared object file: No such file or directory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/jakub/PycharmProjects/ml/.venv/lib/python3.12/site-packages/torch/__init__.py", line 404, in <module>
    _load_global_deps()
  File "/home/jakub/PycharmProjects/ml/.venv/lib/python3.12/site-packages/torch/__init__.py", line 362, in _load_global_deps
    _preload_cuda_deps(lib_folder, lib_name)
  File "/home/jakub/PycharmProjects/ml/.venv/lib/python3.12/site-packages/torch/__init__.py", line 302, in _preload_cuda_deps
    raise ValueError(f"{lib_name} not found in the system path {sys.path}")
ValueError: libcublas.so.*[0-9] not found in the system path ['', '/home/jakub/.pyenv/versions/3.12.11/lib/python312.zip', '/home/jakub/.pyenv/versions/3.12.11/lib/python3.12', '/home/jakub/.pyenv/versions/3.12.11/lib/python3.12/lib-dynload', '/home/jakub/PycharmProjects/ml/.venv/lib/python3.12/site-packages']

```

Relevant part of `uv.lock`:
```
[[package]]
name = "torch"
version = "2.6.0"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "sys_platform == 'darwin'",
]
dependencies = [
    { name = "filelock", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "fsspec", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "jinja2", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "networkx", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "setuptools", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "sympy", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "typing-extensions", marker = "(sys_platform == 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:9a610afe216a85a8b9bc9f8365ed561535c93e804c2a317ef7fabcc5deda0989" },
]

[[package]]
name = "torch"
version = "2.6.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "filelock", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
    { name = "fsspec", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
    { name = "jinja2", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
    { name = "networkx", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cublas-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cuda-cupti-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cuda-nvrtc-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cuda-runtime-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cudnn-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cufft-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-curand-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cusolver-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cusparse-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-cusparselt-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-nccl-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-nvjitlink-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "nvidia-nvtx-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "setuptools", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
    { name = "sympy", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
    { name = "triton", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "typing-extensions", marker = "(extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu') or (extra != 'extra-32-ml-pytorch-cpu' and extra != 'extra-32-ml-pytorch-gpu')" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/e5/35/0c52d708144c2deb595cd22819a609f78fdd699b95ff6f0ebcd456e3c7c1/torch-2.6.0-cp312-cp312-manylinux1_x86_64.whl", hash = "sha256:2bb8987f3bb1ef2675897034402373ddfc8f5ef0e156e2d8cfc47cacafdda4a9", size = 766624563, upload-time = "2025-01-29T16:23:19.084Z" },
    { url = "https://files.pythonhosted.org/packages/01/d6/455ab3fbb2c61c71c8842753b566012e1ed111e7a4c82e0e1c20d0c76b62/torch-2.6.0-cp312-cp312-manylinux_2_28_aarch64.whl", hash = "sha256:b789069020c5588c70d5c2158ac0aa23fd24a028f34a8b4fcb8fcb4d7efcf5fb", size = 95607867, upload-time = "2025-01-29T16:25:55.649Z" },
    { url = "https://files.pythonhosted.org/packages/18/cf/ae99bd066571656185be0d88ee70abc58467b76f2f7c8bfeb48735a71fe6/torch-2.6.0-cp312-cp312-win_amd64.whl", hash = "sha256:7e1448426d0ba3620408218b50aa6ada88aeae34f7a239ba5431f6c8774b1239", size = 204120469, upload-time = "2025-01-29T16:24:01.821Z" },
    { url = "https://files.pythonhosted.org/packages/81/b4/605ae4173aa37fb5aa14605d100ff31f4f5d49f617928c9f486bb3aaec08/torch-2.6.0-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:9a610afe216a85a8b9bc9f8365ed561535c93e804c2a317ef7fabcc5deda0989", size = 66532538, upload-time = "2025-01-29T16:24:18.976Z" },
]

[[package]]
name = "torch"
version = "2.6.0+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "sys_platform != 'darwin'",
]
dependencies = [
    { name = "filelock", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "fsspec", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "jinja2", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "networkx", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "setuptools", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "sympy", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
    { name = "typing-extensions", marker = "(sys_platform != 'darwin' and extra == 'extra-32-ml-pytorch-cpu') or (extra == 'extra-32-ml-pytorch-cpu' and extra == 'extra-32-ml-pytorch-gpu')" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp312-cp312-linux_x86_64.whl", hash = "sha256:59e78aa0c690f70734e42670036d6b541930b8eabbaa18d94e090abf14cc4d91" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp312-cp312-manylinux_2_28_aarch64.whl", hash = "sha256:318290e8924353c61b125cdc8768d15208704e279e7757c113b9620740deca98" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp312-cp312-win_amd64.whl", hash = "sha256:4027d982eb2781c93825ab9527f17fbbb12dbabf422298e4b954be60016f87d8" },
]

[[package]]
name = "torch"
version = "2.6.0+cu126"
source = { registry = "https://download.pytorch.org/whl/cu126" }
dependencies = [
    { name = "filelock", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
    { name = "fsspec", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
    { name = "jinja2", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
    { name = "networkx", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
    { name = "setuptools", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
    { name = "sympy", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
    { name = "typing-extensions", marker = "extra == 'extra-32-ml-pytorch-gpu'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu126/torch-2.6.0%2Bcu126-cp312-cp312-linux_aarch64.whl", hash = "sha256:993e0e99c472df1d2746c3233ef8e88d992904fe75b8996a2c15439c43ff46c4" },
    { url = "https://download.pytorch.org/whl/cu126/torch-2.6.0%2Bcu126-cp312-cp312-manylinux_2_28_x86_64.whl", hash = "sha256:6bc5b9126daa3ac1e4d920b731da9f9503ff1f56204796de124e080f5cc3570e" },
    { url = "https://download.pytorch.org/whl/cu126/torch-2.6.0%2Bcu126-cp312-cp312-win_amd64.whl", hash = "sha256:b10c39c83e5d1afd639b5c9f5683b351e97e41390a93f59c59187004a9949924" },
]
```

### Platform

Ubuntu 25.04, Linux 6.14.0-23-generic x86_64 GNU/Linux

### Version

uv v0.7.20

### Python version

Python 3.12.11

---

_Label `bug` added by @j-adamczyk on 2025-07-10 12:34_

---

_Comment by @charliermarsh on 2025-07-11 00:12_

I think the problem here is that https://download.pytorch.org/whl/cu126/torch-2.6.0%2Bcu126-cp312-cp312-linux_aarch64.whl.metadata doesn't include those dependencies, so the PyTorch index is reporting inconsistent metadata for wheels on that version. We need to fix it in the index unfortunately.

---

_Comment by @charliermarsh on 2025-07-11 00:38_

I thought this was fixed by https://github.com/pytorch/pytorch/pull/145021.

---

_Label `external` added by @charliermarsh on 2025-07-11 01:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-11 01:42_

---

_Comment by @j-adamczyk on 2025-07-11 06:56_

Downgrading to PyTorch 2.5.1 works

---

_Comment by @charliermarsh on 2025-07-14 01:16_

It sounds like this in fixed in v2.7.0, but the PyTorch team doesn't plan to update the existing v2.6.0 wheels.

---

_Closed by @charliermarsh on 2025-07-14 01:16_

---
