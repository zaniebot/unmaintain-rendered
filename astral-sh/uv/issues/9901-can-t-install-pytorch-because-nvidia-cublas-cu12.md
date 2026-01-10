---
number: 9901
title: "Can't install PyTorch because `nvidia-cublas-cu12` can't be installed"
type: issue
state: closed
author: AshkanArabim
labels:
  - question
assignees: []
created_at: 2024-12-15T01:57:25Z
updated_at: 2024-12-15T16:47:01Z
url: https://github.com/astral-sh/uv/issues/9901
synced_at: 2026-01-10T01:24:47Z
---

# Can't install PyTorch because `nvidia-cublas-cu12` can't be installed

---

_Issue opened by @AshkanArabim on 2024-12-15 01:57_

I'm trying to create a docker container with PyTorch (aka `torch`). I closely followed [the documentation](https://docs.astral.sh/uv/guides/integration/pytorch/) but I just can't get it to work. I tried three methods, which I'll cover below

## method 1: installing using `uv add`
reproduction steps:
- `docker run -it ghcr.io/astral-sh/uv:python3.10-alpine /bin/sh`
- `cd home`
- `uv init`
- `uv add torch --index https://download.pytorch.org/whl/cu124` (note: host's CUDA version is 12.7)
- it returns this error:
```
/home # uv add torch --index https://download.pytorch.org/whl/cu124
Resolved 24 packages in 3.16s
error: Distribution `nvidia-cublas-cu12==12.4.5.8 @ registry+https://download.pytorch.org/whl/cu124` can't be installed because it doesn't have a source distribution or wheel for the current platform
/home # 
```

## method 2: installing using a cusom `pyproject.toml` file with custom indices (just like the docs)
reproduction steps:
- `docker run -it ghcr.io/astral-sh/uv:python3.10-alpine /bin/sh`
- `cd home`
- `uv init`
- modify `pyproject.toml` to the following:
```
[project]
name = "home"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
        "torch>=2.5.0",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cu124" },
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```
- `uv sync`
- returns this error:
```
/home # uv sync
Resolved 24 packages in 913ms
error: Distribution `nvidia-cublas-cu12==12.4.5.8 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

## method 3: directly using pytorch's installation guide using the pip interface 
Here's the [link](https://pytorch.org/) to PyTorch's installation guide. I clicked on "Stable" > "Linux" > "Pip" > "Python" > "Cuda 12.4".

reproduction steps:
- `docker run -it ghcr.io/astral-sh/uv:python3.10-alpine /bin/sh`
- `cd home`
- `uv init`
- `uv venv`
- `source .venv/bin/activate`
- `uv pip install torch torchvision torchaudio`
- returns this error
```
/home # uv pip install torch torchvision torchaudio
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
/home # uv venv
Using CPython 3.10.16 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv
/home # source .venv/bin/activate
(home) /home # uv pip install torch torchvision torchaudio
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torch are available:
          torch==1.0.0
          torch==1.0.1
          torch==1.1.0
          torch==1.2.0
          torch==1.3.0
          torch==1.3.1
          torch==1.4.0
          torch==1.5.0
          torch==1.5.1
          torch==1.6.0
          torch==1.7.0
          torch==1.7.1
          torch==1.8.0
          torch==1.8.1
          torch==1.9.0
          torch==1.9.1
          torch==1.10.0
          torch==1.10.1
          torch==1.10.2
          torch==1.11.0
          torch==1.12.0
          torch==1.12.1
          torch==1.13.0
          torch==1.13.1
          torch==2.0.0
          torch==2.0.1
          torch==2.1.0
          torch==2.1.1
          torch==2.1.2
          torch==2.2.0
          torch==2.2.1
          torch==2.2.2
          torch==2.3.0
          torch==2.3.1
          torch==2.4.0
          torch==2.4.1
          torch==2.5.0
          torch==2.5.1
      and torch<=1.10.1 has no wheels with a matching Python ABI tag, we can conclude that torch<=1.10.1 cannot be used.
      And because torch>=1.10.2 has no wheels with a matching platform tag and you require torch, we can conclude that your requirements are unsatisfiable.
```

I may have missed something in the docs. If that's the case please point me to the right direction and I'll take it from there.

Thanks in advance!

---
EDIT: Host information:
- Ubuntu noble 24.04 x86_64
- Linux 6.8.0-49-generic
- Docker version 27.3.1, build ce12230
- GPU: Nvidia RTX 3060
- CUDA Version: 12.7
- GPU Driver Version: 565.57.01

---

_Comment by @charliermarsh on 2024-12-15 02:07_

I think this is because you're on Alpine? But PyTorch doesn't publish musl wheels, which you'd need. See https://download.pytorch.org/whl/cu124/nvidia-cublas-cu12.

---

_Label `question` added by @charliermarsh on 2024-12-15 14:47_

---

_Comment by @charliermarsh on 2024-12-15 14:47_

(So, the fix here is to use a non-Alpine container.)

---

_Comment by @AshkanArabim on 2024-12-15 16:44_

Thanks! My original pyproject.toml works just fine on Debian.

You can mark this issue as resolved.
```
[project]
name = "home"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
        "torch>=2.5.0",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cu124" },
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

```

---

_Closed by @AshkanArabim on 2024-12-15 16:47_

---
