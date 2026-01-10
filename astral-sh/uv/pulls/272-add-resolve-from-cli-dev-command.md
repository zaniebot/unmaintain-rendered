```yaml
number: 272
title: Add resolve from cli dev command
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: add-resolve-from-cli-puffin-dev-command
created_at: 2023-11-01T13:41:05Z
updated_at: 2023-11-01T15:46:48Z
url: https://github.com/astral-sh/uv/pull/272
synced_at: 2026-01-10T15:50:28Z
```

# Add resolve from cli dev command

---

_Pull request opened by @konstin on 2023-11-01 13:41_

I don't want to create a new file for every requirement i test, e.g.

```console
$ cargo run --bin puffin-dev -q -- resolve-cli torch
filelock ==3.13.1
fsspec ==2023.10.0
jinja2 ==3.1.2
markupsafe ==2.1.3
mpmath ==1.3.0
networkx ==3.2.1
nvidia-cublas-cu12 ==12.1.3.1
nvidia-cuda-cupti-cu12 ==12.1.105
nvidia-cuda-nvrtc-cu12 ==12.1.105
nvidia-cuda-runtime-cu12 ==12.1.105
nvidia-cudnn-cu12 ==8.9.2.26
nvidia-cufft-cu12 ==11.0.2.54
nvidia-curand-cu12 ==10.3.2.106
nvidia-cusolver-cu12 ==11.4.5.107
nvidia-cusparse-cu12 ==12.1.0.106
nvidia-nccl-cu12 ==2.18.1
nvidia-nvjitlink-cu12 ==12.3.52
nvidia-nvtx-cu12 ==12.1.105
sympy ==1.12
torch ==2.1.0
triton ==2.1.0
typing-extensions ==4.8.0

```

---

_@charliermarsh approved on 2023-11-01 13:46_

---

_Comment by @charliermarsh on 2023-11-01 13:46_

Nit: can you add an example usage to the PR summary?

---

_Merged by @konstin on 2023-11-01 15:46_

---

_Closed by @konstin on 2023-11-01 15:46_

---

_Branch deleted on 2023-11-01 15:46_

---
