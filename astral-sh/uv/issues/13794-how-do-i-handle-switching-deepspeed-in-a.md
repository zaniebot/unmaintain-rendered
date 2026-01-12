```yaml
number: 13794
title: How do I handle switching deepspeed in a pyproject.toml?
type: issue
state: open
author: 032004129xuzhiyong
labels:
  - question
assignees: []
created_at: 2025-06-03T03:40:10Z
updated_at: 2025-06-03T10:17:29Z
url: https://github.com/astral-sh/uv/issues/13794
synced_at: 2026-01-12T16:01:37Z
```

# How do I handle switching deepspeed in a pyproject.toml?

---

_@032004129xuzhiyong_

### Question

UV is not able to handle the deepspeed package, although it is able to configure torch/torchvision/torchaudio/torch-scatter/etc. to adapt to different cuda versions in a single pyproject.toml file (showing the extra field set to conflicts). I've tried different settings, including setting the group or extra configured as conflicts. it seems to install, but when I detect it using the ds_report tool that comes with deepspeed, I find that the torch that deepspeed relies on at build time gets overwritten with the same torch+cuda version. In summary, I can't use uv sync --extra cuxxx or uc sync --group cuxxx to switch between deepspeed packages that depend on different torch+cuda, even if I force the installation of a different version (but it is possible to switch between packages such as torch). I'm wondering what the problem is, whether it's because uv's cache recognises different cuda versions of torch with different suffixes but deepspeed doesn't, or whether it's because I've set a specific index for torch that deepspeed doesn't have (and it doesn't need).
My python version is cp312 with both cuda121 and cuda124 versions of the torch configured (2.5.1).
Here is my configuration before installing deepspeed.
```
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "accelerate>=1.7.0",
    "blobfile>=3.0.0",
    "charset-normalizer>=3.4.2",
    "datasets>=3.6.0",
    "einops>=0.8.1",
    "hdf5storage>=0.2.0",
    "markupsafe<3.0.2",
    "ogb>=1.3.6",
    "optuna>=4.3.0",
    "optuna-dashboard>=0.18.0",
    "peft>=0.15.2",
    "polars-u64-idx>=1.30.0",
    "polars[all]>=1.30.0",
    "python-benedict[all]>=0.34.1",
    "rdkit>=2025.3.2",
    "scikit-learn>=1.6.1",
    "sentencepiece>=0.2.0",
    "sympy==1.13.1",
    "tensorboard>=2.19.0",
    "tiktoken>=0.9.0",
    "torch-geometric>=2.6.1",
    "transformers>=4.52.4",
    "yarl>=1.20.0",
]

[project.optional-dependencies]
cu121 = [
    "pyg-lib>=0.4.0",
    "torch==2.5.1",
    "torch-cluster>=1.6.3",
    "torch-scatter>=2.1.2",
    "torch-sparse>=0.6.18",
    "torch-spline-conv>=1.2.2",
    "torchaudio==2.5.1",
    "torchvision==0.20.1",
]
cu124 = [
    "pyg-lib>=0.4.0",
    "torch==2.5.1",
    "torch-cluster>=1.6.3",
    "torch-scatter>=2.1.2",
    "torch-sparse>=0.6.18",
    "torch-spline-conv>=1.2.2",
    "torchaudio==2.5.1",
    "torchvision==0.20.1",
]

[tool.uv]
conflicts = [
  [
    {extra = "cu121"},
    {extra = "cu124"},
  ]
]
no-build-isolation-package = [
  "torch-cluster",
  "torch-scatter",
  "torch-sparse",
  "torch-spline-conv",
  "torchaudio",
  "torchvision"
]

[tool.uv.sources]
torch = [
  { index = "indexcu121", extra = "cu121" },
  { index = "indexcu124", extra = "cu124" },
]
torchvision = [
  { index = "indexcu121", extra = "cu121" },
  { index = "indexcu124", extra = "cu124" },
]
torchaudio = [
  { index = "indexcu121", extra = "cu121" },
  { index = "indexcu124", extra = "cu124" },
]
pyg-lib = [
  { index = "indexpygcu121", extra = "cu121" },
  { index = "indexpygcu124", extra = "cu124" },
]
torch-scatter = [
  { index = "indexpygcu121", extra = "cu121" },
  { index = "indexpygcu124", extra = "cu124" },
]
torch-sparse = [
  { index = "indexpygcu121", extra = "cu121" },
  { index = "indexpygcu124", extra = "cu124" },
]
torch-cluster = [
  { index = "indexpygcu121", extra = "cu121" },
  { index = "indexpygcu124", extra = "cu124" },
]
torch-spline-conv = [
  { index = "indexpygcu121", extra = "cu121" },
  { index = "indexpygcu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "indexcu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "indexcu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[[tool.uv.index]]
name = "indexpygcu124"
url = "https://data.pyg.org/whl/torch-2.5.0+cu124.html"
format = "flat"
explicit = true

[[tool.uv.index]]
name = "indexpygcu121"
url = "https://data.pyg.org/whl/torch-2.5.0+cu121.html"
format = "flat"
explicit = true
```

### Platform

Linux 5.4.0-214-generic x86_64 GNU/Linux

### Version

0.7.8

---

_Label `question` added by @032004129xuzhiyong on 2025-06-03 03:40_

---

_Comment by @konstin on 2025-06-03 09:23_

uv's cache assumes that it can reuse a build for deepspeed (same as pip does). In your case however, deepspeed gets compiles against a torch version, but a packaging tool like uv doesn't see that. `uv cache clean deepspeed` can remove the existing build from the cache. This is currently recognized as a major missing feature in the Python packaging ecosystem. 

---

_Comment by @032004129xuzhiyong on 2025-06-03 10:12_

I have another question: when I force two different versions of deepspeed and switch them with `uv sync --extra cuxxx`, uv seems to rely on the source environment's torch to build the deepspeed rather than the target environment's torch.

---

_Comment by @konstin on 2025-06-03 10:17_

This is also a known problem we're trying to solve.

---
