---
number: 13412
title: Filter source dependencies by pattern
type: issue
state: open
author: jzazo
labels:
  - enhancement
assignees: []
created_at: 2025-05-12T15:52:29Z
updated_at: 2025-09-08T15:03:02Z
url: https://github.com/astral-sh/uv/issues/13412
synced_at: 2026-01-07T13:12:18-06:00
---

# Filter source dependencies by pattern

---

_Issue opened by @jzazo on 2025-05-12 15:52_

### Summary

I would like to be able to specify source dependencies by pattern. For example, when I install `torch` from index-url like cuda, a lot of `nvidia-*` dependencies if not explicit are installed from pypi if they exist, or not installed at all if they not exist in pypi.



### Example

At the moment I need to do the following to force the torch dependencies to be installed along pytorch using the index-url (because they may not exist in pypi for the given cuda driver).

```
[project]
name = "example-project-uv"
version = "0.0.12"
description = ""
requires-python = "==3.12.*"
readme = "README.md"
dependencies = []

[project.optional-dependencies]
cuda = [
    "torch==2.6.0",
    "nvidia-cublas-cu12",
    "nvidia-cuda-cupti-cu12",
    "nvidia-cuda-nvrtc-cu12",
    "nvidia-cuda-runtime-cu12",
    "nvidia-cudnn-cu12",
    "nvidia-cufft-cu12",
    "nvidia-cufile-cu12",
    "nvidia-curand-cu12",
    "nvidia-cusolver-cu12",
    "nvidia-cusparse-cu12",
    "nvidia-nvjitlink-cu12",
    "nvidia-nvtx-cu12",
    "triton",
]

[tool.uv.sources]
torch = [{ index = "torchcuda", extra = "cuda" }]
nvidia-cublas-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cuda-cupti-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cuda-nvrtc-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cuda-runtime-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cudnn-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cufft-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cufile-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-curand-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cusolver-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-cusparse-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-nvjitlink-cu12 = { index = "torchcuda", extra = "cuda" }
nvidia-nvtx-cu12 = { index = "torchcuda", extra = "cuda" }
triton = { index = "torchcuda", extra = "cuda" }

[[tool.uv.index]]
name = "torchcuda"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

This could be done as follows:
```
[project]
name = "example-project-uv"
version = "0.0.12"
description = ""
requires-python = "==3.12.*"
readme = "README.md"
dependencies = []

[project.optional-dependencies]
cuda = ["torch==2.6.0"]

[tool.uv.sources]
torch = [{ index = "torchcuda", extra = "cuda" }]
triton = { index = "torchcuda", extra = "cuda" }
"nvidia-*" = { index = "torchcuda", extra = "cuda" }

[[tool.uv.index]]
name = "torchcuda"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

---

_Label `enhancement` added by @jzazo on 2025-05-12 15:52_

---

_Renamed from "Filter dependencies by pattern" to "Filter source dependencies by pattern" by @jzazo on 2025-05-12 16:01_

---

_Comment by @jzazo on 2025-06-24 10:41_

@charliermarsh apologies for pinging, but given that uv has recently released support for pytorch backend, I think this feature is quite related, and I just wanted to draw attention to get an opinion/advise. 

When we install torch with cuda support from explicit index, are extra dependency packages like the nvidia ones pulled from the given index-url? I think there is an issue when such packages are not available in pypi, for example if the index was for cu126.

I solved it as above, by writing those packages explicitly. I guess if you specify cu128, this is not a problem because the nvidia packages are published in pypi.

Let me know if I am missing anything. Thanks.

---

_Comment by @konstin on 2025-09-05 12:39_

Does it work if you follow the torch integration guide (https://docs.astral.sh/uv/guides/integration/pytorch/)? You shouldn't need to declare something for the `nvidia-*` package at all.

---

_Comment by @jzazo on 2025-09-08 15:03_

It does work following the integration guide, but sometimes if you set up an index-url with a specific cuda version, the nvidia packages are not taken from the index-url but from pypi. If pypi does not have compatible packages for that pytorch and cuda version, it won't install them. For example, pypi does not have compatible nvidia-* versions for pytorch 2.70 and index-url "[https://download.pytorch.org/whl/cu126".](https://download.pytorch.org/whl/cu126%22.) It will work if you choose "https://download.pytorch.org/whl/cu128". 

---
