```yaml
number: 15425
title: "`uv sync` always redownloads pytoch even though I have a lockfile"
type: issue
state: closed
author: pshirshov
labels:
  - question
assignees: []
created_at: 2025-08-21T18:29:01Z
updated_at: 2025-08-21T18:33:58Z
url: https://github.com/astral-sh/uv/issues/15425
synced_at: 2026-01-12T16:02:10Z
```

# `uv sync` always redownloads pytoch even though I have a lockfile

---

_@pshirshov_

### Summary

This is my project file:

```
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
    "torch==2.8.0+rocm6.3",
    "torchvision==0.23.0+rocm6.3",
    "pytorch-triton-rocm==3.4.0",
    "transformers==4.55.3",
    "trl==0.21.0",
    "peft==0.17.1",
]

[tool.uv.sources]
pytorch-triton = { index = "pytorch-rocm" }
pytorch-triton-rocm = { index = "pytorch-rocm" }
torch = { index = "pytorch-rocm" }
torchvision = { index = "pytorch-rocm" }

[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.3"
explicit = true
```

Every time I re-create my virtualenv and run `uv sync` in it, `uv` re-downloads pytorch.

`uv sync -v` produces the following log:

[uv.lock.txt](https://github.com/user-attachments/files/21923525/uv.lock.txt)

```
DEBUG uv 0.8.2
DEBUG Found project root: `/home/pavel/work/safe/playq/sentiments/tuning`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/pavel/work/safe/playq/sentiments/tuning`
DEBUG No Python version file found in workspace: /home/pavel/work/safe/playq/sentiments/tuning
DEBUG Using Python request `>=3.12.0` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.12.0`
DEBUG Released lock at `/tmp/nix-shell.9LMEmR/uv-d0bf9b329ca21499.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: project @ file:///home/pavel/work/safe/playq/sentiments/tuning
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 52 packages in 0.60ms
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: peft==0.17.1
DEBUG Identified uncached distribution: pytorch-triton-rocm==3.4.0
DEBUG Identified uncached distribution: torch==2.8.0+rocm6.3
DEBUG Registry requirement already cached: torchvision==0.23.0+rocm6.3
DEBUG Registry requirement already cached: transformers==4.55.3
DEBUG Registry requirement already cached: trl==0.21.0
DEBUG Registry requirement already cached: accelerate==1.10.0
DEBUG Registry requirement already cached: huggingface-hub==0.34.4
DEBUG Registry requirement already cached: numpy==2.3.2
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: psutil==7.0.0
DEBUG Registry requirement already cached: pyyaml==6.0.2
DEBUG Registry requirement already cached: safetensors==0.6.2
DEBUG Registry requirement already cached: tqdm==4.67.1
DEBUG Registry requirement already cached: setuptools==80.9.0
DEBUG Registry requirement already cached: filelock==3.19.1
DEBUG Registry requirement already cached: fsspec==2025.3.0
DEBUG Registry requirement already cached: jinja2==3.1.6
DEBUG Registry requirement already cached: networkx==3.5
DEBUG Registry requirement already cached: sympy==1.14.0
DEBUG Registry requirement already cached: typing-extensions==4.14.1
DEBUG Registry requirement already cached: pillow==11.3.0
DEBUG Registry requirement already cached: regex==2025.7.34
DEBUG Registry requirement already cached: requests==2.32.5
DEBUG Registry requirement already cached: tokenizers==0.21.4
DEBUG Registry requirement already cached: datasets==4.0.0
DEBUG Registry requirement already cached: hf-xet==1.1.8
DEBUG Registry requirement already cached: markupsafe==3.0.2
DEBUG Registry requirement already cached: mpmath==1.3.0
DEBUG Registry requirement already cached: certifi==2025.8.3
DEBUG Registry requirement already cached: charset-normalizer==3.4.3
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: urllib3==2.5.0
DEBUG Registry requirement already cached: dill==0.3.8
DEBUG Registry requirement already cached: multiprocess==0.70.16
DEBUG Registry requirement already cached: pandas==2.3.2
DEBUG Registry requirement already cached: pyarrow==21.0.0
DEBUG Registry requirement already cached: xxhash==3.5.0
DEBUG Registry requirement already cached: aiohttp==3.12.15
DEBUG Registry requirement already cached: python-dateutil==2.9.0.post0
DEBUG Registry requirement already cached: pytz==2025.2
DEBUG Registry requirement already cached: tzdata==2025.2
DEBUG Registry requirement already cached: aiohappyeyeballs==2.6.1
DEBUG Registry requirement already cached: aiosignal==1.4.0
DEBUG Registry requirement already cached: attrs==25.3.0
DEBUG Registry requirement already cached: frozenlist==1.7.0
DEBUG Registry requirement already cached: multidict==6.6.4
DEBUG Registry requirement already cached: propcache==0.3.2
DEBUG Registry requirement already cached: yarl==1.20.1
DEBUG Registry requirement already cached: six==1.17.0
DEBUG Preserving seed package: pip==25.0.1
DEBUG No cache entry for: https://download.pytorch.org/whl/pytorch_triton_rocm-3.4.0-cp312-cp312-linux_x86_64.whl
DEBUG No cache entry for: https://download.pytorch.org/whl/rocm6.3/torch-2.8.0%2Brocm6.3-cp312-cp312-manylinux_2_28_x86_64.whl
Downloading torch (4.6GiB)
Downloading pytorch-triton-rocm (246.7MiB)
 Downloading pytorch-triton-rocm
```

`uv cache clean` doesn't help.

The fact that `torchvision` is cached is an extremely curious observation.

### Platform

NixOS x86_64

### Version

uv 0.8.2

### Python version

_No response_

---

_Label `bug` added by @pshirshov on 2025-08-21 18:29_

---

_Comment by @charliermarsh on 2025-08-21 18:30_

I think that's because the PyTorch index sets `cache-control: 'no-cache,no-store,must-revalidate'`. You can customize the cache control headers yourself with: https://docs.astral.sh/uv/concepts/indexes/#customizing-cache-control-headers

---

_Comment by @charliermarsh on 2025-08-21 18:30_

See: https://github.com/astral-sh/uv/issues/15303

---

_Label `bug` removed by @charliermarsh on 2025-08-21 18:30_

---

_Label `question` added by @charliermarsh on 2025-08-21 18:30_

---

_Comment by @pshirshov on 2025-08-21 18:33_

Holy shit it works, Thanks.

---

_Closed by @pshirshov on 2025-08-21 18:33_

---
