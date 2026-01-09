---
number: 7557
title: "`uv pip compile --universal` is not universal enough for an x86 mac"
type: issue
state: closed
author: zhou13
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-09-19T18:13:36Z
updated_at: 2024-09-19T20:35:45Z
url: https://github.com/astral-sh/uv/issues/7557
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip compile --universal` is not universal enough for an x86 mac

---

_Issue opened by @zhou13 on 2024-09-19 18:13_

It seems that `uv pip compile --universal` does not support x86 macos directly. On my intel mac,
```
echo torch > requirements.in
uv pip compile --universal requirements.in -o requirement.txt
uv pip install -r requirement.txt
```
You will get
```
× No solution found when resolving dependencies:
  ╰─▶ Because torch==2.4.1 has no wheels with a matching Python implementation tag and you require torch==2.4.1, we can
      conclude that your requirements are unsatisfiable.
```

Things work normally when you remove the `--universal` flag, in which `uv` will install `torch==2.2.2`, which still supports x86 macos.

---

_Comment by @charliermarsh on 2024-09-19 18:17_

For `--universal`, the platform shouldn't matter at all (we don't even look at it). What Python version are you using? Can you include the full logs with `--verbose`? Are you using an alternate index?

---

_Label `question` added by @charliermarsh on 2024-09-19 18:17_

---

_Comment by @zhou13 on 2024-09-19 18:19_

I am using python 3.10.
I think the problem is `torch==2.4.1` provide supports for macOS, but only for the arm architecture.

```
$uv pip compile --universal requirements.in -o requirement.txt
DEBUG uv 0.4.12
DEBUG Starting Python discovery for any Python
DEBUG Looking for exact match for request any Python
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Found `cpython-3.10.14-macos-x86_64-none` at `/Users/yichaozhou/Sources/phicore/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.14 interpreter at .venv/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.14
DEBUG Solving with target Python version: >=3.10.14
DEBUG Adding direct dependency: torch*
DEBUG Found fresh response for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (*)
DEBUG Selecting: torch==2.4.1 [preference] (torch-2.4.1-cp310-cp310-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/05/d540049b1832d1062510efc6829634b7fbef5394c757d8312414fb65a3cb/torch-2.4.1-cp310-cp310-manylinux1_x86_64.whl.metadata
DEBUG Adding transitive dependency for torch==2.4.1: filelock*
DEBUG Adding transitive dependency for torch==2.4.1: fsspec*
DEBUG Adding transitive dependency for torch==2.4.1: jinja2*
DEBUG Adding transitive dependency for torch==2.4.1: networkx*
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cublas-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.3.1
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and platform_system =='Linux'}==12.1.105
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and platform_system =='Linux'}==12.1.105
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.105
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cudnn-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==9.1.0.70
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cufft-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==11.0.2.54
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-curand-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==10.3.2.106
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cusolver-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==11.4.5.107
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cusparse-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.0.106
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-nccl-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==2.20.5
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-nvtx-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.105
DEBUG Adding transitive dependency for torch==2.4.1: sympy*
DEBUG Adding transitive dependency for torch==2.4.1: triton{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}==3.0.0
DEBUG Adding transitive dependency for torch==2.4.1: typing-extensions>=4.8.0
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
DEBUG Found fresh response for: https://pypi.org/simple/networkx/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-runtime-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cufft-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-curand-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nccl-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/fsspec/
DEBUG Searching for a compatible version of nvidia-cublas-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.3.1)
DEBUG Selecting: nvidia-cublas-cu12==12.1.3.1 [preference] (nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cublas-cu12==12.1.3.1: nvidia-cublas-cu12==12.1.3.1
DEBUG Adding transitive dependency for nvidia-cublas-cu12==12.1.3.1: nvidia-cublas-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.3.1
DEBUG Searching for a compatible version of nvidia-cublas-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.3.1)
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-cupti-cu12/
DEBUG Selecting: nvidia-cublas-cu12==12.1.3.1 [preference] (nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cusparse-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-nvrtc-cu12/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/f8/feced7779d755758a52d1f6635d990b8d98dc0a29fa568bbe0625f18fdf3/filelock-3.16.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/38/e9/5f72929373e1a0e8d142a130f3f97e6ff920070f87f91c4e13e40e0fba5a/networkx-3.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/triton/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cudnn-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://pypi.org/simple/sympy/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cusolver-cu12/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nvtx-cu12/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1d/a0/6aaea0c2fbea2f89bfd5db25fb1e3481896a423002ebe4e55288907a97a3/fsspec-2024.9.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/37/6d/121efd7382d5b0284239f4ab1fc1590d86d34ed4a4a2fdb13b30ca8e5740/nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cublas-cu12 (==12.1.3.1)
DEBUG Selecting: nvidia-cublas-cu12==12.1.3.1 [preference] (nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}(==12.1.105)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.1.105 [preference] (nvidia_cuda_cupti_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/99/ff/c87e0622b1dadea79d2fb0b25ade9ed98954c9033722eb707053d310d4f3/sympy-1.13.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for nvidia-cuda-cupti-cu12==12.1.105: nvidia-cuda-cupti-cu12==12.1.105
DEBUG Adding transitive dependency for nvidia-cuda-cupti-cu12==12.1.105: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.105
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}(==12.1.105)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.1.105 [preference] (nvidia_cuda_cupti_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7e/00/6b218edd739ecfc60524e585ba8e6b00554dd908de2c9c66c1af3e44e18d/nvidia_cuda_cupti_cu12-12.1.105-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12 (==12.1.105)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.1.105 [preference] (nvidia_cuda_cupti_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}(==12.1.105)
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.1.105 [preference] (nvidia_cuda_nvrtc_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cuda-nvrtc-cu12==12.1.105: nvidia-cuda-nvrtc-cu12==12.1.105
DEBUG Adding transitive dependency for nvidia-cuda-nvrtc-cu12==12.1.105: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.105
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}(==12.1.105)
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.1.105 [preference] (nvidia_cuda_nvrtc_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b6/9f/c64c03f49d6fbc56196664d05dba14e3a561038a81a638eeb47f4d4cfd48/nvidia_cuda_nvrtc_cu12-12.1.105-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12 (==12.1.105)
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.1.105 [preference] (nvidia_cuda_nvrtc_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.105)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.1.105 [preference] (nvidia_cuda_runtime_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cuda-runtime-cu12==12.1.105: nvidia-cuda-runtime-cu12==12.1.105
DEBUG Adding transitive dependency for nvidia-cuda-runtime-cu12==12.1.105: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.105
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.105)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.1.105 [preference] (nvidia_cuda_runtime_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/eb/d5/c68b1d2cdfcc59e72e8a5949a37ddb22ae6cade80cd4a57a84d4c8b55472/nvidia_cuda_runtime_cu12-12.1.105-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12 (==12.1.105)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.1.105 [preference] (nvidia_cuda_runtime_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==9.1.0.70)
DEBUG Selecting: nvidia-cudnn-cu12==9.1.0.70 [preference] (nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cudnn-cu12==9.1.0.70
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cudnn-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==9.1.0.70
DEBUG Searching for a compatible version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==9.1.0.70)
DEBUG Selecting: nvidia-cudnn-cu12==9.1.0.70 [preference] (nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9f/fd/713452cd72343f682b1c7b9321e23829f00b842ceaedcda96e742ea0b0b3/nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cublas-cu12*
DEBUG Searching for a compatible version of nvidia-cudnn-cu12 (==9.1.0.70)
DEBUG Selecting: nvidia-cudnn-cu12==9.1.0.70 [preference] (nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cublas-cu12*
DEBUG Searching for a compatible version of nvidia-cufft-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==11.0.2.54)
DEBUG Selecting: nvidia-cufft-cu12==11.0.2.54 [preference] (nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.0.2.54: nvidia-cufft-cu12==11.0.2.54
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.0.2.54: nvidia-cufft-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==11.0.2.54
DEBUG Searching for a compatible version of nvidia-cufft-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==11.0.2.54)
DEBUG Selecting: nvidia-cufft-cu12==11.0.2.54 [preference] (nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/86/94/eb540db023ce1d162e7bea9f8f5aa781d57c65aed513c33ee9a5123ead4d/nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cufft-cu12 (==11.0.2.54)
DEBUG Selecting: nvidia-cufft-cu12==11.0.2.54 [preference] (nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-curand-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==10.3.2.106)
DEBUG Selecting: nvidia-curand-cu12==10.3.2.106 [preference] (nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-curand-cu12==10.3.2.106: nvidia-curand-cu12==10.3.2.106
DEBUG Adding transitive dependency for nvidia-curand-cu12==10.3.2.106: nvidia-curand-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==10.3.2.106
DEBUG Searching for a compatible version of nvidia-curand-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==10.3.2.106)
DEBUG Selecting: nvidia-curand-cu12==10.3.2.106 [preference] (nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/44/31/4890b1c9abc496303412947fc7dcea3d14861720642b49e8ceed89636705/nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-curand-cu12 (==10.3.2.106)
DEBUG Selecting: nvidia-curand-cu12==10.3.2.106 [preference] (nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cusolver-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==11.4.5.107)
DEBUG Selecting: nvidia-cusolver-cu12==11.4.5.107 [preference] (nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusolver-cu12==11.4.5.107
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusolver-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==11.4.5.107
DEBUG Searching for a compatible version of nvidia-cusolver-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==11.4.5.107)
DEBUG Selecting: nvidia-cusolver-cu12==11.4.5.107 [preference] (nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bc/1d/8de1e5c67099015c834315e333911273a8c6aaba78923dd1d1e25fc5f217/nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cublas-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusparse-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusolver-cu12 (==11.4.5.107)
DEBUG Selecting: nvidia-cusolver-cu12==11.4.5.107 [preference] (nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cublas-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusparse-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusparse-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.0.106)
DEBUG Selecting: nvidia-cusparse-cu12==12.1.0.106 [preference] (nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-cusparse-cu12==12.1.0.106
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-cusparse-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.0.106
DEBUG Searching for a compatible version of nvidia-cusparse-cu12 (==12.1.0.106)
DEBUG Selecting: nvidia-cusparse-cu12==12.1.0.106 [preference] (nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/65/5b/cfaeebf25cd9fdec14338ccb16f6b2c4c7fa9163aefcf057d86b9cc248bb/nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusparse-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.0.106)
DEBUG Selecting: nvidia-cusparse-cu12==12.1.0.106 [preference] (nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-nccl-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==2.20.5)
DEBUG Selecting: nvidia-nccl-cu12==2.20.5 [preference] (nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-nccl-cu12==2.20.5: nvidia-nccl-cu12==2.20.5
DEBUG Adding transitive dependency for nvidia-nccl-cu12==2.20.5: nvidia-nccl-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==2.20.5
DEBUG Searching for a compatible version of nvidia-nccl-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==2.20.5)
DEBUG Selecting: nvidia-nccl-cu12==2.20.5 [preference] (nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_aarch64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nvjitlink-cu12/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/bb/d09dda47c881f9ff504afd6f9ca4f502ded6d8fc2f572cacc5e39da91c28/nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-nccl-cu12 (==2.20.5)
DEBUG Selecting: nvidia-nccl-cu12==2.20.5 [preference] (nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-nvtx-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.105)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/8c/69c9e39cd6bfa813852a94e9bd3c075045e2707d163e9dc2326c82d2c330/nvidia_nvjitlink_cu12-12.6.68-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Selecting: nvidia-nvtx-cu12==12.1.105 [preference] (nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-nvtx-cu12==12.1.105: nvidia-nvtx-cu12==12.1.105
DEBUG Adding transitive dependency for nvidia-nvtx-cu12==12.1.105: nvidia-nvtx-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==12.1.105
DEBUG Searching for a compatible version of nvidia-nvtx-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'} (==12.1.105)
DEBUG Selecting: nvidia-nvtx-cu12==12.1.105 [preference] (nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/d3/8057f0587683ed2fcd4dbfbdfdfa807b9160b809976099d36b8f60d08f03/nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-nvtx-cu12 (==12.1.105)
DEBUG Selecting: nvidia-nvtx-cu12==12.1.105 [preference] (nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Searching for a compatible version of triton{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'} (==3.0.0)
DEBUG Selecting: triton==3.0.0 [preference] (triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl)
DEBUG Adding transitive dependency for triton==3.0.0: triton==3.0.0
DEBUG Adding transitive dependency for triton==3.0.0: triton{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}==3.0.0
DEBUG Searching for a compatible version of triton{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'} (==3.0.0)
DEBUG Selecting: triton==3.0.0 [preference] (triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/45/27/14cc3101409b9b4b9241d2ba7deaa93535a217a211c86c4cc7151fb12181/triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata
DEBUG Adding transitive dependency for triton==3.0.0: filelock*
DEBUG Searching for a compatible version of triton (==3.0.0)
DEBUG Selecting: triton==3.0.0 [preference] (triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl)
DEBUG Adding transitive dependency for triton==3.0.0: filelock*
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.16.1 [preference] (filelock-3.16.1-py3-none-any.whl)
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Selecting: fsspec==2024.9.0 [preference] (fsspec-2024.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.4 [preference] (jinja2-3.1.4-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.4: markupsafe>=2.0
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.3 [preference] (networkx-3.3-py3-none-any.whl)
DEBUG Searching for a compatible version of sympy (*)
DEBUG Selecting: sympy==1.13.3 [preference] (sympy-1.13.3-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.3: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of typing-extensions (>=4.8.0)
DEBUG Selecting: typing-extensions==4.12.2 [preference] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of nvidia-nvjitlink-cu12 (*)
DEBUG Selecting: nvidia-nvjitlink-cu12==12.6.68 [preference] (nvidia_nvjitlink_cu12-12.6.68-py3-none-manylinux2014_aarch64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/mpmath/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/markupsafe/
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==2.1.5 [preference] (MarkupSafe-2.1.5-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e4/54/ad5eb37bf9d51800010a74e4665425831a9db4e7c4e0fde4352e391e808e/MarkupSafe-2.1.5-cp310-cp310-macosx_10_9_universal2.whl.metadata
DEBUG Searching for a compatible version of mpmath (>=1.1.0, <1.4)
DEBUG Selecting: mpmath==1.3.0 [preference] (mpmath-1.3.0-py3-none-any.whl)
DEBUG Tried 22 versions: filelock 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, networkx 1, nvidia-cublas-cu12 1, nvidia-cuda-cupti-cu12 1, nvidia-cuda-nvrtc-cu12 1, nvidia-cuda-runtime-cu12 1, nvidia-cudnn-cu12 1, nvidia-cufft-cu12 1, nvidia-curand-cu121, nvidia-cusolver-cu12 1, nvidia-cusparse-cu12 1, nvidia-nccl-cu12 1, nvidia-nvjitlink-cu12 1, nvidia-nvtx-cu12 1, sympy 1, torch 1, triton 1, typing-extensions 1
DEBUG Split universal resolution took 0.009s
# This file was autogenerated by uv via the following command:
#    uv pip compile --universal requirements.in -o requirement.txt
filelock==3.16.1
    # via
    #   torch
    #   triton
fsspec==2024.9.0
    # via torch
jinja2==3.1.4
    # via torch
markupsafe==2.1.5
    # via jinja2
mpmath==1.3.0
    # via sympy
networkx==3.3
    # via torch
nvidia-cublas-cu12==12.1.3.1 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via
    #   nvidia-cudnn-cu12
    #   nvidia-cusolver-cu12
    #   torch
nvidia-cuda-cupti-cu12==12.1.105 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-cuda-nvrtc-cu12==12.1.105 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-cuda-runtime-cu12==12.1.105 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-cudnn-cu12==9.1.0.70 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-cufft-cu12==11.0.2.54 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-curand-cu12==10.3.2.106 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-cusolver-cu12==11.4.5.107 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-cusparse-cu12==12.1.0.106 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via
    #   nvidia-cusolver-cu12
    #   torch
nvidia-nccl-cu12==2.20.5 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
nvidia-nvjitlink-cu12==12.6.68 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via
    #   nvidia-cusolver-cu12
    #   nvidia-cusparse-cu12
nvidia-nvtx-cu12==12.1.105 ; platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
sympy==1.13.3
    # via torch
torch==2.4.1
    # via -r requirements.in
triton==3.0.0 ; python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'
    # via torch
typing-extensions==4.12.2
    # via torch
```

---

_Comment by @charliermarsh on 2024-09-19 18:21_

Oh sorry, you're saying the subsequent _install_ doesn't work. Yes that's correct and it's the same as #5182. Feel free to track that issue -- it's an extremely hard problem to solve.

---

_Closed by @charliermarsh on 2024-09-19 18:21_

---

_Comment by @zhou13 on 2024-09-19 18:27_

@charliermarsh After reading #5182, I think this is a different issue if I understand correctly. #5182 is about resolving between `2.3.1+cpu` vs `2.3.1` (without the local version), while this one is about requiring support for both arm and intel macOS in the same universal resolve.

In this ticket, we don't have the issue introduced by the local version.

Edit: It seems that `torch-2.4.1-cp310-cp310-macosx_10_15_x86_64.whl` exists on pypi. Not sure why the installation failed.
Edit2: It is for 10.15.

---

_Comment by @zhou13 on 2024-09-19 18:47_

Yes, I think the problem is in `uv pip install -r ` rather than the universal resolver.

I have no problem installing pytorch 2.4.1 with pip:
```
$ pip install torch==2.4.1
Looking in indexes: https://pypi.python.org/simple, https://pypi.apple.com/simple
Collecting torch==2.4.1
  Downloading https://pypi.apple.com/torch/2.4.1/torch-2.4.1-cp310-cp310-macosx_10_15_x86_64.whl (156.7 MB)
```
But I cannot install it with uv (under the same environment)
```
$ uv pip install torch==2.4.1
× No solution found when resolving dependencies:
  ╰─▶ Because torch==2.4.1 has no wheels with a matching Python implementation tag and you require torch==2.4.1, we can
      conclude that your requirements are unsatisfiable.
```


---

_Comment by @charliermarsh on 2024-09-19 18:47_

How are you providing the Apple index?

---

_Comment by @charliermarsh on 2024-09-19 18:47_

Note that `torch-2.4.1-cp310-cp310-macosx_10_15_x86_64.whl` is not coming from PyPI in your first example.

---

_Comment by @zhou13 on 2024-09-19 19:09_

Thank you for the discussion. After doing more debugging, I think you were right: the problem I have is a duplication of https://github.com/astral-sh/uv/issues/5182 due to the problem between `torch==2.4.1+default` and `torch==2.4.1`.

---

_Label `duplicate` added by @zanieb on 2024-09-19 20:35_

---

_Referenced in [astral-sh/uv#9711](../../astral-sh/uv/issues/9711.md) on 2024-12-07 20:47_

---
