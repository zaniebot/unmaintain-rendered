```yaml
number: 9295
title: Configuring accelerators with optional dependencies and PyPI
type: issue
state: closed
author: eginhard
labels:
  - bug
assignees: []
created_at: 2024-11-20T23:22:35Z
updated_at: 2024-11-21T03:26:44Z
url: https://github.com/astral-sh/uv/issues/9295
synced_at: 2026-01-10T04:36:20Z
```

# Configuring accelerators with optional dependencies and PyPI

---

_Issue opened by @eginhard on 2024-11-20 23:22_

Thank you for the quick fix for #9259! I have one more issue, but not sure if it's user error in this case. I removed the custom `pytorch-cu214` index from the example at https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies so that for the `cu214` extra uv should just install whatever it gets from PyPI, i.e. CPU for Mac and Windows.

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
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
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

Now running `uv sync --extra cpu` with uv 0.5.4 on Ubuntu results in:
```
Resolved 29 packages in 866ms
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

---

_Comment by @charliermarsh on 2024-11-20 23:36_

I think I see why this happening, though it's fairly complicated.

---

_Label `bug` added by @charliermarsh on 2024-11-20 23:37_

---

_Comment by @charliermarsh on 2024-11-20 23:43_

I think this might work?

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch===2.5.1 ; platform_system == 'Darwin'",
  "torch==2.5.1+cpu ; platform_system != 'Darwin'",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
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
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

---

_Comment by @charliermarsh on 2024-11-20 23:43_

But it should probably work as-is, with your configuration.

---

_Comment by @eginhard on 2024-11-20 23:52_

Yes, that works (when leaving out torchvision or I guess doing the same there). But then only a single version can be specified.

---

_Comment by @charliermarsh on 2024-11-21 00:06_

Yeah I gotta think on the right way to solve it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-21 01:08_

---

_Closed by @charliermarsh on 2024-11-21 03:26_

---

_Closed by @charliermarsh on 2024-11-21 03:26_

---
