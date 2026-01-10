---
number: 11406
title: Cannot install the CPU version of torch 2.6.0
type: issue
state: closed
author: petteriTeikari
labels:
  - bug
assignees: []
created_at: 2025-02-10T21:33:11Z
updated_at: 2025-02-22T17:24:26Z
url: https://github.com/astral-sh/uv/issues/11406
synced_at: 2026-01-10T01:25:05Z
---

# Cannot install the CPU version of torch 2.6.0

---

_Issue opened by @petteriTeikari on 2025-02-10 21:33_

### Summary

I am facing similar issues as described in https://github.com/astral-sh/uv/issues/1497#top afterr following [your instructions ](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies) and trying to have the CPU and CUDA flags both working for laptop debugging and CUDA for actual running of the code.

* `uv sync --extra cu124` works
* `uv pip install torch==2.6.0+cpu --index-url https://download.pytorch.org/whl/cpu` works 
* `uv sync --extra cpu` fails with the following message:

```
error: Distribution `torch==2.6.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_35_x86_64`), but `torch` (v2.6.0) only has wheels for the following platform: `macosx_11_0_arm64`
```

I would say that [`torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl`]( https://download.pytorch.org/whl/torch/) seems like the correct wheel? Can I somehow force `uv` to use this wheel?

## My `pyproject.toml`

```
[project]
requires-python = ">=3.11"
dependencies = [
   ...packages...
]

[project.optional-dependencies]
# e.g. torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

## `uv.lock`

```
[[package]]
name = "torch"
version = "2.6.0"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "sys_platform == 'darwin'",
    "platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(platform_machine != 'aarch64' and sys_platform == 'linux') or (sys_platform != 'darwin' and sys_platform != 'linux')",
]
dependencies = [
    { name = "filelock", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "fsspec", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "jinja2", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "networkx", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cublas-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cuda-cupti-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cuda-nvrtc-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cuda-runtime-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cudnn-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cufft-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-curand-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cusolver-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cusparse-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-cusparselt-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-nccl-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-nvjitlink-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "nvidia-nvtx-cu12", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "sympy", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
    { name = "triton", marker = "(platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124') or (extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124')" },
    { name = "typing-extensions", marker = "(extra == 'extra-8-sfm-venv-cpu' and extra == 'extra-8-sfm-venv-cu124') or (extra != 'extra-8-sfm-venv-cpu' and extra != 'extra-8-sfm-venv-cu124')" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/78/a9/97cbbc97002fff0de394a2da2cdfa859481fdca36996d7bd845d50aa9d8d/torch-2.6.0-cp311-cp311-manylinux1_x86_64.whl", hash = "sha256:7979834102cd5b7a43cc64e87f2f3b14bd0e1458f06e9f88ffa386d07c7446e1", size = 766715424 },
    { url = "https://files.pythonhosted.org/packages/6d/fa/134ce8f8a7ea07f09588c9cc2cea0d69249efab977707cf67669431dcf5c/torch-2.6.0-cp311-cp311-manylinux_2_28_aarch64.whl", hash = "sha256:ccbd0320411fe1a3b3fec7b4d3185aa7d0c52adac94480ab024b5c8f74a0bf1d", size = 95759416 },
    { url = "https://files.pythonhosted.org/packages/11/c5/2370d96b31eb1841c3a0883a492c15278a6718ccad61bb6a649c80d1d9eb/torch-2.6.0-cp311-cp311-win_amd64.whl", hash = "sha256:46763dcb051180ce1ed23d1891d9b1598e07d051ce4c9d14307029809c4d64f7", size = 204164970 },
    { url = "https://files.pythonhosted.org/packages/0b/fa/f33a4148c6fb46ca2a3f8de39c24d473822d5774d652b66ed9b1214da5f7/torch-2.6.0-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:94fc63b3b4bedd327af588696559f68c264440e2503cc9e6954019473d74ae21", size = 66530713 },
]
```

### Platform

Ubuntu 22.4 X86_64 Architecture (Linux 6.8.0-52-generic)

### Version

uv version: 0.5.29 

### Python version

Python 3.11.10 

---

_Label `bug` added by @petteriTeikari on 2025-02-10 21:33_

---

_Comment by @charliermarsh on 2025-02-10 21:38_

Sorry, there must be something else here that isn't captured by your example. The lockfile generated by this `pyproject.toml` doesn't include that `[[package]]` at all:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
]

[project.optional-dependencies]
# e.g. torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

This only includes:

```toml
[[package]]
name = "torch"
version = "2.6.0"
source = { registry = "https://download.pytorch.org/whl/cpu" }
```

---

_Comment by @charliermarsh on 2025-02-10 21:40_

As an aside, `torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl` is not part of either of the registries you specified. You can see the wheels for the CPU registry [here](https://download.pytorch.org/whl/cpu/torch/) and the CUDA registry [here](https://download.pytorch.org/whl/cu124/torch/). With `--extra cpu`, on Linux you should probably be getting https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp311-cp311-linux_x86_64.whl.

---

_Comment by @petteriTeikari on 2025-02-11 07:28_

> As an aside, `torch-2.6.0+cpu.cxx11.abi-cp311-cp311-linux_x86_64.whl` is not part of either of the registries you specified. You can see the wheels for the CPU registry [here](https://download.pytorch.org/whl/cpu/torch/) and the CUDA registry [here](https://download.pytorch.org/whl/cu124/torch/). With `--extra cpu`, on Linux you should probably be getting https://download.pytorch.org/whl/cpu/torch-2.6.0%2Bcpu-cp311-cp311-linux_x86_64.whl.

Fair enough, I quickly glanced at the wheels, but I ultimately wanted `uv` to find automagically the wheel for CPU. And from previous issues the difficulties were due to how `pytorch` managed things?

---

_Comment by @charliermarsh on 2025-02-11 13:07_

Are you able to share the rest of the pyproject.toml, or at least the dependencies field? It’s missing details necessary to reproduce the issue.

---

_Label `bug` removed by @charliermarsh on 2025-02-11 14:21_

---

_Label `needs-mre` added by @charliermarsh on 2025-02-11 14:21_

---

_Comment by @petteriTeikari on 2025-02-12 10:41_

> Are you able to share the rest of the pyproject.toml, or at least the dependencies field? It’s missing details necessary to reproduce the issue.

Sure, here is the full one

```
[project]
name = "venv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11, <3.12"
dependencies = [
    "einops",
    "fire>=0.7.0",
    "gradio",
    "huggingface-hub",
    "hydra-core>=1.3.2",
    "imageio[ffmpeg]",
    "iopath>=0.1.10",
    "loguru>=0.7.3",
    "numpy==1.24.3",
    "omegaconf",
    "opencv-python",
    "pillow>=11.1.0",
    "plotly",
    "poselib>=2.0.2",
    "pre-commit>=4.1.0",
    "pyceres==2.3",
    "pycolmap==3.10.0",
    "pyqt5-sip>=12.17.0",
    "pyqtchart==5.12",
    "pyrender==0.1.45",
    "roma==1.4.1",
    "scikit-image>=0.25.1",
    "scikit-learn",
    "scipy==1.11.2",
    "setuptools>=75.8.0",
    "timm==0.6.7",
    "torch>=2.6.0",
    "tqdm",
    "trimesh",
    "visdom>=0.2.4",
]

[project.optional-dependencies]
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

---

_Label `needs-mre` removed by @charliermarsh on 2025-02-12 15:47_

---

_Label `bug` added by @charliermarsh on 2025-02-12 15:47_

---

_Comment by @charliermarsh on 2025-02-12 15:48_

I think this is a preference problem. We end up choosing `2.6.0` from the CPU index (rather than `2.6.0+cpu`) because that's what we get from PyPI.


---

_Comment by @charliermarsh on 2025-02-12 18:10_

I think part of the issue is that you also have torch defined as a non-optional dependency. Where do you expect torch to come from when no `--extra` is provided? What version are you hoping for?

---

_Comment by @charliermarsh on 2025-02-12 18:11_

E.g., I think this would do what you want:

```toml
[project]
name = "venv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11, <3.12"
dependencies = [
    "einops",
    "fire>=0.7.0",
    "gradio",
    "huggingface-hub",
    "hydra-core>=1.3.2",
    "imageio[ffmpeg]",
    "iopath>=0.1.10",
    "loguru>=0.7.3",
    "numpy==1.24.3",
    "omegaconf",
    "opencv-python",
    "pillow>=11.1.0",
    "plotly",
    "poselib>=2.0.2",
    "pre-commit>=4.1.0",
    "pyceres==2.3",
    "pycolmap==3.10.0",
    "pyqt5-sip>=12.17.0",
    "pyqtchart==5.12",
    "pyrender==0.1.45",
    "roma==1.4.1",
    "scikit-image>=0.25.1",
    "scikit-learn",
    "scipy==1.11.2",
    "setuptools>=75.8.0",
    "tqdm",
    "trimesh",
    "visdom>=0.2.4",
]

[project.optional-dependencies]
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
  "timm==0.6.7",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
  "timm==0.6.7",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

---

_Comment by @petteriTeikari on 2025-02-14 15:52_

> I think part of the issue is that you also have torch defined as a non-optional dependency. Where do you expect torch to come from when no `--extra` is provided? What version are you hoping for?

Hmm. Yes I agree that it is not the most full-proof way to set up the `pyproject.toml` and I assume that the end-user will choose either one of the extra options and never just uses `uv sync`

I updated the `pyproject.toml` and I am still getting the same issue

```
uv sync --extra cpu
Using CPython 3.11.10
Creating virtual environment at: .venv
Resolved 125 packages in 1.96s
error: Distribution `torch==2.6.0 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_35_x86_64`), but `torch` (v2.6.0) only has wheels for the following platform: `macosx_11_0_arm64`
```


---

_Comment by @charliermarsh on 2025-02-14 19:49_

Yeah, unfortunately I think you have to do this for now, until we fix the "preference" bug I described above:

```toml
cpu = [
  "torch==2.6.0+cpu",
  "torchvision==0.21.0+cpu",
  "timm==0.6.7",
]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-16 00:17_

---

_Referenced in [astral-sh/uv#11546](../../astral-sh/uv/pulls/11546.md) on 2025-02-16 01:14_

---

_Closed by @charliermarsh on 2025-02-16 01:35_

---

_Closed by @charliermarsh on 2025-02-16 01:35_

---

_Referenced in [astral-sh/uv#11558](../../astral-sh/uv/issues/11558.md) on 2025-02-16 16:39_

---

_Referenced in [sdsc-ordes/kg-llm-interface#35](../../sdsc-ordes/kg-llm-interface/pulls/35.md) on 2025-02-17 10:24_

---

_Comment by @amitkma on 2025-02-21 06:36_

Hey @charliermarsh, Isn't this issue resolved in #11546  because I am still facing this issue?

---

_Comment by @charliermarsh on 2025-02-22 17:24_

Yup, the issue is resolved. You should open a new issue with a complete reproduction if you're facing the same problem. (You'll definitely need to `rm uv.lock` after upgrading.)

---
