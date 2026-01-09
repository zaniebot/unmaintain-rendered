---
number: 9307
title: "uv with pytorch & huggingface toolchain"
type: issue
state: closed
author: maxstrobel
labels:
  - bug
assignees: []
created_at: 2024-11-21T10:15:51Z
updated_at: 2024-11-21T13:58:02Z
url: https://github.com/astral-sh/uv/issues/9307
synced_at: 2026-01-07T13:12:18-06:00
---

# uv with pytorch & huggingface toolchain

---

_Issue opened by @maxstrobel on 2024-11-21 10:15_

Hello,
thanks a lot for this tool - a great user experience, real appreciate it!

I found the new pytorch guides that you provided - very helpful.

However, I think I'm facing some issues - my guess would be that packages with dependencies to torch overwrite the resolved packages that are added via extras.

~~~bash
$ uv version
uv 0.5.4 (c62c83c37 2024-11-20)
~~~

______________________

## Initial empty test project

~~~toml
pyproject.toml

[project]
name = "test-proj"
version = "0.1.0"
description = "Test project"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []
~~~

~~~toml
version = 1
requires-python = ">=3.11"

[[package]]
name = "gen-ai"
version = "0.1.0"
source = { virtual = "." }

[package.metadata]

[package.metadata.requires-dev]
dev = []
~~~

__________________________

## Adding Pytorch

Via [Configuring accelerators with optional dependencies
](https://github.com/astral-sh/uv/blob/main/docs/guides/integration/pytorch.md#configuring-accelerators-with-optional-dependencies)


~~~toml
pyproject.toml

[project]
name = "test-proj"
version = "0.1.0"
description = "Test project"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []
optional-dependencies = { cpu = ["torch>=2.5.1"], cu118 = ["torch>=2.5.1"] }

[tool.uv]
conflicts = [[{ extra = "cpu" }, { extra = "cu118" }]]
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
    { index = "pytorch-cu118", extra = "cu118" },
]
~~~

~~~toml
uv.lock
version = 1
requires-python = ">=3.11"
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
]
conflicts = [[
    { package = "test-proj", extra = "cpu" },
    { package = "test-proj", extra = "cu118" },
]]

...

[package.optional-dependencies]
cpu = [
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "platform_system == 'Darwin'" },
    { name = "torch", version = "2.5.1+cpu", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "platform_system != 'Darwin'" },
]
cu118 = [
    { name = "torch", version = "2.5.1+cu118", source = { registry = "https://download.pytorch.org/whl/cu118" } },
]

[package.metadata]
requires-dist = [
    { name = "torch", marker = "platform_system == 'Darwin' and extra == 'cpu'", specifier = ">=2.5.1" },
    { name = "torch", marker = "platform_system != 'Darwin' and extra == 'cpu'", specifier = ">=2.5.1", index = "https://download.pytorch.org/whl/cpu", conflict = { package = "test-proj", extra = "cpu" } },
    { name = "torch", marker = "extra == 'cu118'", specifier = ">=2.5.1", index = "https://download.pytorch.org/whl/cu118", conflict = { package = "test-proj", extra = "cu118" } },
]

[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
]
dependencies = [
    { name = "filelock", marker = "platform_system == 'Darwin'" },
    { name = "fsspec", marker = "platform_system == 'Darwin'" },
    { name = "jinja2", marker = "platform_system == 'Darwin'" },
    { name = "networkx", marker = "platform_system == 'Darwin'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and platform_system == 'Darwin'" },
    { name = "sympy", marker = "platform_system == 'Darwin'" },
    { name = "typing-extensions", marker = "platform_system == 'Darwin'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/d0/db/5d9cbfbc7968d79c5c09a0bc0bc3735da079f2fd07cc10498a62b320a480/torch-2.5.1-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:31f8c39660962f9ae4eeec995e3049b5492eb7360dd4f07377658ef4d728fa4c", size = 63884466 },
    { url = "https://files.pythonhosted.org/packages/57/6c/bf52ff061da33deb9f94f4121fde7ff3058812cb7d2036c97bc167793bd1/torch-2.5.1-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:8c712df61101964eb11910a846514011f0b6f5920c55dbf567bff8a34163d5b1", size = 63858109 },
]

[[package]]
name = "torch"
version = "2.5.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
]
dependencies = [
    { name = "filelock", marker = "platform_system != 'Darwin'" },
    { name = "fsspec", marker = "platform_system != 'Darwin'" },
    { name = "jinja2", marker = "platform_system != 'Darwin'" },
    { name = "networkx", marker = "platform_system != 'Darwin'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and platform_system != 'Darwin'" },
    { name = "sympy", marker = "platform_system != 'Darwin'" },
    { name = "typing-extensions", marker = "platform_system != 'Darwin'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-linux_x86_64.whl", hash = "sha256:07d7c9e069123d5af08b0cf0013d74f680b2d8be7d9e2cf561a52c90c55d9409" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp311-cp311-win_amd64.whl", hash = "sha256:81531d4d5ca74163dc9574b87396531e546a60cceb6253303c7db6a21e867fdf" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp312-cp312-linux_x86_64.whl", hash = "sha256:4856f9d6925121d13c2df07aa7580b767f449dfe71ae5acde9c27535d5da4840" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp312-cp312-win_amd64.whl", hash = "sha256:a6b720410350765d3d77c01a5ce098a6c45af446284e45e87a98b8a16e7d564d" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp313-cp313-linux_x86_64.whl", hash = "sha256:5dbbdf83caa90d0bcaa50e4933ca424889133b35226db79000877d4ec5d9ea37" },
]

[[package]]
name = "torch"
version = "2.5.1+cu118"
source = { registry = "https://download.pytorch.org/whl/cu118" }
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
]
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "nvidia-cublas-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-cupti-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-nvrtc-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-runtime-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cudnn-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cufft-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-curand-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusolver-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusparse-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nccl-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvtx-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "setuptools", marker = "python_full_version >= '3.12'" },
    { name = "sympy" },
    { name = "triton", marker = "python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp311-cp311-linux_x86_64.whl", hash = "sha256:c3a3fa09578e1acb76236dc3056ac67ac2f991d9214ab54ec440c4a1427cf016" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp311-cp311-win_amd64.whl", hash = "sha256:7e09af36c3d0bf8e6554cacae794f9fcd348d096e21f8be1dd42ed3bc48b4e2f" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp312-cp312-linux_x86_64.whl", hash = "sha256:f4c3ae93c72e8b725b510c3bc5b9dd5437f6740a66c3cc3bb1a19f177c3baef4" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp312-cp312-win_amd64.whl", hash = "sha256:3edf97d9b0a17e3f764a511ed084e866c086af76c9d489cc51d5edb77ca7c936" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp313-cp313-linux_x86_64.whl", hash = "sha256:60c1d1b522ea534a17e1ac60a0dc542ed578c45c84d63dc1528febbd1a0f5953" },
]

...
~~~

_____________________

## Adding `transformers` (or `accelerate` should also work)

~~~bash
uv add transformers
~~~

~~~toml
pyproject.toml

[project]
name = "test-proj"
version = "0.1.0"
description = "Test project"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "transformers>=4.46.3",
]
optional-dependencies = { cpu = ["torch>=2.5.1"], cu118 = ["torch>=2.5.1"] }

[tool.uv]
conflicts = [[{ extra = "cpu" }, { extra = "cu118" }]]
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
    { index = "pytorch-cu118", extra = "cu118" },
]
~~~

~~~toml
uv.lock

version = 1
requires-python = ">=3.11"
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
]
conflicts = [[
    { package = "test-proj", extra = "cpu" },
    { package = "test-proj", extra = "cu118" },
]]

...

[package.optional-dependencies]
cpu = [
    { name = "torch", version = "2.5.1", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "platform_system != 'Darwin'" },
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "platform_system == 'Darwin'" },
]
cu118 = [
    { name = "torch", version = "2.5.1+cu118", source = { registry = "https://download.pytorch.org/whl/cu118" } },
]

...

[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
]
dependencies = [
    { name = "filelock", marker = "platform_system != 'Darwin'" },
    { name = "fsspec", marker = "platform_system != 'Darwin'" },
    { name = "jinja2", marker = "platform_system != 'Darwin'" },
    { name = "networkx", marker = "platform_system != 'Darwin'" },
    { name = "nvidia-cublas-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-cupti-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-nvrtc-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-runtime-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cudnn-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cufft-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-curand-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusolver-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusparse-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nccl-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvjitlink-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvtx-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and platform_system != 'Darwin'" },
    { name = "sympy", marker = "platform_system != 'Darwin'" },
    { name = "typing-extensions", marker = "platform_system != 'Darwin'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:d5b3203f191bc40783c99488d2e776dcf93ac431a59491d627a1ca5b3ae20b22" },
    { url = "https://download.pytorch.org/whl/cpu/torch-2.5.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:36d1be99281b6f602d9639bd0af3ee0006e7aab16f6718d86f709d395b6f262c" },
]

[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
]
dependencies = [
    { name = "filelock", marker = "platform_system == 'Darwin'" },
    { name = "fsspec", marker = "platform_system == 'Darwin'" },
    { name = "jinja2", marker = "platform_system == 'Darwin'" },
    { name = "networkx", marker = "platform_system == 'Darwin'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and platform_system == 'Darwin'" },
    { name = "sympy", marker = "platform_system == 'Darwin'" },
    { name = "typing-extensions", marker = "platform_system == 'Darwin'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/d0/db/5d9cbfbc7968d79c5c09a0bc0bc3735da079f2fd07cc10498a62b320a480/torch-2.5.1-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:31f8c39660962f9ae4eeec995e3049b5492eb7360dd4f07377658ef4d728fa4c", size = 63884466 },
    { url = "https://files.pythonhosted.org/packages/57/6c/bf52ff061da33deb9f94f4121fde7ff3058812cb7d2036c97bc167793bd1/torch-2.5.1-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:8c712df61101964eb11910a846514011f0b6f5920c55dbf567bff8a34163d5b1", size = 63858109 },
]

[[package]]
name = "torch"
version = "2.5.1+cu118"
source = { registry = "https://download.pytorch.org/whl/cu118" }
resolution-markers = [
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version < '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
    "python_full_version >= '3.12' and platform_system == 'Darwin'",
]
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "nvidia-cublas-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-cupti-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-nvrtc-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-runtime-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cudnn-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cufft-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-curand-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusolver-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusparse-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nccl-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvtx-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "setuptools", marker = "python_full_version >= '3.12'" },
    { name = "sympy" },
    { name = "triton", marker = "python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp311-cp311-linux_x86_64.whl", hash = "sha256:c3a3fa09578e1acb76236dc3056ac67ac2f991d9214ab54ec440c4a1427cf016" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp311-cp311-win_amd64.whl", hash = "sha256:7e09af36c3d0bf8e6554cacae794f9fcd348d096e21f8be1dd42ed3bc48b4e2f" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp312-cp312-linux_x86_64.whl", hash = "sha256:f4c3ae93c72e8b725b510c3bc5b9dd5437f6740a66c3cc3bb1a19f177c3baef4" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp312-cp312-win_amd64.whl", hash = "sha256:3edf97d9b0a17e3f764a511ed084e866c086af76c9d489cc51d5edb77ca7c936" },
    { url = "https://download.pytorch.org/whl/cu118/torch-2.5.1%2Bcu118-cp313-cp313-linux_x86_64.whl", hash = "sha256:60c1d1b522ea534a17e1ac60a0dc542ed578c45c84d63dc1528febbd1a0f5953" },
]

...
~~~


________________

# Summary

before adding `transformers`:

~~~toml
uv.lock

[package.optional-dependencies]
cpu = [
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "platform_system == 'Darwin'" },
    { name = "torch", version = "2.5.1+cpu", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "platform_system != 'Darwin'" },
]
cu118 = [
    { name = "torch", version = "2.5.1+cu118", source = { registry = "https://download.pytorch.org/whl/cu118" } },
]
~~~

after adding `transformers`:

~~~toml
uv.lock

[package.optional-dependencies]
cpu = [
    { name = "torch", version = "2.5.1", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "platform_system != 'Darwin'" },
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "platform_system == 'Darwin'" },
]
cu118 = [
    { name = "torch", version = "2.5.1+cu118", source = { registry = "https://download.pytorch.org/whl/cu118" } },
]
~~~

So basically the `+cpu` version specifier is removed, which will break the installation of CPU only version.


__________

PS:

If torch is added via [environment markers](https://github.com/astral-sh/uv/blob/main/docs/guides/integration/pytorch.md#configuring-accelerators-with-environment-markers), it seems to be stable.

---

_Comment by @maxstrobel on 2024-11-21 10:18_

Seems that @mbignotti was a little bit faster than me filling the issue - looks like this one is a duplicate of https://github.com/astral-sh/uv/issues/9306, or the other way round.

---

_Referenced in [astral-sh/uv#9310](../../astral-sh/uv/issues/9310.md) on 2024-11-21 11:15_

---

_Comment by @charliermarsh on 2024-11-21 13:57_

I believe this is actually the same as #9289, so let's track there. Thanks for the clear write-up.

---

_Closed by @charliermarsh on 2024-11-21 13:57_

---

_Label `bug` added by @charliermarsh on 2024-11-21 13:57_

---

_Comment by @charliermarsh on 2024-11-21 13:58_

I believe the general issue lies in having dependencies that depend on a package that's listed multiple times, across conflicting groups.

---
