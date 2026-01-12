```yaml
number: 10718
title: uv using default pypi when pytorch nightly index is specified
type: issue
state: closed
author: rbavery
labels: []
assignees: []
created_at: 2025-01-17T17:47:17Z
updated_at: 2025-01-17T19:21:42Z
url: https://github.com/astral-sh/uv/issues/10718
synced_at: 2026-01-12T16:00:19Z
```

# uv using default pypi when pytorch nightly index is specified

---

_@rbavery_

I have the following toml where torch has a lower bound and is pinned to use the nightly index. From this previous issue, this index gets used if I don't specify a lower bound: https://github.com/astral-sh/uv/issues/10702#issue-2794300371

```
[project]
authors = [
    {name = "Ryan Avery", email = "ryan@wherobots.com"},
]
license = {text = "nos"}
requires-python = "<4.0,>=3.10"
dependencies = [
    "httpx<1.0.0,>=0.26.0",
    "shapely<3.0.0,>=2.0.2",
    "geopandas<1.0.0,>=0.14.2",
    "pystac-client<1.0.0,>=0.7.5",
    "pystac<2.0.0,>=1.9.0",
    "stac-model<1.0,>=0.2",
    "tqdm<5.0.0,>=4.66.1",
    "typer[all]<1.0.0,>=0.9.0",
    "rich<14.0.0,>=13.7.0",
    "boto3<2.0.0,>=1.34.134",
    "huggingface_hub"
]
name = "wbc-model"
version = "0.1.0"
description = "Recipes for creating model metadata hosted for WBC users."
readme = "README.md"
keywords = []
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "Operating System :: OS Independent",
    "Topic :: Software Development :: Libraries :: Python Modules",
    "License :: Other/Proprietary License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Private :: Do Not Upload",
]

[build-system]
requires = ["pdm-backend"]
build-backend = "pdm.backend"

# Keywords description https://python-poetry.org/docs/pyproject/#keywords
keywords = []  # UPDATEME with relevant keywords

# Pypi classifiers: https://pypi.org/classifiers/
classifiers = [  # UPDATEME with additional classifiers; remove last classifier to allow publishing on PyPI
  "Development Status :: 3 - Alpha",
  "Intended Audience :: Developers",
  "Operating System :: OS Independent",
  "Topic :: Software Development :: Libraries :: Python Modules",
  "License :: Other/Proprietary License",
  "Programming Language :: Python :: 3",
  "Programming Language :: Python :: 3.10",
  "Programming Language :: Python :: 3.11",
  "Private :: Do Not Upload"
]

[tool.uv]
dev-dependencies = [
    "mypy<2.0.0,>=1.0.0",
    "mypy-extensions<1.0.0,>=0.4.3",
    "pre-commit<3.0.0,>=2.21.0",
    "bandit<2.0.0,>=1.7.5",
    "safety<3.0.0,>=2.3.4",
    "jupyterlab<5.0.0,>=4.0.10",
    "flake8<6.0.0,>=5.0.4",
    "pydocstyle[toml]<7.0.0,>=6.2.0",
    "pydoclint<1.0.0,>=0.3.0",
    "pytest<8.0.0,>=7.2.1",
    "pytest-html<4.0.0,>=3.2.0",
    "pytest-cov<5.0.0,>=4.1.0",
    "pytest-mock<4.0.0,>=3.10.0",
    "pytest-timeout<3.0.0,>=2.2.0",
    "pytest-benchmark<5.0.0,>=4.0.0",
    "pytest-sugar<1.0.0,>=0.9.7",
    "pytest-click<2.0.0,>=1.1.0",
    "pytest-pikachu<2.0.0,>=1.0.0",
    "coverage<8.0.0,>=7.3.0",
    "ruff<1.0.0,>=0.1.7",
    "hypothesis>=6.122.3",
    "expecttest",
]

[dependency-groups]
#if any of these start to conflict: https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies
satlas-solar = [
    "satlas-solar==0.1.0"
]
eurosat = [
    "torchgeo<1.0.0,>=0.6.0",
    "lightning<3.0.0,>=2.1.3",
]
torchgpu = [
  "torch>=2.6",
  "torchvision",
  "pytorch-triton",
]

# uv pays attention to index order, this is prioritized
[[tool.uv.index]]
name = "pytorch-nightly-gpu"
url = "https://download.pytorch.org/whl/nightly/cu124"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-nightly-gpu", marker = "platform_system != 'Darwin'"}, # Add nightly source
]
torchvision = [
  { index = "pytorch-nightly-gpu", marker = "platform_system != 'Darwin'"}, # Add nightly source
]
pytorch-triton = [
    {index = "pytorch-nightly-gpu", marker = "platform_system != 'Darwin'"},
]
satlas-solar = { path = "model-forge/satlas/solar/" }

[tool.pdm.build]
includes = [
    "wbc_model",
]
```

I set the lower bound to `torch>=2.6` since nightlies for 2.7 are out. but I get a weird error that seems to be using the wrong index even though I specified in my toml

```
[[tool.uv.index]]
name = "pytorch-nightly-gpu"
url = "https://download.pytorch.org/whl/nightly/cu124"
explicit = true
```

```
→ uv sync --upgrade --group torchgpu --group satlas-solar
⠙ Resolving dependencies...                                                     warning: Missing version constraint (e.g., a lower bound) for `expecttest`
warning: Missing version constraint (e.g., a lower bound) for `torchvision`
warning: Missing version constraint (e.g., a lower bound) for `pytorch-triton`
warning: Missing version constraint (e.g., a lower bound) for `huggingface-hub`
  × No solution found when resolving dependencies for split (sys_platform
  │ == 'darwin'):
  ╰─▶ Because only torch{sys_platform == 'darwin'}<=2.5.1 is available and
      wbc-model:torchgpu depends on torch{sys_platform == 'darwin'}>=2.6, we
      can conclude that wbc-model:torchgpu's requirements are unsatisfiable.
      And because your project requires wbc-model:torchgpu, we can conclude
      that your project's requirements are unsatisfiable.
```

It seems like uv is only looking at default pypi to say that only torch<=2.5.1 is available. But on the nightly index, versions above 2.6 should be available, as seen [here ](https://github.com/astral-sh/uv/issues/10702#issue-2794300371) where I'm downloading torch 2.7 nightlies.

---

_Comment by @zanieb on 2025-01-17 18:28_

Can you please share a minimal example? It looks like there's a lot of extraneous content in that `pyproject.toml`.

---

_Comment by @charliermarsh on 2025-01-17 18:58_

You have `torch>=2.6` in `torchgpu`. But on Darwin, it has to resolve that using PyPI-only, since your index is marked as "only use this index on non-Darwin". And that version doesn't exist on PyPI. So you either need to change the constraint, make that dependency inapplicable on Darwin, or use the torch index for all platforms.

---

_Comment by @rbavery on 2025-01-17 19:12_

Ok thanks, removing the platform requirement for the torch index fixed this. I was running this on Ubuntu (non-Darwin right?) so I would expect the solver to not look to PyPI and to use the index.

---

_Comment by @zanieb on 2025-01-17 19:20_

The solver is platform independent https://docs.astral.sh/uv/concepts/resolution/#universal-resolution

---

_Comment by @rbavery on 2025-01-17 19:21_

I see, thanks 

---

_Closed by @rbavery on 2025-01-17 19:21_

---
