---
number: 11548
title: "Strip conflict markers in `uv export`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-02-16T03:39:16Z
updated_at: 2025-02-20T20:19:48Z
url: https://github.com/astral-sh/uv/issues/11548
synced_at: 2026-01-07T13:12:18-06:00
---

# Strip conflict markers in `uv export`

---

_Issue opened by @charliermarsh on 2025-02-16 03:39_

Given:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = ["torch"]

[project.optional-dependencies]
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu124 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
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
torchvision = [
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
```

Then `uv export --extra cpu` includes markers like:

```
triton==3.2.0 ; (platform_machine == 'x86_64' and sys_platform == 'linux' and extra != 'extra-7-project-cpu') or (extra == 'extra-7-project-cpu' and extra == 'extra-7-project-cu124') \
    --hash=sha256:8d9b215efc1c26fa7eefb9a157915c92d52e000d2bf83e5f69704047e63f125c \
    --hash=sha256:e5dfa23ba84541d7c0a531dfce76d8bcd19159d50a4a8b14ad01e91734a5c1b0
```

---

_Label `bug` added by @charliermarsh on 2025-02-16 03:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-16 18:12_

---

_Referenced in [astral-sh/uv#11562](../../astral-sh/uv/pulls/11562.md) on 2025-02-16 20:47_

---

_Referenced in [astral-sh/uv#11571](../../astral-sh/uv/pulls/11571.md) on 2025-02-17 03:21_

---

_Referenced in [astral-sh/uv#11513](../../astral-sh/uv/pulls/11513.md) on 2025-02-18 01:27_

---

_Referenced in [astral-sh/uv#11643](../../astral-sh/uv/pulls/11643.md) on 2025-02-19 22:29_

---

_Closed by @charliermarsh on 2025-02-20 20:19_

---

_Closed by @charliermarsh on 2025-02-20 20:19_

---
