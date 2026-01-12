```yaml
number: 9597
title: "[BUG] Failed to install specific pytorch cuda version with `uv pip`"
type: issue
state: closed
author: ISJDOG
labels:
  - question
assignees: []
created_at: 2024-12-03T05:44:21Z
updated_at: 2024-12-03T15:29:37Z
url: https://github.com/astral-sh/uv/issues/9597
synced_at: 2026-01-12T15:59:54Z
```

# [BUG] Failed to install specific pytorch cuda version with `uv pip`

---

_@ISJDOG_

The command is as followed: 

```shell
uv pip install --verbose torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu121
```

Console debug log:

```shell
DEBUG uv 0.5.2 (195f4b634 2024-11-14)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.0-windows-x86_64-none` at `D:\xxx\.venv\Scripts\python.exe` (active virtual environment)     
DEBUG Using Python 3.13.0 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: torchaudio==2.3.0
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: torch>=2.3.0, <2.3.0+
DEBUG Adding direct dependency: torchvision>=0.18.0, <0.18.0+
DEBUG Adding direct dependency: torchaudio>=2.3.0, <2.3.0+
DEBUG No cache entry for: https://download.pytorch.org/whl/cu121/torch/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu121/torchvision/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu121/torchaudio/
DEBUG Searching for a compatible version of torch (>=2.3.0, <2.3.0+)
DEBUG Searching for a compatible version of torch (>=2.3.0, <2.3.0+cu121 | >2.3.0+cu121, <2.3.0+)
DEBUG No compatible version found for: torch
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torch are available:
          torch<2.3.0
          torch>=2.3.0+cu121
      and torch==2.3.0+cu121 has no wheels with a matching Python ABI tag, we can conclude that torch==2.3.0 cannot be used.
      And because you require torch==2.3.0, we can conclude that your requirements are unsatisfiable.
DEBUG Released lock at `.venv\.lock`
```

---

_Comment by @charliermarsh on 2024-12-03 05:54_

This looks right to me -- you're using Python 3.13, but there are no Python 3.13 builds for PyTorch at those versions. You may want to use `--python 3.12` or similar.

---

_Label `question` added by @charliermarsh on 2024-12-03 05:54_

---

_Closed by @zanieb on 2024-12-03 15:29_

---
