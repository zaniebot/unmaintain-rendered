---
number: 10283
title: Resolve order unclear (maybe bug) with multiple conflicting groups
type: issue
state: closed
author: TheMrCodes
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-01-03T11:36:22Z
updated_at: 2025-01-05T20:05:12Z
url: https://github.com/astral-sh/uv/issues/10283
synced_at: 2026-01-07T13:12:18-06:00
---

# Resolve order unclear (maybe bug) with multiple conflicting groups

---

_Issue opened by @TheMrCodes on 2025-01-03 11:36_

## Goal
A deep learning project that should use the same code with multiple hardware dependent library dependencies. 
Only one group of dependencies should be present at one point in time. Examples for groups are CPU only, CUDA 12 dependencies or Intel XPU Libraries.

## Approach
My approach was to model the dependencies as groups called cpu, cuda124, cpu-intel & gpu-intel and only install one group of dependencies for one device at a time using the sync command f.E.: `uv sync --only-group cpu`

## Issue
The sync command whats to install the 'intel-extension-for-pytorch==2.5.10+xpu' dependency when run. This should only be installed when using the 'gpu-intel' group.



> **Personal Note:** 
> Maybe I misunderstood the concept of dependency groups but if so, how could I achieve my goal using uv's features? 
> Thx for your help beforehand!
---

PyProject File:

``` pyproject.toml
[project]
name = "experiments"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "TheMrCodes" }
]
requires-python = ">=3.11"
dependencies = [
    "accelerate>=1.2.1",
    "datasets>=2.14.4",
    "evaluate>=0.4.3",
    "ipykernel>=6.29.5",
    "ipywidgets>=8.1.5",
    "scikit-learn>=1.6.0",
    "setuptools>=75.6.0",
    "tensorboard>=2.18.0",
    "tqdm>=4.67.1",
    "transformers>=4.47.1",
    "ujson>=5.10.0",
]

[project.scripts]
ma-ttc-project = "experiments:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
default-groups = ["cpu"]
conflicts = [ [
    { group = "cpu" },
    { group = "cuda124" },
    { group = "cpu-intel" },
    { group = "gpu-intel" },
] ]

[dependency-groups]
cpu = [
    "torch==2.5.0",
    "torchvision==0.20.0",
]
cuda124 = [
    "torch==2.5.0",
    "torchvision==0.20.0",
]
cpu-intel = [
    "intel-extension-for-pytorch==2.5.0",
    "oneccl-bind-pt==2.5.0",
    "torch==2.5.0",
    "torchvision==0.20.0",
]
gpu-intel = [
    "intel-extension-for-pytorch==2.5.10+xpu",
    "oneccl-bind-pt==2.5.0+xpu",
    "torch==2.5.1+cxx11.abi",
    "torchvision==0.20.1+cxx11.abi",
]

[tool.uv.sources]
torch = [
    { index = "torch-cpu", group = "cpu" },
    { index = "torch-cpu", group = "cpu-intel" },
    { index = "torch-intel-gpu", group = "gpu-intel" },
    { index = "torch-cuda124", group = "cuda124" },
]
torchvision = [
    { index = "torch-cpu", group = "cpu" },
    { index = "torch-cpu", group = "cpu-intel" },
    { index = "torch-intel-gpu", group = "gpu-intel" },
    { index = "torch-cuda124", group = "cuda124" },
]
intel-extension-for-pytorch = [
    { index = "torch-intel-gpu", group = "gpu-intel" },
]
oneccl-bind-pt = [
    { index = "torch-intel-cpu", group = "cpu-intel" },
    { index = "torch-intel-gpu", group = "gpu-intel" },
]


[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-cuda124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "torch-intel-cpu"
url = "https://pytorch-extension.intel.com/release-whl/stable/cpu/cn/"
explicit = true

[[tool.uv.index]]
name = "torch-intel-gpu"
url = "https://pytorch-extension.intel.com/release-whl/stable/xpu/cn/"
explicit = true
```

---

_Comment by @charliermarsh on 2025-01-03 17:25_

What's the exact error that you're seeing? Can you include the output?

One thing to note is that `intel-extension-for-pytorch` still has to be downloaded when _locking_ the project, because we lock for all groups. So `uv sync --only-group cpu` may still show some output for `intel-extension-for-pytorch`.

---

_Comment by @charliermarsh on 2025-01-03 17:26_

E.g., this works fine for me:

```
❯ uv sync --only-group cpu --python 3.11
Using CPython 3.11.10
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 155 packages in 0.91ms
Prepared 7 packages in 2.43s
Installed 12 packages in 244ms
 + filelock==3.16.1
 + fsspec==2024.9.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + numpy==2.2.1
 + pillow==11.1.0
 + sympy==1.13.1
 + torch==2.5.0
 + torchvision==0.20.0
 + typing-extensions==4.12.2
```

---

_Label `question` added by @charliermarsh on 2025-01-03 17:26_

---

_Label `needs-mre` added by @charliermarsh on 2025-01-03 17:26_

---

_Comment by @TheMrCodes on 2025-01-05 11:06_

Ok then this is mostly a misunderstanding on my end not a uv problem.

### For my Understanding
The `uv sync` not only updates the virtual environment (installing dependencies into the venv), but also always updates the lock with pyproject files
 -> wich means updating the lock file requires knowing the hash of all packages
 -> uv always downloads all packages to check the hash, on every `sync` and `run` not only on `add` and `lock`
Is this right? My goal was to not always wast time and GBs on package downloads every time I setup the project on a new VM.

But this would mean I can not do that in my development environment, only in production using the --frozen flag and then this also means when running my script I always have to call `uv run --frozen --group cuda124 src/mytrainingscript.py` es it would install all dependencies.

### Questions
Why is this default behaviour? 

With my intuition from other package managers:
 - `sync` should update the venv (like `pip install -r requirements.txt`)
 - `run` should only run files in the given venv with the preselected setting (like python version)
 - `lock` and `update --all` should regenerate the whole lock file with the constraints in pyproject.toml
 - `add <package>` and `update <package>` should only touch the lock entries effected by the changes made to the installed or updates package and there dependencies

This is in my eyes a bad default behavior when working in a team because every feature commit then updates the lock file which leads to merge conflicts. 

Love the ease of use and performance that uv provides so I would like to continue using it,
would therefor be great if this could be configured in the project settings or as a environment variable.

---

_Comment by @charliermarsh on 2025-01-05 19:58_

Yes, `uv sync` will ensure that the lockfile is up-to-date, unless you run with `--frozen`. This is wholly intentional. When you have an existing `uv.lock` file, it will only be updated if your dependencies change. We do not do a full re-resolution or change any of the dependencies, unless your own dependencies have changed. As such, `uv sync` typically does not update the lockfile -- it just checks if it's up-to-date.

> uv always downloads all packages to check the hash, on every sync and run not only on add and lock

We validate the hash when you run `sync` -- we validate that the hash in the lockfile matches the hash of the _actual_ file. This is actually the only meaningful time to validate hashes. We have to make sure that the hashes provided by the registry line up with what we expected.


---

_Comment by @charliermarsh on 2025-01-05 19:59_

To put it succinctly: `uv sync` will ensure that the lockfile is up-to-date. This is typically a ~no-op -- it just requires cross-referencing your own dependencies with the lockfile. But it is very much intentional that `uv sync` and `uv run` ensure that your environment is correct by default.


---

_Closed by @charliermarsh on 2025-01-05 19:59_

---

_Comment by @TheMrCodes on 2025-01-05 20:05_

Thank you for clarifying the philosophy behind that design decision.

---
