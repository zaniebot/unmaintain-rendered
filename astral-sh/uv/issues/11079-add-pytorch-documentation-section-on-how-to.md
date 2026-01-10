---
number: 11079
title: Add pytorch documentation section on how to install for Intel GPUs
type: issue
state: closed
author: natemcintosh
labels:
  - documentation
assignees: []
created_at: 2025-01-29T19:32:08Z
updated_at: 2025-01-30T18:48:54Z
url: https://github.com/astral-sh/uv/issues/11079
synced_at: 2026-01-10T01:25:01Z
---

# Add pytorch documentation section on how to install for Intel GPUs

---

_Issue opened by @natemcintosh on 2025-01-29 19:32_

### Summary

Pytorch 2.6 [adds support for Intel GPUs](https://pytorch.org/docs/main/notes/get_start_xpu.html). The current [uv pytorch install docs](https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index) include instructions for:
- CPU only
- Various CUDA versions
- ROCm

But not yet for Intel GPUs.

My current attempt at mimicking the docs for existing GPUs is
```pyproject.toml
# pyproject.toml
[project]
name = "pytorch-intel"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["torch>=2.6.0"]

[[tool.uv.index]]
name = "pytorch-intel-gpu"
url = "https://download.pytorch.org/whl/xpu"
explicit = true

[tool.uv.sources]
torch = [{ index = "pytorch-intel-gpu", marker = "platform_system == 'Linux'" }]

```

```python
# hello.py
import torch


print(torch.xpu.is_available())
```
Running `uv run hello.py` produces
```sh
  × No solution found when resolving dependencies for split (sys_platform
  │ == 'linux'):
  ╰─▶ Because there is no version of pytorch-triton-xpu{platform_machine
      == 'x86_64' and sys_platform == 'linux'}==3.2.0 and torch==2.6.0+xpu
      depends on pytorch-triton-xpu{platform_machine == 'x86_64'
      and sys_platform == 'linux'}==3.2.0, we can conclude that
      torch==2.6.0+xpu cannot be used.
      And because only the following versions of torch{sys_platform ==
      'linux'} are available:
          torch{sys_platform == 'linux'}<2.6.0
          torch{sys_platform == 'linux'}==2.6.0+xpu
      and your project depends on torch{sys_platform == 'linux'}>=2.6.0, we
      can conclude that your project's requirements are unsatisfiable.
```

### Example

_No response_

---

_Label `enhancement` added by @natemcintosh on 2025-01-29 19:32_

---

_Comment by @charliermarsh on 2025-01-30 00:48_

I think this works:

```toml
# pyproject.toml
[project]
name = "pytorch-intel"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["torch>=2.6.0", "pytorch-triton-xpu"]

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/xpu"
explicit = true

[tool.uv.sources]
torch = [{ index = "pytorch", marker = "sys_platform == 'linux'" }]
pytorch-triton-xpu = [{ index = "pytorch" }]
```

Can you give it a try?

---

_Label `enhancement` removed by @charliermarsh on 2025-01-30 00:48_

---

_Label `documentation` added by @charliermarsh on 2025-01-30 00:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-30 00:48_

---

_Comment by @natemcintosh on 2025-01-30 14:34_

Yes, this worked for me! 

I got a warning from pytorch that it couldn't initialize numpy. Adding numpy to the pyproject fixed the warning. 

---

_Comment by @charliermarsh on 2025-01-30 16:47_

Thanks, I'll add something to this effect to the docs.

---

_Referenced in [astral-sh/uv#11109](../../astral-sh/uv/pulls/11109.md) on 2025-01-30 18:44_

---

_Closed by @charliermarsh on 2025-01-30 18:48_

---

_Closed by @charliermarsh on 2025-01-30 18:48_

---
