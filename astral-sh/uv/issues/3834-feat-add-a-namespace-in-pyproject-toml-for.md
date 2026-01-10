---
number: 3834
title: "feat: add a namespace in pyproject.toml for dependency override"
type: issue
state: closed
author: Di-Is
labels:
  - configuration
assignees: []
created_at: 2024-05-25T08:37:30Z
updated_at: 2024-06-03T23:27:43Z
url: https://github.com/astral-sh/uv/issues/3834
synced_at: 2026-01-10T01:23:31Z
---

# feat: add a namespace in pyproject.toml for dependency override

---

_Issue opened by @Di-Is on 2024-05-25 08:37_

Are there any plans to add a namespace in pyproject.toml that can be used with the dependency override feature?

For example, if we could add "tool.uv.override-dependencies" as follows and define the dependencies to be overridden, it would further improve the usability of uv.

```toml
[project]
name = "ml-frameworks"
version = "1.0.0"
dependencies = [
  "torch ==2.2.2",
  "torchvision >=0.17.0",
  "tensorflow[and-cuda] ==2.16.1",
  "jax[cuda12]==0.4.23"
]

[tool.uv]
override-dependencies = [
  "nvidia-cublas-cu12 == 12.3.4.1",
  "nvidia-cuda-cupti-cu12 == 12.3.101",
  "nvidia-cuda-nvcc-cu12 == 12.3.107",
  "nvidia-cuda-nvrtc-cu12 == 12.3.107",
  "nvidia-cuda-runtime-cu12 == 12.3.101",
  "nvidia-cudnn-cu12 == 8.9.7.29",
  "nvidia-cufft-cu12 == 11.0.12.1",
  "nvidia-curand-cu12 == 10.3.4.107",
  "nvidia-cusolver-cu12 == 11.5.4.101",
  "nvidia-cusparse-cu12 == 12.2.0.103",
  "nvidia-nccl-cu12 == 2.19.3",
  "nvidia-nvjitlink-cu12 == 12.3.101"
]

[tool.uv.sources]
# Pin a dependency for a specific registry.
torch = { index = "torch-cu121" }

# See "Indexes".
[tool.uv.indexes]
torch-cu121 = "https://download.pytorch.org/whl/cu121"
```

---

_Label `configuration` added by @zanieb on 2024-05-25 13:37_

---

_Comment by @zanieb on 2024-05-25 13:37_

I think so yes! cc @konstin 

---

_Renamed from "feat: add a hierarchy in pyproject.toml for dependency override" to "feat: add a namespace in pyproject.toml for dependency override" by @Di-Is on 2024-05-26 13:20_

---

_Referenced in [astral-sh/uv#3839](../../astral-sh/uv/pulls/3839.md) on 2024-05-26 13:42_

---

_Comment by @konstin on 2024-05-27 09:56_

This would be great to have!

---

_Comment by @Di-Is on 2024-06-03 23:27_

Close this issue because the PR has been incorporated into the main branch.

---

_Closed by @Di-Is on 2024-06-03 23:27_

---
