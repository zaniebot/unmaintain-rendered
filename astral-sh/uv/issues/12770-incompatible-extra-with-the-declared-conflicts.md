```yaml
number: 12770
title: Incompatible extra with the declared conflicts error when remove a package
type: issue
state: closed
author: BaoNguyen6742
labels:
  - bug
assignees: []
created_at: 2025-04-09T03:23:53Z
updated_at: 2025-05-18T21:28:51Z
url: https://github.com/astral-sh/uv/issues/12770
synced_at: 2026-01-12T16:01:12Z
```

# Incompatible extra with the declared conflicts error when remove a package

---

_@BaoNguyen6742_

### Summary

When removing a package after installing Pytorch using the official tutorial gives an ``error: Extras `cpu` and `cu124` are incompatible with the declared conflicts: {`test[cpu]`, `test[cu124]`} ``

- Step to reproduce

1. `uv init test -p 3.10`

2. Use this config for pyproject.toml
```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []


[project.optional-dependencies]
cpu = ["torch==2.6.0", "torchvision==0.21.0", "torchaudio==2.6.0"]
cu124 = ["torch==2.6.0", "torchvision==0.21.0", "torchaudio==2.6.0"]


[tool.uv]
conflicts = [[{ extra = "cpu" }, { extra = "cu124" }]]


[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", extra = "cpu" },
    { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
    { index = "pytorch-cpu", extra = "cpu" },
    { index = "pytorch-cu124", extra = "cu124" },
]
torchaudio = [
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

3. `uv sync --extra cu124`

4. `uv add tensorboard`

5. `uv remove tensorboard`

This return the error ``error: Extras `cpu` and `cu124` are incompatible with the declared conflicts: {`test[cpu]`, `test[cu124]`}``

Even though the package tensorboard is remove from the pyproject.toml file, it's not actually remove and can still be seen with `uv pip list`

```
Package                  Version
------------------------ ------------
absl-py                  2.2.2
filelock                 3.18.0
fsspec                   2025.3.2
grpcio                   1.71.0
jinja2                   3.1.6
markdown                 3.7
markupsafe               3.0.2
mpmath                   1.3.0
networkx                 3.4.2
numpy                    2.2.4
nvidia-cublas-cu12       12.4.5.8
nvidia-cuda-cupti-cu12   12.4.127
nvidia-cuda-nvrtc-cu12   12.4.127
nvidia-cuda-runtime-cu12 12.4.127
nvidia-cudnn-cu12        9.1.0.70
nvidia-cufft-cu12        11.2.1.3
nvidia-curand-cu12       10.3.5.147
nvidia-cusolver-cu12     11.6.1.9
nvidia-cusparse-cu12     12.3.1.170
nvidia-cusparselt-cu12   0.6.2
nvidia-nccl-cu12         2.21.5
nvidia-nvjitlink-cu12    12.4.127
nvidia-nvtx-cu12         12.4.127
packaging                24.2
pillow                   11.1.0
protobuf                 6.30.2
setuptools               78.1.0
six                      1.17.0
sympy                    1.13.1
tensorboard              2.19.0
tensorboard-data-server  0.7.2
torch                    2.6.0+cu124
torchaudio               2.6.0+cu124
torchvision              0.21.0+cu124
triton                   3.2.0
typing-extensions        4.13.1
werkzeug                 3.1.3
```
The package is also still in the venv folder as when you run `which tensorboard` it return "test/.venv/bin/tensorboard"

The only way to actually remove it is to run `uv sync` since it will sync the environment with the pyproject.toml and since the package is remove in there, running `uv sync --extra cu124` will actually remove it now
```
Resolved 35 packages in 0.51ms
Uninstalled 10 packages in 10ms
 - absl-py==2.2.2
 - grpcio==1.71.0
 - markdown==3.7
 - packaging==24.2
 - protobuf==6.30.2
 - setuptools==78.1.0
 - six==1.17.0
 - tensorboard==2.19.0
 - tensorboard-data-server==0.7.2
 - werkzeug==3.1.3
```
Is there any way to prevent this error as there should obviously no error when you remove this package, and I don't have to use `uv sync` to actually remove the package 

### Platform

Linux 6.8.0-57-generic x86_64 GNU/Linux

### Version

uv 0.7.2

### Python version

Python 3.10.15

---

_Label `bug` added by @BaoNguyen6742 on 2025-04-09 03:23_

---

_Comment by @sglbl on 2025-04-09 08:38_

![Image](https://github.com/user-attachments/assets/d974a27a-1fb4-4829-8078-02f98a4cbd37)
I also had the same issue. Plus, now trying to add with extra changes the pyproject toml dependency to` "torch[cpu]>=2.5.1". `Before it wasn't like this and it was only in `[project.optional-dependencies]` (@charliermarsh)


---

_Comment by @BaoNguyen6742 on 2025-04-09 10:09_

> ![Image](https://github.com/user-attachments/assets/d974a27a-1fb4-4829-8078-02f98a4cbd37) I also had the same issue. Plus, now trying to add with extra changes the pyproject toml dependency to`"torch[cpu]>=2.5.1".`Before it wasn't like this and it was only in `[project.optional-dependencies]` ([@charliermarsh](https://github.com/charliermarsh))

Can you do a `uv pip list` for me after install torch using extra like that, because in my case it's not downloading the torch version from the Pytorch index but from Pypi which is the version that doesn't run with Cuda

---

_Renamed from "Incompatible extra with the declared conflict error when remove a package" to "Incompatible extra with the declared conflicts error when remove a package" by @BaoNguyen6742 on 2025-05-03 13:19_

---

_Comment by @rbavery on 2025-05-10 18:28_

I'm also getting this issue, can't remove packages but I can add them using `--extra cpu` or `--extra cu124`. My toml is below

```
[project]
name = "wherobots-executors-demo"
version = "0.1.0"
description = "Example repos for using executors for raster pipelines."
readme = "README.md"
authors = [ { name = "Len Strnad", email = "len@wherobots.com" } ]
requires-python = ">=3.11"
classifiers = [
  "Programming Language :: Python :: 3 :: Only",
  "Programming Language :: Python :: 3.11",
  "Programming Language :: Python :: 3.12",
  "Programming Language :: Python :: 3.13",
]

dependencies = [
  "bokeh>=3.1",
  "boto3>=1.34.131",
  "branca>=0.8.1",
  "dask",
  "distributed",
  "folium>=0.19.5",
  "geopandas>=1.0.1",
  "huggingface-hub>=0.28.1",
  "ipykernel>=6.29.5",
  "ipywidgets",
  "kornia>=0.7.2,<1",
  "mapclassify>=2.8.1",
  "matplotlib>=3.10.1",
  "more-itertools>=10.7",
  "numpy",
  "odc-algo>=0.2.3",
  "odc-loader>=0.5.1",
  "odc-stac<=0.4",
  "pandas",
  "pillow",
  "pre-commit",
  "pyarrow>=19.0.1",
  "pystac>=1.9,<2",
  "pystac-client>=0.7.5,<1",
  "ray[default]",
  "rustac[arrow]>=0.7",
  "s3fs>=2025.3",
  "sam2>=1.1",
  "scikit-image>=0.22,<1",
  "stac-geoparquet>=0.6",
  "stac-model==0.2",
  "tenacity>=9.1.2",
  "tqdm>=4.67.1,<5",
  "transformers>=4.48.2",
  "wandb>=0.17.3,<1",
  "xarray>=2025.1.2",
  "zarr",
]

optional-dependencies.cpu = [
  "torch>=2.6",
  "torchvision>=0.21",
]
optional-dependencies.cu124 = [
  "pytorch-triton",
  "torch>=2.6",
  "torchvision>=0.21",
]

[tool.uv]
conflicts = [ [ { extra = "cpu" }, { extra = "cu124" } ] ]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
pytorch-triton = [
  { index = "pytorch-cu124", extra = "cu124" },
]

```

---

_Comment by @charliermarsh on 2025-05-10 18:36_

It looks like we sync with `--all-extras` on remove, which seems like a mistake.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-10 18:36_

---

_Closed by @charliermarsh on 2025-05-18 21:28_

---
