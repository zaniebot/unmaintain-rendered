---
number: 12357
title: "Does auto option in ``torch-backend`` preview only select the highest supported CUDA version?"
type: issue
state: closed
author: FishAlchemist
labels:
  - question
assignees: []
created_at: 2025-03-21T08:19:11Z
updated_at: 2025-03-22T15:53:25Z
url: https://github.com/astral-sh/uv/issues/12357
synced_at: 2026-01-07T13:12:18-06:00
---

# Does auto option in ``torch-backend`` preview only select the highest supported CUDA version?

---

_Issue opened by @FishAlchemist on 2025-03-21 08:19_

### Question
Ref: 
* https://github.com/astral-sh/uv/pull/12070

What I actually want to ask is, even if the Python version is not supported, will uv still choose to install the highest compatible version of CUDA?

### Command:
```
 uv pip install "torch<=2.4,>=2.0" --torch-backend=auto --preview --verbose --dry-run
```
When using Python 3.8, if I use **auto** for installation, uv will try to find wheels from CUDA 12.6, resulting in not finding any wheels, and the error messages are somewhat confusing
```
DEBUG uv 0.6.9 (3d9460278 2025-03-20)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.8.20-windows-x86_64-none` at `C:\Users\[user-name]\Documents\source\temp_rustpython\.venv\Scripts\python.exe` (virtual environment)
DEBUG Using Python 3.8.20 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: torch>=2.0, <=2.4
DEBUG Detected CUDA driver version from `nvidia-smi`: 572.83
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8.20
DEBUG Adding direct dependency: torch>=2.0, <=2.4+
DEBUG Acquired lock for `C:\Users\[user-name]\AppData\Local\uv\cache\simple-v15\index\d349d1d03fe1cebf\torch.lock`
DEBUG No cache entry for: https://download.pytorch.org/whl/cu126/torch/
DEBUG Released lock at `C:\Users\[user-name]\AppData\Local\uv\cache\simple-v15\index\d349d1d03fe1cebf\torch.lock`
DEBUG Searching for a compatible version of torch (>=2.0, <=2.4+)
DEBUG Searching for a compatible version of torch (>=2.0, <2.0.1 | >2.0.1, <=2.4+)
DEBUG Searching for a compatible version of torch (>2.0.0, <2.0.1 | >2.0.1, <=2.4+)
DEBUG No compatible version found for: torch
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torch are available:
          torch<=2.0.0
          torch==2.0.1
          torch>2.4
      and torch>=2.0.0,<=2.0.1 has no wheels with a matching platform tag (e.g., `win_amd64`), we can conclude that torch>=2.0.0,<=2.0.1 cannot be used.
      And because you require torch>=2.0,<=2.4, we can conclude that your requirements are unsatisfiable.

      hint: Wheels are available for `torch` (v2.0.1) on the following platform: `manylinux2014_aarch64`
DEBUG Released lock at `C:\Users\[user-name]\Documents\source\temp_rustpython\.venv\.lock`
```

You can follow this table to filter out CUDA versions that are not supported by the current Python version. If the resolver has difficulty doing this, perhaps you could also choose to inform the user of the lack of support in the error message.
https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix
Apart from the latest PyTorch version, I guess the supported Python versions are fixed

### Platform

Windows 11 x86-64

### Version

uv 0.6.9 (3d9460278 2025-03-20) | Python 3.8.20

---

_Label `question` added by @FishAlchemist on 2025-03-21 08:19_

---

_Renamed from "Does auto option in torch-backend preview only select the highest supported CUDA version?" to "Does auto option in ``torch-backend`` preview only select the highest supported CUDA version?" by @FishAlchemist on 2025-03-21 08:19_

---

_Assigned to @charliermarsh by @zanieb on 2025-03-21 13:38_

---

_Comment by @charliermarsh on 2025-03-21 14:03_

Yeah it should be trying each compatible version in sequence. This should be easy to fix. Thanks. 

---

_Referenced in [astral-sh/uv#12385](../../astral-sh/uv/pulls/12385.md) on 2025-03-22 00:37_

---

_Closed by @charliermarsh on 2025-03-22 15:53_

---

_Closed by @charliermarsh on 2025-03-22 15:53_

---
