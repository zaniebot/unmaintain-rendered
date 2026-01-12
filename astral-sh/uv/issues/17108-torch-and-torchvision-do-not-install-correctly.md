```yaml
number: 17108
title: torch and torchvision do not install correctly when multiple index are used
type: issue
state: open
author: ZidongLiu
labels:
  - question
assignees: []
created_at: 2025-12-12T19:05:36Z
updated_at: 2025-12-13T03:41:37Z
url: https://github.com/astral-sh/uv/issues/17108
synced_at: 2026-01-12T16:02:44Z
```

# torch and torchvision do not install correctly when multiple index are used

---

_@ZidongLiu_

### Summary

Here is my project.toml

```
[project]
name = "decomfl"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
	"transformers>=4.57.0",
	"accelerate",
	"datasets",
	"numpy",
	"peft",
	"tensorboardx",
	"tqdm",
	"pydantic",
	"pydantic-settings",
	"python-dotenv",
	"ruff==0.6",
]

[project.optional-dependencies]
dev = [
	"isort==5.13.2",
	"pytest==8.3",
	"mypy==1.11",
]
cuda = [
	"torch>=2.7.0",
	"torchvision>=0.22.0",
]
cpu = [
	"torch>=2.7.0",
	"torchvision>=0.22.0",
]

[tool.uv.sources]
# CUDA PyTorch sources - only used when --extra cuda is specified
# For CPU-only installation (--extra cpu), PyPI will be used instead
torch = { index = "pytorch-cu126", marker = "extra == 'cuda'" }
torchvision = { index = "pytorch-cu126", marker = "extra == 'cuda'" }

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

```

I tried `uv sync --extra cuda`, it could install torch and torchvision correctly

<img width="574" height="169" alt="Image" src="https://github.com/user-attachments/assets/98351b33-4b10-4399-a336-74dfebbcbe3f" />

But when I type `uv sync --extra cpu`, there are 2 unexpected behavior:
1. torchvision is not installed at all
2. torch still installs as cu126, while it should actually install from PyPI

<img width="566" height="210" alt="Image" src="https://github.com/user-attachments/assets/79a96431-dfc6-42e6-9ded-3c6eba2e3b43" />

### Platform

Windows 10

### Version

uv 0.9.17 (2b5d65e61 2025-12-09)

### Python version

3.10.19

---

_Label `bug` added by @ZidongLiu on 2025-12-12 19:05_

---

_Comment by @charliermarsh on 2025-12-13 03:00_

The use of `torch = { index = "pytorch-cu126", marker = "extra == 'cuda'" }` is incorrect. I think you're looking for:
```toml
torch = { index = "pytorch-cu126", extra = "cuda" }
```

You also need to tell uv to use conflicting (different) versions across the two extras:

```toml
[tool.uv]
conflicts = [
  [
    { extra = "cuda" },
    { extra = "cpu" },
  ],
]
```

---

_Comment by @charliermarsh on 2025-12-13 03:01_

(Note however that the Linux wheel on PyPI is not a CPU-only wheel; it includes CUDA.)

---

_Label `bug` removed by @charliermarsh on 2025-12-13 03:01_

---

_Label `question` added by @charliermarsh on 2025-12-13 03:01_

---

_Label `error messages` added by @charliermarsh on 2025-12-13 03:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-13 03:15_

---

_Comment by @charliermarsh on 2025-12-13 03:40_

Here's a complete working example:
```toml
[project]
name = "decomfl"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "transformers>=4.57.0",
    "accelerate",
    "datasets",
    "numpy",
    "peft",
    "tensorboardx",
    "tqdm",
    "pydantic",
    "pydantic-settings",
    "python-dotenv",
    "ruff==0.6",
]

[project.optional-dependencies]
dev = [
    "isort==5.13.2",
    "pytest==8.3",
    "mypy==1.11",
]
cuda = [
    "torch>=2.7.0",
    "torchvision>=0.22.0",
]
cpu = [
    "torch>=2.7.0",
    "torchvision>=0.22.0",
]

[tool.uv.sources]
torch = [{ index = "pytorch-cu126", extra = "cuda" }, { index = "pytorch-cpu", extra = "cpu" }]
torchvision = [{ index = "pytorch-cu126", extra = "cuda" }, { index = "pytorch-cpu", extra = "cpu" }]

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv]
conflicts = [
  [
    { extra = "cuda" },
    { extra = "cpu" },
  ],
]
```

---

_Label `error messages` removed by @charliermarsh on 2025-12-13 03:41_

---
