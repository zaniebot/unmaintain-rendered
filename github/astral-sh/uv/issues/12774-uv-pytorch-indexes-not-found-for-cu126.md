---
number: 12774
title: UV PyTorch indexes not found for cu126
type: issue
state: open
author: bakas-george
labels:
  - needs-mre
assignees: []
created_at: 2025-04-09T08:29:25Z
updated_at: 2025-07-03T16:04:03Z
url: https://github.com/astral-sh/uv/issues/12774
synced_at: 2026-01-07T13:12:18-06:00
---

# UV PyTorch indexes not found for cu126

---

_Issue opened by @bakas-george on 2025-04-09 08:29_

### Summary

Hello, 

By default, torch installation via `uv` using CUDA 12.6 fails due to missing support in the [default](https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index) uv PyTorch indexes:

* The uv PyTorch index does not support CUDA 12.6 yet whereas CUDA 12.4 is officially supported on uv.

The issue is that you can manually override the index url on the command line:
```
uv pip install torch --index-url https://download.pytorch.org/whl/cu126
```
but when specified in the `pyproject.toml` this is not overridden. 

### Platform

Linux Ubuntu 24.04 LTS

### Version

uv 0.6.13

### Python version

3.13.3

---

_Label `bug` added by @bakas-george on 2025-04-09 08:29_

---

_Comment by @charliermarsh on 2025-04-09 12:28_

Sorry, what do you mean by “by default”? How would uv know to look on the PyTorch index if it wasn’t specified?

---

_Comment by @bakas-george on 2025-04-09 12:55_

It was specified in the `pyproject.toml` like this:

```
[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

---

_Comment by @charliermarsh on 2025-04-09 14:40_

Okay, but what error are you experiencing when you do that? uv doesn't hard-code any distinction between those indexes. All PyTorch indexes should work equally well.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-10 14:02_

---

_Label `bug` removed by @charliermarsh on 2025-04-10 14:02_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-10 14:02_

---

_Comment by @rulller on 2025-04-14 07:54_

When running `uv sync` it does not install some additional libraries needed for `pytorch` with `cuda`.

On the other hand, when running `uv pip install torch --index https://download.pytorch.org/whl/cu126` the libraries are installed.

These libraries are:

```
 + nvidia-cublas-cu12==12.6.4.1
 + nvidia-cuda-cupti-cu12==12.6.80
 + nvidia-cuda-nvrtc-cu12==12.6.77
 + nvidia-cuda-runtime-cu12==12.6.77
 + nvidia-cudnn-cu12==9.5.1.17
 + nvidia-cufft-cu12==11.3.0.4
 + nvidia-curand-cu12==10.3.7.77
 + nvidia-cusolver-cu12==11.7.1.2
 + nvidia-cusparse-cu12==12.5.4.2
 + nvidia-cusparselt-cu12==0.6.3
 + nvidia-nccl-cu12==2.21.5
 + nvidia-nvjitlink-cu12==12.6.85
 + nvidia-nvtx-cu12==12.6.77
 + triton==3.2.0
```

---

_Comment by @konstin on 2025-04-14 08:08_

Can you share your `pyproject.toml`?

If you are using `explicit = true`, it will work different than `--index https://download.pytorch.org/whl/cu126`, does it work without `explicit = true`?

---

_Comment by @KukumavMozolo on 2025-04-15 12:31_

Thats a known bug wiith pytorch 2.6.0 and cu126, use cu124
https://github.com/python-poetry/poetry/issues/10152
https://github.com/pytorch/pytorch/issues/146679#issue-2837429944
also allready mentioned here i think: https://github.com/astral-sh/uv/issues/10693

---

_Comment by @jbohnslav on 2025-07-03 16:04_

This issue still exists for pytorch nightly + cuda 12.6 today. 

---
