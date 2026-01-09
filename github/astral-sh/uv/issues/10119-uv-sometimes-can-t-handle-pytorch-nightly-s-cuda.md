---
number: 10119
title: "`uv` sometimes can't handle PyTorch nightly's cuda dependencies correctly."
type: issue
state: closed
author: YouJiacheng
labels:
  - external
assignees: []
created_at: 2024-12-23T07:46:50Z
updated_at: 2025-01-16T15:00:35Z
url: https://github.com/astral-sh/uv/issues/10119
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv` sometimes can't handle PyTorch nightly's cuda dependencies correctly.

---

_Issue opened by @YouJiacheng on 2024-12-23 07:46_

my pyproject.toml
```toml
[project]
name = "modded-nanogpt"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "numpy>=2.1.3",
    "torch==2.6.0.dev20241222",
    "pytorch-triton>=3.2.0",
    "huggingface-hub>=0.26.2",
    "tqdm>=4.67.0",
]

[tool.uv]
environments = [
    "platform_system == 'Linux'",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-nightly-cu126"},
]
pytorch-triton = [
    { index = "pytorch-nightly-cu126"},
]

[[tool.uv.index]]
name = "pytorch-nightly-cu126"
url = "https://download.pytorch.org/whl/nightly/cu126"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```
`uv tree` output
```
Resolved 22 packages in 7ms
modded-nanogpt v0.1.0
├── huggingface-hub v0.27.0
│   ├── filelock v3.16.1
│   ├── fsspec v2024.10.0
│   ├── packaging v24.2
│   ├── pyyaml v6.0.2
│   ├── requests v2.32.3
│   │   ├── certifi v2024.12.14
│   │   ├── charset-normalizer v3.4.0
│   │   ├── idna v3.10
│   │   └── urllib3 v2.2.3
│   ├── tqdm v4.67.1
│   └── typing-extensions v4.12.2
├── numpy v2.2.0
├── pytorch-triton v3.2.0+git35c6c7c6
├── torch v2.6.0.dev20241222+cu126
│   ├── filelock v3.16.1
│   ├── fsspec v2024.10.0
│   ├── jinja2 v3.1.4
│   │   └── markupsafe v3.0.2
│   ├── networkx v3.4.2
│   ├── setuptools v75.6.0
│   ├── sympy v1.13.1
│   │   └── mpmath v1.3.0
│   └── typing-extensions v4.12.2
└── tqdm v4.67.1
```
expected output:
```
Resolved 35 packages in 7ms
modded-nanogpt v0.1.0
├── huggingface-hub v0.27.0
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── packaging v24.2
│   ├── pyyaml v6.0.2
│   ├── requests v2.32.3
│   │   ├── certifi v2024.12.14
│   │   ├── charset-normalizer v3.4.0
│   │   ├── idna v3.10
│   │   └── urllib3 v2.3.0
│   ├── tqdm v4.67.1
│   └── typing-extensions v4.12.2
├── numpy v2.2.1
├── pytorch-triton v3.2.0+git35c6c7c6
├── torch v2.6.0.dev20241222+cu126
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── jinja2 v3.1.5
│   │   └── markupsafe v3.0.2
│   ├── networkx v3.4.2
│   ├── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cuda-cupti-cu12 v12.6.80
│   ├── nvidia-cuda-nvrtc-cu12 v12.6.77
│   ├── nvidia-cuda-runtime-cu12 v12.6.77
│   ├── nvidia-cudnn-cu12 v9.5.1.17
│   │   └── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cufft-cu12 v11.3.0.4
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-curand-cu12 v10.3.7.77
│   ├── nvidia-cusolver-cu12 v11.7.1.2
│   │   ├── nvidia-cublas-cu12 v12.6.4.1
│   │   ├── nvidia-cusparse-cu12 v12.5.4.2
│   │   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-cusparse-cu12 v12.5.4.2 (*)
│   ├── nvidia-cusparselt-cu12 v0.6.3
│   ├── nvidia-nccl-cu12 v2.21.5
│   ├── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-nvtx-cu12 v12.6.77
│   ├── setuptools v75.6.0
│   ├── sympy v1.13.1
│   │   └── mpmath v1.3.0
│   └── typing-extensions v4.12.2
└── tqdm v4.67.1
(*) Package tree already displayed
```

---
# extra test 1

In a new directory I run `uv init` `uv venv` then `uv pip install torch --index-url https://download.pytorch.org/whl/nightly/cu126`.
It can correctly install `nvidia-*`.
```
Resolved 24 packages in 2.83s
Installed 24 packages in 127ms
 + filelock==3.16.1
 + fsspec==2024.10.0
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.4.2
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
 + pytorch-triton==3.2.0+git0d4682f0
 + setuptools==70.2.0
 + sympy==1.13.1
 + torch==2.6.0.dev20241222+cu126
 + typing-extensions==4.12.2
```
and `uv pip tree` gives
```
torch v2.6.0.dev20241222+cu126
├── filelock v3.16.1
├── fsspec v2024.10.0
├── jinja2 v3.1.4
│   └── markupsafe v2.1.5
├── networkx v3.4.2
├── nvidia-cublas-cu12 v12.6.4.1
├── nvidia-cuda-cupti-cu12 v12.6.80
├── nvidia-cuda-nvrtc-cu12 v12.6.77
├── nvidia-cuda-runtime-cu12 v12.6.77
├── nvidia-cudnn-cu12 v9.5.1.17
│   └── nvidia-cublas-cu12 v12.6.4.1
├── nvidia-cufft-cu12 v11.3.0.4
│   └── nvidia-nvjitlink-cu12 v12.6.85
├── nvidia-curand-cu12 v10.3.7.77
├── nvidia-cusolver-cu12 v11.7.1.2
│   ├── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cusparse-cu12 v12.5.4.2
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   └── nvidia-nvjitlink-cu12 v12.6.85
├── nvidia-cusparse-cu12 v12.5.4.2 (*)
├── nvidia-cusparselt-cu12 v0.6.3
├── nvidia-nccl-cu12 v2.21.5
├── nvidia-nvjitlink-cu12 v12.6.85
├── nvidia-nvtx-cu12 v12.6.77
├── pytorch-triton v3.2.0+git0d4682f0
├── setuptools v70.2.0
├── sympy v1.13.1
│   └── mpmath v1.3.0
└── typing-extensions v4.12.2
(*) Package tree already displayed
```

---
# extra test 2

`torch==2.6.0.dev20241203+cu126` is okay:
```
Resolved 35 packages in 7ms
modded-nanogpt v0.1.0
├── huggingface-hub v0.27.0
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── packaging v24.2
│   ├── pyyaml v6.0.2
│   ├── requests v2.32.3
│   │   ├── certifi v2024.12.14
│   │   ├── charset-normalizer v3.4.0
│   │   ├── idna v3.10
│   │   └── urllib3 v2.3.0
│   ├── tqdm v4.67.1
│   └── typing-extensions v4.12.2
├── numpy v2.2.1
├── pytorch-triton v3.2.0+git35c6c7c6
├── torch v2.6.0.dev20241203+cu126
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── jinja2 v3.1.5
│   │   └── markupsafe v3.0.2
│   ├── networkx v3.4.2
│   ├── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cuda-cupti-cu12 v12.6.80
│   ├── nvidia-cuda-nvrtc-cu12 v12.6.77
│   ├── nvidia-cuda-runtime-cu12 v12.6.77
│   ├── nvidia-cudnn-cu12 v9.5.1.17
│   │   └── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cufft-cu12 v11.3.0.4
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-curand-cu12 v10.3.7.77
│   ├── nvidia-cusolver-cu12 v11.7.1.2
│   │   ├── nvidia-cublas-cu12 v12.6.4.1
│   │   ├── nvidia-cusparse-cu12 v12.5.4.2
│   │   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-cusparse-cu12 v12.5.4.2 (*)
│   ├── nvidia-cusparselt-cu12 v0.6.3
│   ├── nvidia-nccl-cu12 v2.21.5
│   ├── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-nvtx-cu12 v12.6.77
│   ├── setuptools v75.6.0
│   ├── sympy v1.13.1
│   │   └── mpmath v1.3.0
│   └── typing-extensions v4.12.2
└── tqdm v4.67.1
(*) Package tree already displayed
```

---

_Comment by @YouJiacheng on 2024-12-23 08:25_

there are these requirements in METADATA
![Image](https://github.com/user-attachments/assets/33feafa4-39ff-4efd-9c9d-b650deb4124d)


---

_Comment by @charliermarsh on 2024-12-23 13:43_

Can you be more specific about the actual problem here? What's going wrong? What happened that you did not expect?

---

_Label `question` added by @charliermarsh on 2024-12-23 13:43_

---

_Comment by @YouJiacheng on 2024-12-23 14:40_

I expect `uv sync` on the `pyproject.toml` I posted on this issue will install
```
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
```
as if I used `torch==2.6.0.dev20241203` instead of `torch==2.6.0.dev20241222` in the `pyproject.toml`.

---

_Comment by @YouJiacheng on 2024-12-23 14:44_

Or:
expected `uv tree` output:
```
modded-nanogpt v0.1.0
├── huggingface-hub v0.27.0
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── packaging v24.2
│   ├── pyyaml v6.0.2
│   ├── requests v2.32.3
│   │   ├── certifi v2024.12.14
│   │   ├── charset-normalizer v3.4.0
│   │   ├── idna v3.10
│   │   └── urllib3 v2.3.0
│   ├── tqdm v4.67.1
│   └── typing-extensions v4.12.2
├── numpy v2.2.1
├── pytorch-triton v3.2.0+git35c6c7c6
├── torch v2.6.0.dev20241222+cu126
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── jinja2 v3.1.5
│   │   └── markupsafe v3.0.2
│   ├── networkx v3.4.2
│   ├── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cuda-cupti-cu12 v12.6.80
│   ├── nvidia-cuda-nvrtc-cu12 v12.6.77
│   ├── nvidia-cuda-runtime-cu12 v12.6.77
│   ├── nvidia-cudnn-cu12 v9.5.1.17
│   │   └── nvidia-cublas-cu12 v12.6.4.1
│   ├── nvidia-cufft-cu12 v11.3.0.4
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-curand-cu12 v10.3.7.77
│   ├── nvidia-cusolver-cu12 v11.7.1.2
│   │   ├── nvidia-cublas-cu12 v12.6.4.1
│   │   ├── nvidia-cusparse-cu12 v12.5.4.2
│   │   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   │   └── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-cusparse-cu12 v12.5.4.2 (*)
│   ├── nvidia-cusparselt-cu12 v0.6.3
│   ├── nvidia-nccl-cu12 v2.21.5
│   ├── nvidia-nvjitlink-cu12 v12.6.85
│   ├── nvidia-nvtx-cu12 v12.6.77
│   ├── setuptools v75.6.0
│   ├── sympy v1.13.1
│   │   └── mpmath v1.3.0
│   └── typing-extensions v4.12.2
└── tqdm v4.67.1
(*) Package tree already displayed
```
actual `uv tree` output
```
modded-nanogpt v0.1.0
├── huggingface-hub v0.27.0
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── packaging v24.2
│   ├── pyyaml v6.0.2
│   ├── requests v2.32.3
│   │   ├── certifi v2024.12.14
│   │   ├── charset-normalizer v3.4.0
│   │   ├── idna v3.10
│   │   └── urllib3 v2.3.0
│   ├── tqdm v4.67.1
│   └── typing-extensions v4.12.2
├── numpy v2.2.1
├── pytorch-triton v3.2.0+git35c6c7c6
├── torch v2.6.0.dev20241222+cu126
│   ├── filelock v3.16.1
│   ├── fsspec v2024.12.0
│   ├── jinja2 v3.1.5
│   │   └── markupsafe v3.0.2
│   ├── networkx v3.4.2
│   ├── setuptools v75.6.0
│   ├── sympy v1.13.1
│   │   └── mpmath v1.3.0
│   └── typing-extensions v4.12.2
└── tqdm v4.67.1
```

---

_Comment by @YouJiacheng on 2024-12-23 14:47_

I also updated the description of the issue.

---

_Comment by @YouJiacheng on 2024-12-23 14:49_

I think it's very specific and clear now.

---

_Comment by @charliermarsh on 2024-12-23 15:25_

Thank you! I think this is something that basically has to be solved by PyTorch. The issue is that the wheels for `2.6.0.dev20241222+cu126` don't have consistent metadata, and it's a fundamental assumption of uv that the metadata for a given version _is_ consistent.

For example, the [manylinux version](https://download.pytorch.org/whl/nightly/cu126/torch-2.6.0.dev20241222%2Bcu126-cp312-cp312-manylinux_2_28_x86_64.whl) has:

```
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
Requires-Dist: pytorch-triton==3.2.0+git0d4682f0; platform_system == "Linux" and platform_machine == "x86_64" and python_version != "3.13t"
```

But the [ARM Linux version](https://download.pytorch.org/whl/nightly/cu126/torch-2.6.0.dev20241222%2Bcu126-cp312-cp312-linux_aarch64.whl) has:

```
Requires-Dist: filelock
Requires-Dist: typing-extensions>=4.10.0
Requires-Dist: setuptools; python_version >= "3.12"
Requires-Dist: sympy==1.13.1; python_version >= "3.9"
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Provides-Extra: optree
Requires-Dist: optree>=0.13.0; extra == "optree"
Provides-Extra: opt-einsum
Requires-Dist: opt-einsum>=3.3; extra == "opt-einsum"
```

---

_Label `question` removed by @charliermarsh on 2024-12-23 15:25_

---

_Label `upstream` added by @charliermarsh on 2024-12-23 15:25_

---

_Comment by @YouJiacheng on 2024-12-23 16:09_

got it, is there any workaround if I only care about linux?

---

_Comment by @YouJiacheng on 2024-12-23 16:12_

I remember there is a way to specify metadata manually, but I don't want to do so.
Is there a way to specify the source of metadata to be a given wheel?

---

_Comment by @charliermarsh on 2024-12-23 16:15_

Yeah, you could do:

```toml
[project]
name = "modded-nanogpt"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "torch",
]

[tool.uv]
environments = [
    "platform_system == 'Linux'",
]

[tool.uv.sources]
torch = [
    { url = "https://download.pytorch.org/whl/nightly/cu126/torch-2.6.0.dev20241222%2Bcu126-cp312-cp312-manylinux_2_28_x86_64.whl" }
]
```

---

_Closed by @charliermarsh on 2024-12-23 19:07_

---

_Comment by @YouJiacheng on 2024-12-25 05:25_

Hmmmm, it seems that this will cause `uv sync` hang/stuck after cache clean (or after some time).
I need to `rm uv.lock` and then `uv sync` will download the torch wheel again.
do you have any idea?

---

_Comment by @tmm1 on 2025-01-16 08:29_

Was this reported upstream? Has it been happening consistently on versions other than `2.6.0.dev20241222-cu126`?

---

_Comment by @charliermarsh on 2025-01-16 15:00_

Sorry, I would need a more thorough reproduction to help out.

---

_Referenced in [astral-sh/uv#10693](../../astral-sh/uv/issues/10693.md) on 2025-01-16 20:06_

---

_Referenced in [pytorch/pytorch#145021](../../pytorch/pytorch/pulls/145021.md) on 2025-01-17 00:12_

---
