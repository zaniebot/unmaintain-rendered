---
number: 11442
title: Two versions of numpy installed at the same time
type: issue
state: closed
author: Ramlaoui
labels:
  - bug
  - duplicate
assignees: []
created_at: 2025-02-12T11:29:42Z
updated_at: 2025-02-12T14:17:01Z
url: https://github.com/astral-sh/uv/issues/11442
synced_at: 2026-01-10T01:25:05Z
---

# Two versions of numpy installed at the same time

---

_Issue opened by @Ramlaoui on 2025-02-12 11:29_

### Summary

Hello! Thanks for adding the functionality for conflicting dependencies. I have an issue however with Numpy, where the two conflicting versions are being installed and the older one is used when I boot up Python.

Here is my `pyproject.toml` file:

```toml
[project]
readme = "README.md"
authors = []
requires-python = ">=3.11"
dependencies = [
    "hydra-core>=1.3.2",
    "rich>=13.9.4",
    "torch>=2.4.0",
    "tqdm>=4.67.1",
    "pandas>=2.2.3",
    "pymatgen>=2024.11.13",
    "datasets>=3.2.0",
    "lightning>=2.5.0.post0",
    "mlflow>=2.20.1",
    "ase>=3.24.0",
    "torch_geometric>=2.4.0",
    # Linux dependencies
    "pyg-lib>=0.4.0; sys_platform != 'darwin'",
    "torch_scatter==2.1.2+pt24cu124; sys_platform != 'darwin'",
    "torch-sparse==0.6.18+pt24cu124; sys_platform != 'darwin'",
    "torch-cluster==1.6.3+pt24cu124; sys_platform != 'darwin'",
    "torch-spline-conv==1.2.2+pt24cu124; sys_platform != 'darwin'",
    # macOS dependencies
    "torch_scatter==2.1.2; sys_platform == 'darwin'",
    "torch-sparse==0.6.18; sys_platform == 'darwin'",
    "torch-cluster==1.6.3; sys_platform == 'darwin'",
    "torch-spline-conv==1.2.2; sys_platform == 'darwin'",
]

[build-system]
requires = ["hatchling", "setuptools"]
build-backend = "hatchling.build"

[project.optional-dependencies]
compile = [
    "pyg-lib>=0.4.0"
]
fairchem = [
    "fairchem-core>=1.4.0",
    "pyg-lib>=0.4.0",
    "e3nn>=0.5.0",
    "numpy==1.26.4",
]
mace = [
    "mace-torch>=0.3.10",
    "pyg-lib>=0.4.0",
    "e3nn==0.4.4",
    "numpy==2.2.2",
]

[tool.uv.sources]
pyg_lib = [{ git = "https://github.com/pyg-team/pyg-lib.git", marker = "sys_platform == 'darwin'" }]
torch = [
    { index = "torch-cu124", marker = "sys_platform != 'darwin'" },
    { index = "torch-cpu", marker = "sys_platform == 'darwin'" },
]

[[tool.uv.index]]
name = "torch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv]
dev-dependencies = [
    "ipython>=8.29.0",
    "ipdb>=0.13.13",
    "ruff>=0.7.1",
    "pytest>=8.3",
    "shibuya>=2024.10.15",
    "sphinx-autoapi>=3.3.2",
    "sphinx-autodoc-typehints>=2.5.0",
    "sphinx-code-include>=1.4.0",
    "sphinx-copybutton>=0.5.2",
    "sphinx-design>=0.6.1",
    "sphinx-math-dollar>=1.2.1",
    "sphinxawesome-theme>=5.3.2",
    "pre-commit>=4.0.1",
    "beautifulsoup4>=4.12.3",
    "lxml>=5.3.0",
    "requests>=2.32.3",
]
# these packages use torch, so we need to build them in the same environment
no-build-isolation-package = ["pyg-lib", "torch-scatter", "torch-sparse", "torch-cluster", "torch-spline-conv"]
find-links = [
  "https://data.pyg.org/whl/torch-2.4.0+cu124.html",
  "https://data.pyg.org/whl/torch-2.4.0+cpu.html",
]
conflicts = [
    # Conflict with e3nn and numpy
    [
        { extra = "fairchem" },
        { extra = "mace" },
    ]
]

[tool.ruff.lint]
extend-select = ["I"]
```

The problem is that when I run 
```bash
$ uv sync
$ uv sync --extra mace
```

I get both numpy versions:
```bash
 ~ numpy==1.26.4
 ~ numpy==2.2.2
```

And they get reinstalled every time I do `uv sync --extra mace`.

```bash
Python 3.11.9 (main, Apr  2 2024, 08:25:04) [Clang 15.0.0 (clang-1500.3.9.4)]
Type 'copyright', 'credits' or 'license' for more information
IPython 8.32.0 -- An enhanced Interactive Python. Type '?' for help.

[ins] In [1]: import numpy
numpy
[ins] In [2]: numpy.__version__
Out[2]: '1.26.4'
```

Thanks!



### Platform

macOS 14 arm64

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

### Python version

Python 3.11

---

_Label `bug` added by @Ramlaoui on 2025-02-12 11:29_

---

_Assigned to @BurntSushi by @konstin on 2025-02-12 12:01_

---

_Comment by @konstin on 2025-02-12 13:13_

Are you able to share your `uv.lock`, too?

---

_Comment by @BurntSushi on 2025-02-12 13:14_

OK, so I was able to reproduce this, but your `pyproject.toml` as given didn't work for me. I had to add a `name` and `version` to your `[project]` section. I also had to remove your `[build-system]` _and_ manually install `setuptools` into the environment. Once I did that, I could see two different versions of `numpy` being installed:

```
$ rm -rf uv.lock .venv && uv-0.5.29 venv && uv-0.5.29 pip install setuptools && uv-0.5.29 sync -p3.12 --extra mace 2>&1 | rg numpy
Using CPython 3.13.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 1 package in 2ms
Installed 1 package in 14ms
 + setuptools==75.8.0
 + numpy==1.26.4
 + numpy==2.2.2
```

I wondered if #11323 fixed this, so I ran with `uv 0.5.30` (the latest release, where as it looks like you're using `0.5.29`):

```
$ rm -rf uv.lock .venv && uv venv && uv pip install setuptools && uv sync -p3.12 --extra mace 2>&1 | rg numpy
Using CPython 3.13.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 1 package in 18ms
Installed 1 package in 13ms
 + setuptools==75.8.0
 + numpy==2.2.2
```

And indeed, it looks like this bug has been fixed. :-)

---

_Closed by @BurntSushi on 2025-02-12 13:14_

---

_Closed by @BurntSushi on 2025-02-12 13:15_

---

_Label `duplicate` added by @BurntSushi on 2025-02-12 13:15_

---

_Comment by @Ramlaoui on 2025-02-12 14:16_

Amazing, thanks a lot!

---
