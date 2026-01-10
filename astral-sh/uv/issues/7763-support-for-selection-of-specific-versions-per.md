---
number: 7763
title: Support for selection of specific versions per user / sync
type: issue
state: closed
author: relativityhd
labels: []
assignees: []
created_at: 2024-09-28T18:32:13Z
updated_at: 2024-09-28T21:28:48Z
url: https://github.com/astral-sh/uv/issues/7763
synced_at: 2026-01-10T01:24:19Z
---

# Support for selection of specific versions per user / sync

---

_Issue opened by @relativityhd on 2024-09-28 18:32_

First: I love `uv`, thank you very much for working on such a nice piece of software!

I have a question / feature request which involves installing `GDAL` (and `PyTorch`).

Currently, I have project setup with `rye` to install the Python-bindings for `GDAL`.GDAL itself is a geo-software written in C++ which is not easy to install, especially having multiple versions of it installed at the same time is nearly impossible, without having conda. Therefore, everyone in our team uses a slightly different version of GDAL, some 3.8.4, some 3.6.4 and so on. All of these are compatible with our code, therefore having different versions installed is no problem. The big problem with the Python-bindings is, that they require the exact same version of python-binding package installed as the currently installed GDAL software. Therefore, I utilized python extras configured with Rye, so that each of us can define their GDAL version when installing the python environment with

```sh
rye sync --features gdal38
```

and I put this in my `pyproject.toml`:

```toml
[project.optional-dependencies]
gdal39 = ["gdal==3.9.2"]
gdal38 = ["gdal==3.8.5"]
gdal384 = ["gdal==3.8.4"]
gdal37 = ["gdal==3.7.3"]
gdal36 = ["gdal==3.6.4"]
```

<details>

<summary>A similar thing can be applied to CUDA versions:</summary>

```toml
[project.optional-dependencies]
cpu = ["torch==2.2.0+cpu", "torchvision==0.17.0+cpu"]
cuda11 = [
    "torch==2.2.0+cu118",
    "torchvision==0.17.0+cu118",
    "cupy-cuda11x>=13.3.0",
    "cucim-cu11>=24.8.0",
]
cuda12 = [
    "torch==2.2.0+cu121",
    "torchvision==0.17.0+cu121",
    "cupy-cuda12x>=13.3.0",
    "cucim-cu12==24.8.*",
]

[[tool.rye.sources]]
name = "nvidia"
url = "https://pypi.nvidia.com"

[[tool.rye.sources]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"

[[tool.rye.sources]]
name = "pytorch-cu11"
url = "https://download.pytorch.org/whl/cu118"

[[tool.rye.sources]]
name = "pytorch-cu12"
url = "https://download.pytorch.org/whl/cu121"

```

</details>

This is probably an antipattern, since it depends on disjoint extras, something @charliermarsh mentioned to be not supported by uv (https://github.com/astral-sh/uv/pull/7745#discussion_r1779668158)

Now to my question: **Is there a better way of accomplishing this with uv (or in the future planned)?**

If not, I'd like to open a feature request to supporting this use-case. One idea would be custom markers, which could be setted by the user when syncing.


---

_Comment by @charliermarsh on 2024-09-28 19:07_

We plan to support disjoint extras in the future, hopefully in the next few weeks or months.

---

_Comment by @relativityhd on 2024-09-28 21:25_

Thank you for the quick reply! Therefore my configuration isn't an antipattern? Looking forward for the feature!

---

_Closed by @relativityhd on 2024-09-28 21:28_

---
