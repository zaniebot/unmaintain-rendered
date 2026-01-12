```yaml
number: 16368
title: "Switching torch extra between pypi & torch backend"
type: issue
state: open
author: tpgillam
labels: []
assignees: []
created_at: 2025-10-20T09:04:09Z
updated_at: 2025-12-14T21:22:27Z
url: https://github.com/astral-sh/uv/issues/16368
synced_at: 2026-01-12T16:02:30Z
```

# Switching torch extra between pypi & torch backend

---

_@tpgillam_

uv 0.9.4. Consider the following pyproject.toml in an otherwise empty directory:

```toml
[project]
name = "moo"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = ["numpy"]


[project.optional-dependencies]
cpu = [
  "torch>=2.8.0",
]
cu128 = [
  "torch>=2.8.0",
]
pypi = [
  "torch>=2.8.0",
]


[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu128" },
    { extra = "pypi" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu128", extra = "cu128" },
  { index = "pytorch-pypi", extra = "pypi" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[[tool.uv.index]]
name = "pytorch-pypi"
url = "https://pypi.org/simple/"
explicit = true
```

This is basically the same as the [documented example](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies), with the addition of the `pytorch-pypi` index as an option.

I've now run the following commands on a machine with a GPU:

```console
$ uv run --extra cpu python -c "import torch; print(torch.cuda.is_available())"
Using CPython 3.13.7
Creating virtual environment at: .venv
Installed 11 packages in 5.26s
False
$ uv run --extra cu128 python -c "import torch; print(torch.cuda.is_available())"
True
$ uv run --extra pypi python -c "import torch; print(torch.cuda.is_available())"
True
$ uv run --extra cpu python -c "import torch; print(torch.cuda.is_available())"
Uninstalled 1 package in 2.45s
Installed 1 package in 3.90s
False
$ uv run --extra pypi python -c "import torch; print(torch.cuda.is_available())"
False
```

A little inspection of the verbose logs shows that, when specifying `--extra pypi`, no package is added or removed. We can look at the syncs to see a bit more explicitly:


```console
$ rm -rf .venv && uv sync --extra pypi
Using CPython 3.13.7
Creating virtual environment at: .venv
Resolved 31 packages in 0.65ms
Installed 27 packages in 4.70s
 + filelock==3.20.0
 + fsspec==2025.9.0
 + jinja2==3.1.6
 + markupsafe==3.0.3
 + mpmath==1.3.0
 + networkx==3.5
 + numpy==2.3.4
 + nvidia-cublas-cu12==12.8.4.1
 + nvidia-cuda-cupti-cu12==12.8.90
 + nvidia-cuda-nvrtc-cu12==12.8.93
 + nvidia-cuda-runtime-cu12==12.8.90
 + nvidia-cudnn-cu12==9.10.2.21
 + nvidia-cufft-cu12==11.3.3.83
 + nvidia-cufile-cu12==1.13.1.3
 + nvidia-curand-cu12==10.3.9.90
 + nvidia-cusolver-cu12==11.7.3.90
 + nvidia-cusparse-cu12==12.5.8.93
 + nvidia-cusparselt-cu12==0.7.1
 + nvidia-nccl-cu12==2.27.5
 + nvidia-nvjitlink-cu12==12.8.93
 + nvidia-nvshmem-cu12==3.3.20
 + nvidia-nvtx-cu12==12.8.90
 + setuptools==80.9.0
 + sympy==1.14.0
 + torch==2.9.0
 + triton==3.5.0
 + typing-extensions==4.15.0
$ uv sync --extra pypi
Resolved 31 packages in 2ms
Audited 27 packages in 13ms
$ uv sync --extra cpu
Resolved 31 packages in 1ms
Uninstalled 17 packages in 3.05s
Installed 1 package in 4.31s
 - nvidia-cublas-cu12==12.8.4.1
 - nvidia-cuda-cupti-cu12==12.8.90
 - nvidia-cuda-nvrtc-cu12==12.8.93
 - nvidia-cuda-runtime-cu12==12.8.90
 - nvidia-cudnn-cu12==9.10.2.21
 - nvidia-cufft-cu12==11.3.3.83
 - nvidia-cufile-cu12==1.13.1.3
 - nvidia-curand-cu12==10.3.9.90
 - nvidia-cusolver-cu12==11.7.3.90
 - nvidia-cusparse-cu12==12.5.8.93
 - nvidia-cusparselt-cu12==0.7.1
 - nvidia-nccl-cu12==2.27.5
 - nvidia-nvjitlink-cu12==12.8.93
 - nvidia-nvshmem-cu12==3.3.20
 - nvidia-nvtx-cu12==12.8.90
 - torch==2.9.0
 + torch==2.9.0+cpu
 - triton==3.5.0
$ uv sync --extra pypi
Resolved 31 packages in 2ms
Installed 16 packages in 185ms
 + nvidia-cublas-cu12==12.8.4.1
 + nvidia-cuda-cupti-cu12==12.8.90
 + nvidia-cuda-nvrtc-cu12==12.8.93
 + nvidia-cuda-runtime-cu12==12.8.90
 + nvidia-cudnn-cu12==9.10.2.21
 + nvidia-cufft-cu12==11.3.3.83
 + nvidia-cufile-cu12==1.13.1.3
 + nvidia-curand-cu12==10.3.9.90
 + nvidia-cusolver-cu12==11.7.3.90
 + nvidia-cusparse-cu12==12.5.8.93
 + nvidia-cusparselt-cu12==0.7.1
 + nvidia-nccl-cu12==2.27.5
 + nvidia-nvjitlink-cu12==12.8.93
 + nvidia-nvshmem-cu12==3.3.20
 + nvidia-nvtx-cu12==12.8.90
 + triton==3.5.0
$ uv sync --extra cu128
Resolved 31 packages in 2ms
Uninstalled 1 package in 2.75s
Installed 1 package in 4.33s
 - torch==2.9.0+cpu
 + torch==2.9.0+cu128
$ uv sync --extra pypi
Resolved 31 packages in 1ms
Audited 27 packages in 4ms
```

---

I presume this is because the torch indices name their wheels e.g. `2.9.0+cpu` and `2.9.0+cu128`, whereas the pypi wheel is just `2.9.0`. But this behaviour did trip us up a bit, so possibly there's some room for improvement here, even if just in the docs.

---

_Assigned to @konstin by @konstin on 2025-10-24 12:54_

---

_Comment by @konstin on 2025-10-24 12:55_

This is a bug about how we handle switching from local version to non-local version, we have to fix the invalidation.

---

_Comment by @tekeinhor on 2025-11-05 12:52_

Hello team,
I think I have a similar issue. I wanted to add my case here to support this "bug".

Let's say I have this following pyproject.toml

```toml
[project]
name = "foo"
version = "0.1.0"
description = "my description"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "my-private-pkg>=0.1.0",
]


[build-system]
requires = ["uv_build>=0.8.0,<0.9"]
build-backend = "uv_build"


[tool.uv.build-backend]
module-root = ""


[tool.uv.sources]
my-private-pkg = { path = "../private-pkg-dir", editable = true } # editable mode when in dev
#my-private-pkg = { index = "privaterepo" } # index mode when in prod

[[tool.uv.index]]
name = "privaterepo"
url = "<link_to_my_private_repo_index>"

```

Steps:
1. `pyproject.toml` use editable mode
2. `uv sync`  with editable mode, `uv.lock` updated correctly,  `.venv` updated  with local file
3. `pyproject.toml`  from editable mode to index mode (comment line with editable mode and uncomment line with index mode)
4. `uv sync` again, `uv.lock` updated, but the `.venv`  is not getting updated with the index version of `my-private-pkg`

It only works when I delete the `.venv` and re do the `uv sync`.

PS: It is a delight to use `uv`. Thanks for ur work !

---

_Comment by @konstin on 2025-12-14 21:21_

@tekeinhor Are there any local versions involved? This bug is caused by a specific edge case about local versions that only torch should really be affected by (which hacks GPU support with local versions). Your case seems to be a different on of switching between index and local package: uv sees a matching version of my-private-pkg in the venv, and given that the lockfile requires `my-private-pkg` at the same version, considers that fitting. There's some nuances around this (people intentionally using a custom install vs. you now have to pass `--reinstall-package` to `uv sync`), but they'd fit better in their own issue. You may also want to try using conflict-dependencies to support local editable and index package through extras.

---
