---
number: 15830
title: Cuda 13.0 and its python supporting packages
type: issue
state: closed
author: whitesscott
labels:
  - enhancement
assignees: []
created_at: 2025-09-14T01:47:43Z
updated_at: 2025-11-03T02:51:07Z
url: https://github.com/astral-sh/uv/issues/15830
synced_at: 2026-01-07T13:12:19-06:00
---

# Cuda 13.0 and its python supporting packages

---

_Issue opened by @whitesscott on 2025-09-14 01:47_

### Summary

Nvidia has released Jetson AGX Thor developer kit. It and other Nvidia hardware in the Blackwell line use CUDA 13.0.48. I forked this repo with the intention of attempting to implement Cuda 13 and quickly realized there were many parts that may need updating and the way I think the json and other related data  don't seem to be updatable without breaking the Cuda 12 bits.


```
uv.schema.json
          "description": "Use the PyTorch index for CUDA 13.0.",
          "const": "cu130"
```
And following list of Nvidia Cuda13 python packages may not be complete 

```
cuda-bindings==13.0.1
cuda-cccl==0.0.1.dev0
cuda-core==0.3.2
cuda-pathfinder==1.2.2
cuda-python==13.0.1
cupy-cuda13x==13.6.0
nvidia-cublas==13.0.2.14
nvidia-cuda-crt==13.0.88
nvidia-cuda-cupti==13.0.85
nvidia-cuda-nvcc==13.0.88
nvidia-cuda-nvrtc==13.0.88
nvidia-cuda-nvrtc==13.0.88
nvidia-cuda-runtime==13.0.88
nvidia-cuda-runtime==13.0.88
nvidia-cudnn-cu13==9.13.0.50
nvidia-cufile==1.15.1.6
nvidia-cusparselt-cu13==0.8.1
nvidia-nccl-cu13==2.28.3
nvidia-nccl-cu13==2.28.3
nvidia-nvimgcodec-tegra-cu13==0.6.0.32
nvidia-nvjitlink==13.0.88
nvidia-nvjpeg2k-tegra-cu13==0.9.0.43
nvidia-nvtx==13.0.85
nvidia-nvvm==13.0.88
nvmath-python @ file:///home/scott/.git/nvmath-python
nvtx==0.2.13
```

pytorch/pytorch Release 2.9 compiles with Cuda 13 and creates nightly pytorch2.9_cu13 wheels; but is not yet officially released.

### Example

When attempting to build vllm-project/vllm with Cuda 13 on a Jetson AGX Thor dev kit from within a uv venv, compliation fails with it not accepting Cuda 13 as valid. My presumption is that is because of UVs not including Cuda 13.

---

_Label `enhancement` added by @whitesscott on 2025-09-14 01:47_

---

_Comment by @konstin on 2025-09-14 11:06_

Can you share the commands you ran and the error message you got?

---

_Comment by @whitesscott on 2025-09-15 00:54_

My apologies; this a problem with vllm and _not_ with UV.  After hammering on it for a while more today
 it is clear that the issue only arises when I use VLLM_USE_PRECOMPILED=0 uv pip install . **_--no-build-isolation_**  -v  

---

_Closed by @charliermarsh on 2025-11-03 02:51_

---
