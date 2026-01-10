---
number: 14675
title: Issues with indexes when trying to enable CPU and GPU extras
type: issue
state: open
author: sunildkumar
labels:
  - bug
assignees: []
created_at: 2025-07-17T01:15:41Z
updated_at: 2025-09-09T16:45:32Z
url: https://github.com/astral-sh/uv/issues/14675
synced_at: 2026-01-10T01:25:47Z
---

# Issues with indexes when trying to enable CPU and GPU extras

---

_Issue opened by @sunildkumar on 2025-07-17 01:15_

### Summary

I'm trying to create a python package with a `cpu` and `gpu` extra. These extras define which extra from Roboflow's Trackers library to use (https://github.com/roboflow/trackers).

Here's the minimal version of my pyproject:
```toml
[project]
name = "demo-uv-roboflow-trackers-bug"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
cpu = [
    "torch",
    "torchvision", 
    "trackers[cpu] @ git+https://github.com/roboflow/trackers.git@main",
]
gpu = [
    "torch",
    "torchvision",
    "trackers[cu124] @ git+https://github.com/roboflow/trackers.git@main",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "gpu" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "gpu" },
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

`uv sync`, `uv sync --extra cpu`, and `uv sync --extra gpu` all fail with the same error: 
```bash
error: Requirements contain conflicting indexes for package `torch` in all marker environments:
- https://download.pytorch.org/whl/cpu
- https://download.pytorch.org/whl/cu124
```

I've created a demo repo where you can clone a reproducible version if that's helpful: https://github.com/sunildkumar/demo-uv-roboflow-trackers-bug



### Platform

Ubuntu 22.04.5 LTS

### Version

uv 0.7.21

### Python version

This fails with the error above. The project is >=3.12

---

_Label `bug` added by @sunildkumar on 2025-07-17 01:15_

---

_Comment by @CompRhys on 2025-09-08 22:15_

We have encountered similar issues did you find a workable solution?

---

_Comment by @sunildkumar on 2025-09-08 22:18_

nope not yet!

---

_Comment by @konstin on 2025-09-09 07:21_

This is a bug with how we handle source in URL dependencies. We can build an MRE by replacing trackers with a path dependency:

```toml
[project]
name = "demo-uv-roboflow-trackers-bug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch",
  "trackers[cpu]",
]
gpu = [
  "torch",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "gpu" },
]
trackers = { path = "trackers" }

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

```toml
[project]
name = "trackers"
version = "1.0.0"
requires-python = ">=3.9"

[project.optional-dependencies]
cpu = [
  "torch>=2.6.0",
]

cu124 = [
  "torch>=2.6.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

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

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["trackers"]
```

---

_Comment by @edaniels on 2025-09-09 14:15_

Yeah the issue I'm having is following https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies specifically in a workspace. When one package needs a GPU version and another needs a CPU version on the same platform/marker and we set the conflict between the packages, we get `Requirements contain conflicting indexes`. The only solution we have right now which feels bad is to install torch later with pip.

Aside, is this something pyx plans on doing better?

---

_Comment by @sunildkumar on 2025-09-09 16:45_

Thanks @konstin! 

---
