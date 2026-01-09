---
number: 7936
title: Docker build with workspace package
type: issue
state: closed
author: idinsmore1
labels:
  - question
assignees: []
created_at: 2024-10-04T21:26:43Z
updated_at: 2024-10-23T16:32:53Z
url: https://github.com/astral-sh/uv/issues/7936
synced_at: 2026-01-07T13:12:17-06:00
---

# Docker build with workspace package

---

_Issue opened by @idinsmore1 on 2024-10-04 21:26_

Thanks for such an amazing tool in `uv`, it has been an awesome addition in my work as an analyst in academic research. I'm trying to build out a docker image of the workspace using the docker guide in the docs, but I'm running into the following error while trying to build the image. 

```bash
docker build . 
 > [stage-0 5/7] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --frozen --no-install-project --no-install-workspace --compile-bytecode:
#10 5.139 Using CPython 3.11.10
#10 5.139 Creating virtual environment at: .venv
#10 8.494 error: Failed to prepare distributions
#10 8.494   Caused by: Failed to fetch wheel: dracopy==1.4.0
#10 8.494   Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
#10 8.494 --- stdout:
#10 8.494
#10 8.494 --- stderr:
#10 8.494 Traceback (most recent call last):
#10 8.494   File "<string>", line 14, in <module>
#10 8.494   File "/root/.cache/uv/builds-v0/.tmpVvUWy6/lib/python3.11/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
#10 8.494     return self._get_build_requires(config_settings, requirements=[])
#10 8.494            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#10 8.494   File "/root/.cache/uv/builds-v0/.tmpVvUWy6/lib/python3.11/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
#10 8.494     self.run_setup()
#10 8.494   File "/root/.cache/uv/builds-v0/.tmpVvUWy6/lib/python3.11/site-packages/setuptools/build_meta.py", line 503, in run_setup
#10 8.494     super().run_setup(setup_script=setup_script)
#10 8.494   File "/root/.cache/uv/builds-v0/.tmpVvUWy6/lib/python3.11/site-packages/setuptools/build_meta.py", line 318, in run_setup
#10 8.494     exec(code, locals())
#10 8.494   File "<string>", line 6, in <module>
#10 8.494 ModuleNotFoundError: No module named 'skbuild'
#10 8.494 ---
------
executor failed running [/bin/sh -c uv sync --frozen --no-install-project --no-install-workspace --compile-bytecode]: exit code: 2
```

My workspace looks like this:
```bash
├── Dockerfile
├── LICENSE
├── README.md
├── packages
│   ├── mircat-seg
│   │   ├── README.md
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── mircat_seg
│   │           ├── __init__.py
│   │           ├── core.py
│   │           └── models
│   │               └── __init__.py
│   └── mircat-stats
│       ├── Dockerfile
│       ├── README.md
│       ├── pyproject.toml
│       └── src
│           └── mircat_stats
│               ├── __init__.py
│               ├── configs
│               │   ├── __init__.py
│               │   ├── logging.py
│               │   ├── models.py
│               │   └── statistics.py
│               ├── dicom
│               │   ├── __init__.py
│               │   └── dicom_folder.py
│               ├── models
│               │   ├── __init__.py
│               │   └── xgboost.json
│               └── statistics
│                   ├── __init__.py
│                   ├── aorta.py
│                   ├── body_and_tissues.py
│                   ├── centerline.py
│                   ├── contrast_detection.py
│                   ├── cpr.py
│                   ├── nifti.py
│                   ├── total.py
│                   └── utils.py
├── pyproject.toml
├── src
│   └── mircat
│       └── __init__.py
└── uv.lock
```
My root `pyproject.toml`:
```toml
[project]
name = "mircat"
version = "0.1.1"
description = "write description here"
readme = "README.md"
requires-python = ">=3.10,<3.12"
dependencies = [
    "loguru>=0.7.2",
    "nibabel>=5.2.1",
    "numpy==1.26",
    "scipy==1.13.1",
    "threadpoolctl>=3.5.0",
    "mircat-seg",
    "mircat-stats",
    "scikit-build>=0.18.1",
]

[project.scripts]
mircat = "mircat:mircat"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
mircat-seg = { workspace = true }
mircat-stats = { workspace = true }

[tool.uv.workspace]
members = ["packages/mircat-seg", "packages/mircat-stats"]
```
and finally my Dockerfile
```dockerfile
FROM ubuntu:oracular

COPY --from=ghcr.io/astral-sh/uv:0.4.18 /uv /bin/uv

# Change the working directory to the `app` directory
RUN apt-get update && \
    apt-get install -y clang gcc g++ python3-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Install dependencies
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-install-workspace --compile-bytecode

# Copy the project into the image
ADD . /app

# Sync the project
RUN --mount=type=cache,target=/root/.cache/uv \
    && uv sync --frozen

ENTRYPOINT ["uv", "run", "mircat"]
```

Any ideas on a solution would be great. Thanks!


---

_Comment by @zanieb on 2024-10-04 21:45_

It looks like that version of Dracopy does not define `skbuild` as a build requirement but imports it: https://inspector.pypi.io/project/dracopy/1.4.0/packages/40/dc/fc22bdbe29fc9ecc7232f55253e7d6419be5b28ca77eb2e9f9cd9fac321c/DracoPy-1.4.0.tar.gz/DracoPy-1.4.0/setup.py#line.6

Hence, when we attempt to build that project in an isolated environment we see the error:

> ModuleNotFoundError: No module named 'skbuild'

Is there a newer version of that package that properly declares its dependencies? If not you'll need to disable build isolation and manually install its build dependencies beforehand (i.e. `uv pip install skbuild && uv sync --no-build-isolation-package dracopy`).

See more details about build isolation https://docs.astral.sh/uv/pip/compatibility/#pep-517-build-isolation (and #2252)

---

_Label `question` added by @zanieb on 2024-10-04 21:46_

---

_Comment by @idinsmore1 on 2024-10-08 15:11_

Weirdly enough, this seems to be an issue of trying to build the image on an M1 Mac vs. on a Linux machine. DracoPy doesn't properly pull it's pre-built wheel when I attempt to build the image on Mac and tries to build from source (where the error is happening), but it works just fine when I build the image on my Ubuntu machine, with the same base image used on both. Thanks so much for your help and quick response, this is good to know for the future!

---

_Closed by @idinsmore1 on 2024-10-08 15:11_

---

_Comment by @BielStela on 2024-10-23 16:32_

Sam happens to me but with library `rasterio` as a dependency, Building a docker image like yours in mac silicon fails with a similar error :
```
 > [stage-0 5/7] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --frozen --no-install-project:                                           
0.088 Using CPython 3.12.7 interpreter at: /usr/local/bin/python                                                                     
0.088 Creating virtual environment at: .venv                                                                                         
0.727 error: Failed to prepare distributions                                                                                         
0.727   Caused by: Failed to download and build `rasterio==1.3.11`
0.727   Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
0.727 
0.727 [stderr]
0.727 <string>:22: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
0.727 WARNING:root:Failed to get options via gdal-config: [Errno 2] No such file or directory: 'gdal-config'
0.727 ERROR: A GDAL API version must be specified. Provide a path to gdal-config using a GDAL_CONFIG environment variable or use a GDAL_VERSION environment variable.
```
What blow my mind is that I can build perfectly fine in x86 linux OS 🙃 

---
