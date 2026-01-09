---
number: 13961
title: Using different index based on --extra flag
type: issue
state: closed
author: 4rrw
labels:
  - question
assignees: []
created_at: 2025-06-11T08:29:28Z
updated_at: 2025-06-11T08:58:43Z
url: https://github.com/astral-sh/uv/issues/13961
synced_at: 2026-01-07T13:12:18-06:00
---

# Using different index based on --extra flag

---

_Issue opened by @4rrw on 2025-06-11 08:29_

### Question

Is it possible to point uv to use different index for a package based on a flag passed to uv sync?
I would like to achieve something like this:
```toml
[tool.uv.sources]
torch = [
  { index = "rocm", marker = "extra == 'rocm'" },
  { index = "pytorch-cpu", marker = "extra == 'cpu'" },
]


[[tool.uv.index]]
name = "rocm"
url = "https://repo.radeon.com/rocm/manylinux/rocm-rel-6.4.1/"

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
```
and then `uv sync --extra rocm` would install torch from rocm index.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @4rrw on 2025-06-11 08:29_

---

_Comment by @4rrw on 2025-06-11 08:58_

I found a solution. Example for cuda because this radeon index would not work because of file structure in it.
```toml
[project.optional-dependencies]
cpu = [
  "torch",
]
cu121 = [
  "torch",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu121" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu121", extra = "cu121" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true
```
`uv sync --extra cu121` would install cuda version

---

_Closed by @4rrw on 2025-06-11 08:58_

---
