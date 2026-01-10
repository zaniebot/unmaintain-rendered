---
number: 11953
title: "uv sync removes `[project.dependencies]` when using scikit-build-core as the build backend"
type: issue
state: closed
author: mann-patwa
labels:
  - needs-mre
assignees: []
created_at: 2025-03-04T15:23:04Z
updated_at: 2025-03-17T10:58:26Z
url: https://github.com/astral-sh/uv/issues/11953
synced_at: 2026-01-10T01:25:13Z
---

# uv sync removes `[project.dependencies]` when using scikit-build-core as the build backend

---

_Issue opened by @mann-patwa on 2025-03-04 15:23_

### Question

### **Description**
When running `uv sync`, it installs the dependencies from the `[dependency-groups.dev]` section correctly but removes all dependencies listed in `[project.dependencies]`.

However, when I remove scikit-build-core as the build backend from `pyproject.toml` and then run `uv sync`, it correctly installs all dependencies, including `[project.dependencies]`.

This suggests that `uv sync` might not be handling `[project.dependencies]` properly when a build backend like scikit-build-core is specified.

### **Fixes Tried:**
Installed the `[project.dependencies]` by using `uv pip install -r pyproject.toml` which installs the dependencies in the .venv correctly, then ran `uv sync`, which uninstalled them from `.venv`.

added dependencies to the `[project.dependencies]` using `uv add some_package` which correctly added the dependencies to the `[project.dependencies]` entry in `pyprojoect.toml` but does not install it to `.venv`.
Is there a scenario where:

###  **Suspected Problem**
- `uv sync` be prioritizing --group dev and ignoring [project.dependencies] when a build backend is specified?
-  There is an issue with how scikit-build-core interacts with uv?


### Platform

macOs 15 arm64

### Version

uv 0.6.3

---

_Label `question` added by @mann-patwa on 2025-03-04 15:23_

---

_Comment by @zanieb on 2025-03-05 18:54_

> When running uv sync, it installs the dependencies from the [dependency-groups.dev] section correctly but removes all dependencies listed in [project.dependencies].

This sounds like the expected behavior if you don't have a `[build-system]` defined https://docs.astral.sh/uv/concepts/projects/config/#build-systems but you have `scikit-buid-core` setup?

Can you share your `pyproject.toml`? See https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/ for more guidance.

---

_Comment by @mann-patwa on 2025-03-06 12:15_

I did have the [build-system] set to `scikit-build-core`.


I tried to replicate the issue, and I noticed that when I added version = ["dynamic"] to the [project] section, the behavior was reproduced. However, when I removed version = ["dynamic"] and used a static version instead, uv sync worked as expected.
That was the only change I made, but I have no idea why it worked.

This was essentially my old `pyproject.toml` file for reference:


```python
[project]
name = "my-project"
#version = "0.1.0"
description = "3D medical imaging reconstruction software"
license = { file = "LICENSE.txt" }
dependencies = [
    "wxpython>=4.2.1",
    "pyopengl>=3.1.7",
    "pyopengl-accelerate>=3.1.7",
    "numpy>=1.26.4",
    "scipy>=1.13.0",
    "matplotlib>=3.8.4",
    "pillow>=10.3.0",
    "imageio>=2.34.1",
    "nibabel>=5.2.1",
    "scikit-image>=0.23.2",
    "python-gdcm>=3.0.24.1",
    "vtk>=9.3.0",
    "torch>=2.3.0",
    "cython>=3.0.10",
    "pypubsub>=4.0.3",
    "h5py>=3.11.0",
    "psutil>=5.9.8",
    "pyserial>=3.5",
    "pyacvd>=0.2.11",
    "setuptools>=69.5.1",
    "aioconsole>=0.7.1",
    "mido>=1.3.2",
    "nest-asyncio>=1.6.0",
    "python-rtmidi>=1.5.8",
    "python-socketio[client]>=5.11.2",
    "requests>=2.32.2",
    "uvicorn[standard]>=0.30.0",
    "scikit-learn>=1.5.0",
]
readme = "README.md"
requires-python = ">= 3.10"
dynamic = ["version"]


[build-system]
requires = ["scikit-build-core", "cython >= 3.0.2", "numpy >= 1.26.4"]
build-backend = "scikit_build_core.build"

[tool.scikit-build]
experimental = true
metadata.version.provider = "scikit_build_core.metadata.setuptools_scm"
sdist.include = ["invesalius/version.py"]

[tool.setuptools_scm]  # Section required
write_to = "invesalius/version.py"



```

---

_Comment by @charliermarsh on 2025-03-17 02:07_

I think it's going to be hard to help without a complete reproduction (like, a Git repo we can clone plus the steps we should run). It's just a lot of work to go from the above to reproducing the behavior, and we may not even be able to do so.

---

_Label `question` removed by @charliermarsh on 2025-03-17 02:07_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-17 02:07_

---

_Closed by @mann-patwa on 2025-03-17 10:58_

---
