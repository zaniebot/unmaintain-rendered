---
number: 15239
title: Warning that PySide6 is provided by more than one package since uv 0.8.7
type: issue
state: open
author: my1e5
labels:
  - question
assignees: []
created_at: 2025-08-12T14:12:33Z
updated_at: 2025-08-18T16:35:37Z
url: https://github.com/astral-sh/uv/issues/15239
synced_at: 2026-01-10T01:25:54Z
---

# Warning that PySide6 is provided by more than one package since uv 0.8.7

---

_Issue opened by @my1e5 on 2025-08-12 14:12_

### Question

Since uv version `0.8.7`, I'm getting a warning when using the PySide6 package:


> warning: The module `PySide6` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `pyside6-addons` (v6.9.0) or `pyside6` (v6.9.0).


Just wanted to understand how best to deal with it. When you install `pyside6`, it also installs `pyside6-addons` and `pyside6-essentials` for you. 
```
Installed 4 packages in 3.52s
 + pyside6==6.9.0
 + pyside6-addons==6.9.0
 + pyside6-essentials==6.9.0
 + shiboken6==6.9.0
```
So what can be done? Is this something that PySide6 needs to address?

### `0.8.6`
```bash
$ uvx --with uv==0.8.6 uv run -p 3.13 --reinstall --with pyside6==6.9.0 python -c "import PySide6; print(PySide6.__versi
on__)"
Installed 1 package in 42ms
Installed 4 packages in 3.52s
6.9.0
```

### `0.8.7`
```bash
$ uvx --with uv==0.8.7 uv run -p 3.13 --reinstall --with pyside6==6.9.0 python -c "import PySide6; print(PySide6.__version__)"
[2/4] pyside6==6.9.0   warning: The module `PySide6` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `pyside6-addons` (v6.9.0) or `pyside6` (v6.9.0).
[3/4] pyside6-addons==6.9.0   warning: The module `PySide6` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `pyside6-essentials` (v6.9.0) or `pyside6-addons` (v6.9.0).
Installed 4 packages in 3.46s
6.9.0
```

### Platform

Windows 10

### Version

uv 0.8.9 (68c0bf8a2 2025-08-11)

---

_Label `question` added by @my1e5 on 2025-08-12 14:12_

---

_Comment by @charliermarsh on 2025-08-12 15:00_

It might be a bug in the check; we'll take a look.

---

_Comment by @StefanDensow-keen on 2025-08-13 08:44_

In uv 0.8.9, I see the same issue with pyside6, but also with nvidia packages, which are dependencies of `torch v2.6.0+cu118`:
```
├── torch v2.6.0+cu118
...
│   ├── nvidia-cublas-cu11 v11.11.3.6
│   ├── nvidia-cuda-cupti-cu11 v11.8.87
│   ├── nvidia-cuda-nvrtc-cu11 v11.8.89
│   ├── nvidia-cuda-runtime-cu11 v11.8.89
│   ├── nvidia-cudnn-cu11 v9.1.0.70
│   │   └── nvidia-cublas-cu11 v11.11.3.6
│   ├── nvidia-cufft-cu11 v10.9.0.58
│   ├── nvidia-curand-cu11 v10.3.0.86
│   ├── nvidia-cusolver-cu11 v11.4.1.48
│   │   └── nvidia-cublas-cu11 v11.11.3.6
│   ├── nvidia-cusparse-cu11 v11.7.5.86
│   ├── nvidia-nccl-cu11 v2.21.5
│   ├── nvidia-nvtx-cu11 v11.8.86
```
Warnings:
```
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjpeg2k-cu11` (v0.9.0.43) or `nvidia-nccl-cu11` (v2.21.5).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjpeg2k-cu11` (v0.9.0.43) or `nvidia-cudnn-cu11` (v9.1.0.70).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cudnn-cu11` (v9.1.0.70) or `nvidia-cublas-cu11` (v11.11.3.6).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu11` (v11.8.86) or `nvidia-cublas-cu11` (v11.11.3.6).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu11` (v11.8.86) or `nvidia-nvimgcodec-cu11` (v0.5.0.13).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvimgcodec-cu11` (v0.5.0.13) or `nvidia-cuda-cupti-cu11` (v11.8.87).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-dali-cuda110` (v1.50.0) or `nvidia-cuda-cupti-cu11` (v11.8.87).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtiff-cu11` (v0.5.1.75) or `nvidia-dali-cuda110` (v1.50.0).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtiff-cu11` (v0.5.1.75) or `nvidia-curand-cu11` (v10.3.0.86).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusolver-cu11` (v11.4.1.48) or `nvidia-curand-cu11` (v10.3.0.86).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusolver-cu11` (v11.4.1.48) or `nvidia-cuda-runtime-cu11` (v11.8.89).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu11` (v10.9.0.58) or `nvidia-cuda-runtime-cu11` (v11.8.89).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvcomp-cu11` (v5.0.0.6) or `nvidia-cufft-cu11` (v10.9.0.58).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvcomp-cu11` (v5.0.0.6) or `nvidia-cusparse-cu11` (v11.7.5.86).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusparse-cu11` (v11.7.5.86) or `nvidia-cuda-nvrtc-cu11` (v11.8.89).
```

---

_Comment by @my1e5 on 2025-08-13 09:50_

This also happens with [`mlflow`](https://pypi.org/project/mlflow/)

```
$ uvx --with uv==0.8.7 uv run -p 3.13 --reinstall --with mlflow python -c "import mlflow; print(mlflow.__version__)"
warning: The module `mlflow` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `mlflow-tracing` (v3.2.0) or `mlflow` (v3.2.0).
warning: The module `mlflow` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `mlflow-skinny` (v3.2.0) or `mlflow` (v3.2.0).
Installed 74 packages in 7.85s
3.2.0

---

_Comment by @charliermarsh on 2025-08-13 09:57_

I think we'll roll this back while we figure out a way to improve the detection.

---

_Comment by @JelleZijlstra on 2025-08-13 14:15_

Looks like PySide6 and PySide6-addons produce the exact same `PySide6/__init__.py` file, so maybe uv could suppress the check if the two files are identical? If they're identical, the issue also seems significantly less harmful.

---

_Comment by @charliermarsh on 2025-08-13 14:19_

(I think it _is_ still a problem because if the user uninstalls `pyside6-addons`, it would leave you with a partial `pyside6` installation (missing files, but a complete `.dist-info`, confusing installers). I agree it's less harmful, though.)


---

_Comment by @JelleZijlstra on 2025-08-13 14:26_

You could also handle that case in `uv pip uninstall` I suppose, at a significant cost in complexity. You'd have to scan all other installed packages to see if someone else also provides a file in the package you're uninstalling.

---

_Comment by @charliermarsh on 2025-08-13 14:27_

Yeah, I've been tracking that here: #15238. We have to refcount the files or something.

---

_Comment by @zanieb on 2025-08-13 20:01_

In #15253 I've moved this behind a feature gate while we iterate on detection.

---

_Referenced in [astral-sh/uv#15253](../../astral-sh/uv/pulls/15253.md) on 2025-08-13 20:02_

---

_Comment by @konstin on 2025-08-18 14:42_

I've created an upstream bug: https://bugreports.qt.io/browse/PYSIDE-3161

---

_Referenced in [mlflow/mlflow#17267](../../mlflow/mlflow/issues/17267.md) on 2025-08-18 16:35_

---

_Comment by @konstin on 2025-08-18 16:35_

I've looked in the `uv pip install torch` case, the nvidia packages are fixed in a newer version, but torch still uses the older version, so we need to wait until torch updates.

I've also create an upstream bug for mlflow: https://github.com/mlflow/mlflow/issues/17267

---

_Referenced in [astral-sh/uv#15357](../../astral-sh/uv/issues/15357.md) on 2025-08-18 16:36_

---

_Referenced in [astral-sh/uv#15813](../../astral-sh/uv/issues/15813.md) on 2025-09-12 14:35_

---
