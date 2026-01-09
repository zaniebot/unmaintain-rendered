---
number: 16651
title: "`torch-tb-profiler==0.4.3` fails to install when specifying  `--torch-backend`"
type: issue
state: closed
author: lalith-AE30
labels:
  - bug
assignees: []
created_at: 2025-11-09T09:07:23Z
updated_at: 2025-11-10T15:26:14Z
url: https://github.com/astral-sh/uv/issues/16651
synced_at: 2026-01-07T13:12:19-06:00
---

# `torch-tb-profiler==0.4.3` fails to install when specifying  `--torch-backend`

---

_Issue opened by @lalith-AE30 on 2025-11-09 09:07_

### Summary

## Description
I've tested on both python `3.11` and `3.10` with `--torch-backend=` `cpu`,`cu118`,`auto`. All result in the same error.

##  Steps to Reproduce
Create a new project and sync or create a venv and attempt to install any version of `torch-tb-profiler` other than `0.1.0`.
```
# uv init
# uv sync
uv venv
uv pip install torch-tb-profiler==0.4.3 --torch-backend auto
```
## Expected Behaviour
`torch-tb-profiler==0.4.3` or any version other than `0.1.0` is installable when specifying `--torch-backend`.

## Actual Behaviour
The package fails to install.
```
uv pip install torch-tb-profiler==0.4.3 --torch-backend auto -v
DEBUG uv 0.9.8
DEBUG Acquired shared lock for `/home/lalith/.cache/uv`
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.11.14-linux-x86_64-gnu` at `/home/lalith/tmp/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.11.14 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: torch-tb-profiler==0.4.3
DEBUG Detected CUDA driver version from `nvidia-smi`: 581.80
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.14
DEBUG Solving with target Python version: >=3.11.14
DEBUG Adding direct dependency: torch-tb-profiler>=0.4.3, <0.4.3+
DEBUG No cache entry for: https://download.pytorch.org/whl/cu130/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu129/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu128/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu123/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu122/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu126/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu124/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu125/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu121/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu120/torch-tb-profiler/
DEBUG No netrc file found
DEBUG Acquired lock for `credentials store`
DEBUG Released lock at `/home/lalith/.local/share/uv/credentials/credentials.toml.lock`
DEBUG No credentials file found at /home/lalith/.local/share/uv/credentials/credentials.toml
DEBUG No cache entry for: https://download.pytorch.org/whl/cu118/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu115/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu116/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu117/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu114/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu113/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu112/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu111/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu110/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu100/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu91/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu102/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu101/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu92/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu90/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cu80/torch-tb-profiler/
DEBUG No cache entry for: https://download.pytorch.org/whl/cpu/torch-tb-profiler/
DEBUG Searching for a compatible version of torch-tb-profiler (>=0.4.3, <0.4.3+)
DEBUG No compatible version found for: torch-tb-profiler
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch-tb-profiler==0.4.3 and you require torch-tb-profiler==0.4.3, we can conclude that your requirements are unsatisfiable.
DEBUG Released lock at `/home/lalith/tmp/.venv/.lock`
DEBUG Released lock at `/home/lalith/.cache/uv/.lock`
```

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.9.8

### Python version

Python 3.11.14

---

_Label `bug` added by @lalith-AE30 on 2025-11-09 09:07_

---

_Renamed from "`torch-tb-profiler==0.4.3` fails to install when specifying  --torch-backend" to "`torch-tb-profiler==0.4.3` fails to install when specifying  `--torch-backend`" by @lalith-AE30 on 2025-11-09 09:12_

---

_Comment by @charliermarsh on 2025-11-09 19:02_

We should probably remove it from the list. It _is_ present on the PyTorch indexes, but only at that one version (seemingly).

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-09 19:02_

---

_Referenced in [astral-sh/uv#16655](../../astral-sh/uv/pulls/16655.md) on 2025-11-09 19:03_

---

_Closed by @charliermarsh on 2025-11-10 15:26_

---
