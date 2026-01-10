```yaml
number: 9734
title: Support PyTorch CPU and GPU dependency
type: issue
state: closed
author: jbcdnr
labels: []
assignees: []
created_at: 2024-12-09T09:40:34Z
updated_at: 2024-12-27T16:43:31Z
url: https://github.com/astral-sh/uv/issues/9734
synced_at: 2026-01-10T04:36:21Z
```

# Support PyTorch CPU and GPU dependency

---

_Issue opened by @jbcdnr on 2024-12-09 09:40_

I followed the guide for supporting [PyTorch CPU and GPU dependency](https://docs.astral.sh/uv/guides/integration/pytorch/). It works well until I added a package `accelerate` that depends on torch, then this package would depend on both version and both `cpu` and `gpu` version are installed.

`pyproject.toml`
```toml
[project]
name = "test-torch"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
cpu = ["torch>=2.5.1", "accelerate==0.26.1"]
cu124 = ["torch>=2.5.1", "accelerate==0.26.1"]

[tool.uv]
conflicts = [[{ extra = "cpu" }, { extra = "cu124" }]]

[tool.uv.sources]
torch = [
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


`uv tree`
```

uv tree
warning: `VIRTUAL_ENV=/opt/venv` does not match the project environment path `.venv` and will be ignored
Resolved 39 packages in 2ms
test-torch v0.1.0
├── accelerate v0.26.1 (extra: cpu)
│   ├── huggingface-hub v0.26.3
│   │   ├── filelock v3.16.1
│   │   ├── fsspec v2024.10.0
│   │   ├── packaging v24.2
│   │   ├── pyyaml v6.0.2
│   │   ├── requests v2.32.3
│   │   │   ├── certifi v2024.8.30
│   │   │   ├── charset-normalizer v3.4.0
│   │   │   ├── idna v3.10
│   │   │   └── urllib3 v2.2.3
│   │   ├── tqdm v4.67.1
│   │   └── typing-extensions v4.12.2
│   ├── numpy v2.1.3
│   ├── packaging v24.2
│   ├── psutil v6.1.0
│   ├── pyyaml v6.0.2
│   ├── safetensors v0.4.5
│   ├── torch v2.5.1+cpu
│   │   ├── filelock v3.16.1
│   │   ├── fsspec v2024.10.0
│   │   ├── jinja2 v3.1.4
│   │   │   └── markupsafe v3.0.2
│   │   ├── networkx v3.4.2
│   │   ├── setuptools v75.6.0
│   │   ├── sympy v1.13.1
│   │   │   └── mpmath v1.3.0
│   │   └── typing-extensions v4.12.2
│   └── torch v2.5.1+cu124
│       ├── filelock v3.16.1
│       ├── fsspec v2024.10.0
│       ├── jinja2 v3.1.4 (*)
│       ├── networkx v3.4.2
│       ├── nvidia-cublas-cu12 v12.4.5.8
│       ├── nvidia-cuda-cupti-cu12 v12.4.127
│       ├── nvidia-cuda-nvrtc-cu12 v12.4.127
│       ├── nvidia-cuda-runtime-cu12 v12.4.127
│       ├── nvidia-cudnn-cu12 v9.1.0.70
│       │   └── nvidia-cublas-cu12 v12.4.5.8
│       ├── nvidia-cufft-cu12 v11.2.1.3
│       │   └── nvidia-nvjitlink-cu12 v12.4.127
│       ├── nvidia-curand-cu12 v10.3.5.147
│       ├── nvidia-cusolver-cu12 v11.6.1.9
│       │   ├── nvidia-cublas-cu12 v12.4.5.8
│       │   ├── nvidia-cusparse-cu12 v12.3.1.170
│       │   │   └── nvidia-nvjitlink-cu12 v12.4.127
│       │   └── nvidia-nvjitlink-cu12 v12.4.127
│       ├── nvidia-cusparse-cu12 v12.3.1.170 (*)
│       ├── nvidia-nccl-cu12 v2.21.5
│       ├── nvidia-nvjitlink-cu12 v12.4.127
│       ├── nvidia-nvtx-cu12 v12.4.127
│       ├── setuptools v75.6.0
│       ├── sympy v1.13.1 (*)
│       ├── triton v3.1.0
│       │   └── filelock v3.16.1
│       └── typing-extensions v4.12.2
├── torch v2.5.1+cpu (extra: cpu) (*)
├── accelerate v0.26.1 (extra: cu124) (*)
└── torch v2.5.1+cu124 (extra: cu124) (*)
(*) Package tree already displayed
```

We found a hack to pretend that there are 2 distinct packages `accelerate` for CPU and GPU by defining a separatte (but identic source):
```toml
accelerate = [
    { git = "https://github.com/huggingface/accelerate", tag = "v0.26.1", extra = "torch-cpu" },
]
```

Is it the expected behavior of the resolver? Or are we handling something incorrectly?

Thanks a lot.

---

_Comment by @harteros on 2024-12-09 09:56_

I think what you are looking at is https://github.com/astral-sh/uv/issues/9289 which is currently being developed at https://github.com/astral-sh/uv/pull/9370

---

_Comment by @jbcdnr on 2024-12-09 11:46_

Thank you @harteros for the pointer :)

---

_Closed by @jbcdnr on 2024-12-09 11:46_

---

_Comment by @lordsoffallen on 2024-12-27 16:43_

@harteros Is this solved? Cuz i'm having the same problems with it. Here is what I had to put in the configuration:

```
requires-python = ">=3.10"
dependencies = [
    "anthropic>=0.42.0",
    "datasets>=3.2.0",
    "diffusers>=0.32.1",
    "elevenlabs>=1.50.3",
    "google-cloud-aiplatform>=1.75.0",
    "kedro>=0.19.10",
    "kedro-datasets>=6.0.0",
    "lxml>=5.3.0",
    "markdownify>=0.14.1",
    "matplotlib>=3.10.0",
    "numpy>=2.2.1",
    "openai>=1.58.1",
    "pandas>=2.2.3",
    "python-dotenv>=1.0.1",
    "replicate>=1.0.4",
    "sentencepiece>=0.2.0",
    "structlog>=24.4.0",
    "tenacity>=9.0.0",
    "transformers>=4.47.1",
]

[project.optional-dependencies]
cpu = [
    "torch>=2.5.1",
    "torchvision>=0.20.1",
    "accelerate>=1.2.1",
    "peft>=0.14.0",
    "xformers>=0.0.28",
]
gpu = [
    "torch>=2.5.1",
    "torchvision>=0.20.1",
    "accelerate>=1.2.1",
    "peft>=0.14.0",
    "xformers>=0.0.28",
]

[dependency-groups]
dev = [
    "ipywidgets>=8.1.5",
    "jupyter>=1.1.1",
    "jupyter-archive>=3.4.0",
    "jupyterlab>=4.3.4",
    "notebook>=7.3.2",
    "pytest>=8.3.4",
]


# UV
[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv]
conflicts = [
    [
        { extra = "cpu" },
        { extra = "gpu" },
    ],
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cu124", extra = "gpu", marker = "platform_system == 'Linux'" },
    { index = "pytorch", extra = "cpu", marker = "platform_system == 'Linux'" }
]
torchvision = [
    { index = "pytorch-cu124", extra = "gpu", marker = "platform_system == 'Linux'" },
    { index = "pytorch", extra = "cpu", marker = "platform_system == 'Linux'" }
]
```

Every torch dependent package is now being listed under certain extras to make sure we don't install CUDA libs on cpu env. 

---
