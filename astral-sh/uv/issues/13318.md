```yaml
number: 13318
title: "The hash value of the `aarch64.whl` is missing in the Docker container on the macOS M1 platform."
type: issue
state: closed
author: eric-gitta-moore
labels:
  - bug
assignees: []
created_at: 2025-05-06T19:08:18Z
updated_at: 2025-05-13T15:34:42Z
url: https://github.com/astral-sh/uv/issues/13318
synced_at: 2026-01-10T03:41:47Z
```

# The hash value of the `aarch64.whl` is missing in the Docker container on the macOS M1 platform.

---

_Issue opened by @eric-gitta-moore on 2025-05-06 19:08_

### Question

I don't know what happened. Why is it that exactly the `manylinux_2_28_aarch64.whl` has its hash missing, which results in the inability to install it? 

```shell
❯ docker run -it --rm -v .:/app nvidia/cuda:12.8.1-cudnn-devel-ubuntu22.04 /bin/bash
❯ uv cache clean
❯ uv sync --extra cpu
DEBUG Requirement already installed: markdown-it-py==3.0.0
DEBUG Requirement already installed: pygments==2.19.1
DEBUG Requirement already installed: mdurl==0.1.2
DEBUG Found stale response for: https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_aarch64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_aarch64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_aarch64.whl
  x Failed to download `torch==2.7.0+cpu`
  `-> Hash mismatch for `torch==2.7.0+cpu`

      Expected:
        sha256:c98c4f48f42a2237e079f3de48e8549de2c8cf68cdcf2041564c7794bbce0b59
        sha256:f874c1ba4c834db5848eaafd6e63dfce87fb44bb2d9234978c3ad47b5b0f37dd
        sha256:6b7edcbf8bb0b9ac2e6c001434797c5ec3f25394f91eb0ed7aeeeeed9ad4500f
        sha256:e32f385dc0b5007ca410035c3b91ef4b1b34b142e9bcdb31d3f0224b7748e992

      Computed:
        sha256:ce510375ed79223db3ec144fe14cbcffc8a361ac57f39674397ff2d8db3b2c21
  help: `torch` (v2.7.0+cpu) was included because `mt-photos-ai[cpu]` (v0.1.0) depends on `torch`
root@0415c658ba27:/app# uanme -a
```


project.toml
```toml
[project]
name = "mt-photos-ai"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10,<3.12"
dependencies = [
    "cn-clip>=1.5.1",
    "fastapi[standard]>=0.115",
    "numpy>=1.24.4,<2",
    "opencv-contrib-python>=4.6.0.66",
    "opencv-python-headless>=4.6.0.66",
    "python-dotenv>=1.0.0",
    "python-multipart>=0.0.6",
    "rapidocr>=2.0.6",
    "uvicorn[standard]>=0.23.2",
    "insightface>=0.7.3",
    "requests>=2.28.1",
]

[project.optional-dependencies]
cuda = ["onnxruntime-gpu>=1.21.1", "torch>=2.6.0", "torchvision", "torchaudio"]
cpu = ["onnxruntime>=1.21.1", "torch>=2.6.0", "torchvision", "torchaudio"]

[tool.uv]
conflicts = [[{ extra = "cuda" }, { extra = "cpu" }]]

[tool.uv.sources]
torch = [
    { index = "pytorch-cuda", extra = "cuda" },
    { index = "pytorch-cpu", extra = "cpu" },
]
torchvision = [
    { index = "pytorch-cuda", extra = "cuda" },
    { index = "pytorch-cpu", extra = "cpu" },
]
torchaudio = [
    { index = "pytorch-cuda", extra = "cuda" },
    { index = "pytorch-cpu", extra = "cpu" },
]

[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
default = true

[[tool.uv.index]]
name = "pytorch-cuda"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

```


uv.lock
```toml

[[package]]
name = "torch"
version = "2.7.0+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "python_full_version >= '3.11' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version >= '3.11' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version >= '3.11' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version < '3.11' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version < '3.11' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version < '3.11' and sys_platform != 'darwin' and sys_platform != 'linux')",
]
dependencies = [
    { name = "filelock", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "fsspec", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "jinja2", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "networkx", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "sympy", version = "1.14.0", source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple/" }, marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "typing-extensions", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-manylinux_2_28_x86_64.whl", hash = "sha256:c98c4f48f42a2237e079f3de48e8549de2c8cf68cdcf2041564c7794bbce0b59" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-win_amd64.whl", hash = "sha256:f874c1ba4c834db5848eaafd6e63dfce87fb44bb2d9234978c3ad47b5b0f37dd" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_x86_64.whl", hash = "sha256:6b7edcbf8bb0b9ac2e6c001434797c5ec3f25394f91eb0ed7aeeeeed9ad4500f" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-win_amd64.whl", hash = "sha256:e32f385dc0b5007ca410035c3b91ef4b1b34b142e9bcdb31d3f0224b7748e992" },
]

```

### Platform

macOS 15 Darwin 24.3.0 arm64

### Version

uv 0.7.2

---

_Label `question` added by @eric-gitta-moore on 2025-05-06 19:08_

---

_Comment by @eric-gitta-moore on 2025-05-06 19:12_

I must be pasted it manually

```diff
[[package]]
name = "torch"
version = "2.7.0+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "python_full_version >= '3.11' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version >= '3.11' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version >= '3.11' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version < '3.11' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version < '3.11' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version < '3.11' and sys_platform != 'darwin' and sys_platform != 'linux')",
]
dependencies = [
    { name = "filelock", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "fsspec", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "jinja2", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "networkx", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "sympy", version = "1.14.0", source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple/" }, marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
    { name = "typing-extensions", marker = "(sys_platform != 'darwin' and extra == 'extra-12-mt-photos-ai-cpu') or (extra == 'extra-12-mt-photos-ai-cpu' and extra == 'extra-12-mt-photos-ai-cuda')" },
]
wheels = [
+    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-manylinux_2_28_aarch64.whl", hash = "sha256:ce510375ed79223db3ec144fe14cbcffc8a361ac57f39674397ff2d8db3b2c21" },
-    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-manylinux_2_28_x86_64.whl", hash = "sha256:c98c4f48f42a2237e079f3de48e8549de2c8cf68cdcf2041564c7794bbce0b59" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp310-cp310-win_amd64.whl", hash = "sha256:f874c1ba4c834db5848eaafd6e63dfce87fb44bb2d9234978c3ad47b5b0f37dd" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_aarch64.whl" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-manylinux_2_28_x86_64.whl", hash = "sha256:6b7edcbf8bb0b9ac2e6c001434797c5ec3f25394f91eb0ed7aeeeeed9ad4500f" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.7.0%2Bcpu-cp311-cp311-win_amd64.whl", hash = "sha256:e32f385dc0b5007ca410035c3b91ef4b1b34b142e9bcdb31d3f0224b7748e992" },
]
```

---

_Comment by @konstin on 2025-05-06 22:30_

Linking this to the other issue that reported this: #13195

---

_Label `question` removed by @konstin on 2025-05-06 22:56_

---

_Label `bug` added by @konstin on 2025-05-06 22:56_

---

_Comment by @07pepa on 2025-05-13 14:47_

Also true on m3

---

_Comment by @konstin on 2025-05-13 15:34_

Merging this with #13195

I've reported this upstream at https://github.com/pytorch/pytorch/issues/153469

---

_Closed by @konstin on 2025-05-13 15:34_

---
