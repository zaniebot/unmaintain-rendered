---
number: 9353
title: Cannot add torch dependent lib with Pytorch env following official guide.
type: issue
state: closed
author: sebastian-sz
labels:
  - question
assignees: []
created_at: 2024-11-22T13:27:10Z
updated_at: 2024-11-22T14:15:55Z
url: https://github.com/astral-sh/uv/issues/9353
synced_at: 2026-01-07T13:12:18-06:00
---

# Cannot add torch dependent lib with Pytorch env following official guide.

---

_Issue opened by @sebastian-sz on 2024-11-22 13:27_

Hi everyone. Thank you for the amazing work that has been done with uv and Pytorch integration.

I am following the [official documentation](https://github.com/astral-sh/uv/blob/main/docs/guides/integration/pytorch.md) to setup a Pytorch project. All works until I try to add a dependency that depends on Pytorch, e.g. `uv add torchmetrics`, which fails with:
```bash
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```
but running `uv pip install torchmetrics` works with no issues.

# To reproduce:
`pyproject.toml`
```toml
[project]
name = "uv-torchmetrics"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
]

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
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
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
And the command: `uv add torchmetrics`

Platform: Ubuntu 22.04
uv version: 0.5.3

If there is anything I could do to improve my workflow I'd appreciate the help!


---

_Comment by @charliermarsh on 2024-11-22 14:00_

I think what you ultimately want is this (you need to edit yourself, not via `uv add`):

```toml
[project]
name = "uv-torchmetrics"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
]

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
  "torchmetrics",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
  "torchmetrics",
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
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchmetrics = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
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

However, I think this is blocked on #9289. I'll come back to this once that issue is closed out.

---

_Label `bug` added by @charliermarsh on 2024-11-22 14:00_

---

_Comment by @charliermarsh on 2024-11-22 14:01_

Actually, sorry, I think what I posted above should work now?

---

_Label `bug` removed by @charliermarsh on 2024-11-22 14:01_

---

_Label `question` added by @charliermarsh on 2024-11-22 14:01_

---

_Comment by @sebastian-sz on 2024-11-22 14:15_

@charliermarsh You are absolutely right. The above works. Thank you for the swift response!

---

_Closed by @sebastian-sz on 2024-11-22 14:15_

---
