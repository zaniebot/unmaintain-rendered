```yaml
number: 10604
title: Requirement unsatisfiable when installing torch 2.4.1
type: issue
state: closed
author: pharmapsychotic
labels: []
assignees: []
created_at: 2025-01-14T17:07:27Z
updated_at: 2025-01-14T17:29:30Z
url: https://github.com/astral-sh/uv/issues/10604
synced_at: 2026-01-10T04:27:58Z
```

# Requirement unsatisfiable when installing torch 2.4.1

---

_Issue opened by @pharmapsychotic on 2025-01-14 17:07_

# Steps to reproduce error
```shell
uv pip install torch==2.4.1 --extra-index-url https://pypi.nvidia.com
```

# Expected results
When running with regular `pip`,  torch 2.4.1 installs correctly.
```shell
$ pip install torch==2.4.1 --extra-index-url https://pypi.nvidia.com
Successfully installed MarkupSafe-3.0.2 filelock-3.16.1 fsspec-2024.12.0 jinja2-3.1.5 mpmath-1.3.0 networkx-3.4.2 nvidia-cublas-cu12-12.1.3.1 nvidia-cuda-cupti-cu12-12.1.105 nvidia-cuda-nvrtc-cu12-12.1.105 nvidia-cuda-runtime-cu12-12.1.105 nvidia-cudnn-cu12-9.1.0.70 nvidia-cufft-cu12-11.0.2.54 nvidia-curand-cu12-10.3.2.106 nvidia-cusolver-cu12-11.4.5.107 nvidia-cusparse-cu12-12.1.0.106 nvidia-nccl-cu12-2.20.5 nvidia-nvjitlink-cu12-12.6.85 nvidia-nvtx-cu12-12.1.105 sympy-1.13.3 torch-2.4.1 triton-3.0.0 typing-extensions-4.12.2
```

<details>
<summary>Full pip output</summary>

```shell
Looking in indexes: https://pypi.org/simple, https://pypi.ngc.nvidia.com, https://pypi.nvidia.com
Collecting torch==2.4.1
  Downloading torch-2.4.1-cp310-cp310-manylinux1_x86_64.whl.metadata (26 kB)
Collecting filelock (from torch==2.4.1)
  Downloading filelock-3.16.1-py3-none-any.whl.metadata (2.9 kB)
Collecting typing-extensions>=4.8.0 (from torch==2.4.1)
  Downloading typing_extensions-4.12.2-py3-none-any.whl.metadata (3.0 kB)
Collecting sympy (from torch==2.4.1)
  Downloading sympy-1.13.3-py3-none-any.whl.metadata (12 kB)
Collecting networkx (from torch==2.4.1)
  Downloading networkx-3.4.2-py3-none-any.whl.metadata (6.3 kB)
Collecting jinja2 (from torch==2.4.1)
  Downloading jinja2-3.1.5-py3-none-any.whl.metadata (2.6 kB)
Collecting fsspec (from torch==2.4.1)
  Downloading fsspec-2024.12.0-py3-none-any.whl.metadata (11 kB)
Collecting nvidia-cuda-nvrtc-cu12==12.1.105 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cuda-nvrtc-cu12/nvidia_cuda_nvrtc_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (23.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 23.7/23.7 MB 92.4 MB/s eta 0:00:00
Collecting nvidia-cuda-runtime-cu12==12.1.105 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cuda-runtime-cu12/nvidia_cuda_runtime_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (823 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 823.6/823.6 kB 232.4 MB/s eta 0:00:00
Collecting nvidia-cuda-cupti-cu12==12.1.105 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cuda-cupti-cu12/nvidia_cuda_cupti_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (14.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.1/14.1 MB 143.0 MB/s eta 0:00:00
Collecting nvidia-cudnn-cu12==9.1.0.70 (from torch==2.4.1)
  Downloading nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl.metadata (1.6 kB)
Collecting nvidia-cublas-cu12==12.1.3.1 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cublas-cu12/nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl (410.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 410.6/410.6 MB 141.4 MB/s eta 0:00:00
Collecting nvidia-cufft-cu12==11.0.2.54 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cufft-cu12/nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl (121.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 121.6/121.6 MB 122.7 MB/s eta 0:00:00
Collecting nvidia-curand-cu12==10.3.2.106 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-curand-cu12/nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl (56.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 56.5/56.5 MB 143.8 MB/s eta 0:00:00
Collecting nvidia-cusolver-cu12==11.4.5.107 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cusolver-cu12/nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl (124.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 124.2/124.2 MB 143.1 MB/s eta 0:00:00
Collecting nvidia-cusparse-cu12==12.1.0.106 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-cusparse-cu12/nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl (196.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 196.0/196.0 MB 137.6 MB/s eta 0:00:00
Collecting nvidia-nccl-cu12==2.20.5 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-nccl-cu12/nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_x86_64.whl (176.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 176.2/176.2 MB 123.9 MB/s eta 0:00:00
Collecting nvidia-nvtx-cu12==12.1.105 (from torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-nvtx-cu12/nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl (99 kB)
Collecting triton==3.0.0 (from torch==2.4.1)
  Downloading triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (1.3 kB)
Collecting nvidia-nvjitlink-cu12 (from nvidia-cusolver-cu12==11.4.5.107->torch==2.4.1)
  Downloading https://pypi.nvidia.com/nvidia-nvjitlink-cu12/nvidia_nvjitlink_cu12-12.6.85-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl (19.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 19.7/19.7 MB 133.0 MB/s eta 0:00:00
Collecting MarkupSafe>=2.0 (from jinja2->torch==2.4.1)
  Downloading MarkupSafe-3.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (4.0 kB)
Collecting mpmath<1.4,>=1.1.0 (from sympy->torch==2.4.1)
  Downloading mpmath-1.3.0-py3-none-any.whl.metadata (8.6 kB)
Downloading torch-2.4.1-cp310-cp310-manylinux1_x86_64.whl (797.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 797.1/797.1 MB 148.2 MB/s eta 0:00:00
Downloading nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl (664.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 664.8/664.8 MB 147.5 MB/s eta 0:00:00
Downloading triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (209.4 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 209.4/209.4 MB 147.0 MB/s eta 0:00:00
Downloading typing_extensions-4.12.2-py3-none-any.whl (37 kB)
Downloading filelock-3.16.1-py3-none-any.whl (16 kB)
Downloading fsspec-2024.12.0-py3-none-any.whl (183 kB)
Downloading jinja2-3.1.5-py3-none-any.whl (134 kB)
Downloading networkx-3.4.2-py3-none-any.whl (1.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 154.0 MB/s eta 0:00:00
Downloading sympy-1.13.3-py3-none-any.whl (6.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.2/6.2 MB 150.2 MB/s eta 0:00:00
Downloading MarkupSafe-3.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (20 kB)
Downloading mpmath-1.3.0-py3-none-any.whl (536 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 536.2/536.2 kB 293.3 MB/s eta 0:00:00
Installing collected packages: mpmath, typing-extensions, sympy, nvidia-nvtx-cu12, nvidia-nvjitlink-cu12, nvidia-nccl-cu12, nvidia-curand-cu12, nvidia-cufft-cu12, nvidia-cuda-runtime-cu12, nvidia-cuda-nvrtc-cu12, nvidia-cuda-cupti-cu12, nvidia-cublas-cu12, networkx, MarkupSafe, fsspec, filelock, triton, nvidia-cusparse-cu12, nvidia-cudnn-cu12, jinja2, nvidia-cusolver-cu12, torch
Successfully installed MarkupSafe-3.0.2 filelock-3.16.1 fsspec-2024.12.0 jinja2-3.1.5 mpmath-1.3.0 networkx-3.4.2 nvidia-cublas-cu12-12.1.3.1 nvidia-cuda-cupti-cu12-12.1.105 nvidia-cuda-nvrtc-cu12-12.1.105 nvidia-cuda-runtime-cu12-12.1.105 nvidia-cudnn-cu12-9.1.0.70 nvidia-cufft-cu12-11.0.2.54 nvidia-curand-cu12-10.3.2.106 nvidia-cusolver-cu12-11.4.5.107 nvidia-cusparse-cu12-12.1.0.106 nvidia-nccl-cu12-2.20.5 nvidia-nvjitlink-cu12-12.6.85 nvidia-nvtx-cu12-12.1.105 sympy-1.13.3 torch-2.4.1 triton-3.0.0 typing-extensions-4.12.2
```
</details>

# Actual results
`uv` is unable to resolve dependencies

```shell
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==9.1.0.70 and torch==2.4.1 depends on nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform ==
      'linux'}==9.1.0.70, we can conclude that torch==2.4.1 cannot be used.
      And because you require torch==2.4.1, we can conclude that your requirements are unsatisfiable.
```

<details>
<summary>Full uv pip output</summary>

```shell
$ uv pip install torch==2.4.1 --extra-index-url https://pypi.nvidia.com --verbose
DEBUG uv 0.5.18
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/pharmapsychotic/secret/venv/bin/python3` (active virtual environment)
Using Python 3.10.16 environment at: venv_test
DEBUG Acquired lock for `venv_test`
DEBUG At least one requirement is not satisfied: torch==2.4.1
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.16
DEBUG Solving with target Python version: >=3.10.16
DEBUG Adding direct dependency: torch>=2.4.1, <2.4.1+
DEBUG No cache entry for: https://pypi.nvidia.com/torch/
DEBUG Checking netrc for credentials for https://pypi.nvidia.com/torch/
DEBUG Found stale response for: https://pypi.org/simple/torch/
DEBUG Sending revalidation request for: https://pypi.org/simple/torch/
DEBUG Found not-modified response for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (>=2.4.1, <2.4.1+)
DEBUG Selecting: torch==2.4.1 [compatible] (torch-2.4.1-cp310-cp310-manylinux1_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/05/d540049b1832d1062510efc6829634b7fbef5394c757d8312414fb65a3cb/torch-2.4.1-cp310-cp310-manylinux1_x86_64.whl.metadata
DEBUG Adding transitive dependency for torch==2.4.1: filelock*
DEBUG Adding transitive dependency for torch==2.4.1: typing-extensions>=4.8.0
DEBUG Adding transitive dependency for torch==2.4.1: sympy*
DEBUG Adding transitive dependency for torch==2.4.1: networkx*
DEBUG Adding transitive dependency for torch==2.4.1: jinja2*
DEBUG Adding transitive dependency for torch==2.4.1: fsspec*
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.1.105, <12.1.105+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.1.105, <12.1.105+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.1.105, <12.1.105+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=9.1.0.70, <9.1.0.70+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.1.3.1, <12.1.3.1+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.0.2.54, <11.0.2.54+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=10.3.2.106, <10.3.2.106+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.4.5.107, <11.4.5.107+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.1.0.106, <12.1.0.106+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=2.20.5, <2.20.5+
DEBUG Adding transitive dependency for torch==2.4.1: nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.1.105, <12.1.105+
DEBUG Adding transitive dependency for torch==2.4.1: triton{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}>=3.0.0, <3.0.0+
DEBUG No cache entry for: https://pypi.nvidia.com/filelock/
DEBUG No cache entry for: https://pypi.nvidia.com/typing-extensions/
DEBUG No cache entry for: https://pypi.nvidia.com/sympy/
DEBUG No cache entry for: https://pypi.nvidia.com/networkx/
DEBUG No cache entry for: https://pypi.nvidia.com/jinja2/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cuda-runtime-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cuda-cupti-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cudnn-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cublas-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cufft-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-curand-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cusparse-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-nccl-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-nvtx-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/triton/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cuda-nvrtc-cu12/
DEBUG No cache entry for: https://pypi.nvidia.com/fsspec/
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cusolver-cu12/
DEBUG Found stale response for: https://pypi.org/simple/triton/
DEBUG Sending revalidation request for: https://pypi.org/simple/triton/
DEBUG Found stale response for: https://pypi.org/simple/filelock/
DEBUG Sending revalidation request for: https://pypi.org/simple/filelock/
DEBUG Found stale response for: https://pypi.org/simple/fsspec/
DEBUG Sending revalidation request for: https://pypi.org/simple/fsspec/
DEBUG Found stale response for: https://pypi.org/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-extensions/
DEBUG Found stale response for: https://pypi.org/simple/sympy/
DEBUG Sending revalidation request for: https://pypi.org/simple/sympy/
DEBUG Found stale response for: https://pypi.org/simple/jinja2/
DEBUG Sending revalidation request for: https://pypi.org/simple/jinja2/
DEBUG Found stale response for: https://pypi.org/simple/networkx/
DEBUG Sending revalidation request for: https://pypi.org/simple/networkx/
DEBUG Found not-modified response for: https://pypi.org/simple/triton/
DEBUG Searching for a compatible version of triton{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'} (>=3.0.0, <3.0.0+)
DEBUG Selecting: triton==3.0.0 [compatible] (triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl)
DEBUG Adding transitive dependency for triton==3.0.0: triton==3.0.0
DEBUG Adding transitive dependency for triton==3.0.0: triton{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}==3.0.0
DEBUG Searching for a compatible version of nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.1.105, <12.1.105+)
DEBUG Selecting: nvidia-nvtx-cu12==12.1.105 [compatible] (nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-nvtx-cu12==12.1.105: nvidia-nvtx-cu12==12.1.105
DEBUG Adding transitive dependency for nvidia-nvtx-cu12==12.1.105: nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.1.105
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/45/27/14cc3101409b9b4b9241d2ba7deaa93535a217a211c86c4cc7151fb12181/triton-3.0.0-1-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-nvtx-cu12/nvidia_nvtx_cu12-12.1.105-py3-none-manylinux1_x86_64.whl#sha256=dc21cf308ca5691e7c04d962e213f8a4aa9bbfa23d95412f452254c2caeb09e5
DEBUG Searching for a compatible version of nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=2.20.5, <2.20.5+)
DEBUG Selecting: nvidia-nccl-cu12==2.20.5 [compatible] (nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-nccl-cu12==2.20.5: nvidia-nccl-cu12==2.20.5
DEBUG Adding transitive dependency for nvidia-nccl-cu12==2.20.5: nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==2.20.5
DEBUG Searching for a compatible version of nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.1.0.106, <12.1.0.106+)
DEBUG Selecting: nvidia-cusparse-cu12==12.1.0.106 [compatible] (nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-cusparse-cu12==12.1.0.106
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.1.0.106
DEBUG Searching for a compatible version of nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=11.4.5.107, <11.4.5.107+)
DEBUG Selecting: nvidia-cusolver-cu12==11.4.5.107 [compatible] (nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusolver-cu12==11.4.5.107
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==11.4.5.107
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-nccl-cu12/nvidia_nccl_cu12-2.20.5-py3-none-manylinux2014_x86_64.whl#sha256=057f6bf9685f75215d0c53bf3ac4a10b3e6578351de307abad9e18a99182af56
DEBUG Searching for a compatible version of nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=10.3.2.106, <10.3.2.106+)
DEBUG Selecting: nvidia-curand-cu12==10.3.2.106 [compatible] (nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl)
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cusparse-cu12/nvidia_cusparse_cu12-12.1.0.106-py3-none-manylinux1_x86_64.whl#sha256=f3b50f42cf363f86ab21f720998517a659a48131e8d538dc02f8768237bd884c
DEBUG Adding transitive dependency for nvidia-curand-cu12==10.3.2.106: nvidia-curand-cu12==10.3.2.106
DEBUG Adding transitive dependency for nvidia-curand-cu12==10.3.2.106: nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==10.3.2.106
DEBUG Searching for a compatible version of nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=11.0.2.54, <11.0.2.54+)
DEBUG Selecting: nvidia-cufft-cu12==11.0.2.54 [compatible] (nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.0.2.54: nvidia-cufft-cu12==11.0.2.54
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.0.2.54: nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==11.0.2.54
DEBUG Searching for a compatible version of nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.1.3.1, <12.1.3.1+)
DEBUG Selecting: nvidia-cublas-cu12==12.1.3.1 [compatible] (nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cublas-cu12==12.1.3.1: nvidia-cublas-cu12==12.1.3.1
DEBUG Adding transitive dependency for nvidia-cublas-cu12==12.1.3.1: nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.1.3.1
DEBUG Searching for a compatible version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=9.1.0.70, <9.1.0.70+)
DEBUG No compatible version found for: nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}
DEBUG Recording unit propagation conflict of nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} from incompatibility of (torch)
DEBUG Searching for a compatible version of torch (>2.4.1, <2.4.1+)
DEBUG No compatible version found for: torch
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cusolver-cu12/nvidia_cusolver_cu12-11.4.5.107-py3-none-manylinux1_x86_64.whl#sha256=8a7ec542f0412294b15072fa7dab71d31334014a69f953004ea7a118206fe0dd
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-curand-cu12/nvidia_curand_cu12-10.3.2.106-py3-none-manylinux1_x86_64.whl#sha256=9d264c5036dde4e64f1de8c50ae753237c12e0b1348738169cd0f8a536c0e1e0
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cufft-cu12/nvidia_cufft_cu12-11.0.2.54-py3-none-manylinux1_x86_64.whl#sha256=794e3948a1aa71fd817c3775866943936774d1c14e7628c74f6f7417224cdf56
DEBUG No cache entry for: https://pypi.nvidia.com/nvidia-cublas-cu12/nvidia_cublas_cu12-12.1.3.1-py3-none-manylinux1_x86_64.whl#sha256=ee53ccca76a6fc08fb9701aa95b6ceb242cdaab118c3bb152af4e579af792728
DEBUG Found not-modified response for: https://pypi.org/simple/filelock/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/f8/feced7779d755758a52d1f6635d990b8d98dc0a29fa568bbe0625f18fdf3/filelock-3.16.1-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/fsspec/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/networkx/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/de/86/5486b0188d08aa643e127774a99bac51ffa6cf343e3deb0583956dca5b22/fsspec-2024.12.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/sympy/
DEBUG Found not-modified response for: https://pypi.org/simple/jinja2/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bd/0f/2ba5fbcd631e3e88689309dbe978c5769e883e4b84ebfe7da30b43275c5a/jinja2-3.1.5-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/99/ff/c87e0622b1dadea79d2fb0b25ade9ed98954c9033722eb707053d310d4f3/sympy-1.13.3-py3-none-any.whl.metadata
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==9.1.0.70 and torch==2.4.1 depends on nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform ==
      'linux'}==9.1.0.70, we can conclude that torch==2.4.1 cannot be used.
      And because you require torch==2.4.1, we can conclude that your requirements are unsatisfiable.
```
</details>

# Extra details

Also tried giving additional indexes on CLI but get the same error.
```shell
uv pip install torch==2.4.1 \
    --index-url https://pypi.org/simple \
    --extra-index-url https://pypi.ngc.nvidia.com \
    --extra-index-url https://pypi.nvidia.com
```

Running `pip index` 9.1.0.70 doesn't appear
```shell
$ pip index versions nvidia-cudnn-cu12 --index-url https://pypi.nvidia.com
nvidia-cudnn-cu12 (9.6.0.74)
Available versions: 9.6.0.74, 9.5.1.17, 9.5.0.50, 9.3.0.75, 9.2.1.18, 9.2.0.82, 9.1.1.17, 9.0.0.312, 8.9.7.29, 8.9.6.50, 8.9.5.30, 8.9.4.25, 8.9.2.26, 8.9.1.23, 8.9.0.131, 8.8.1.3
```
So maybe the `uv` behavior is correct? But `pip` is able to install this torch version and it is the version I need pinned so I'm working around the issue by using `pip` to install torch and then `uv` to install the rest of my dependencies. I'd love to get `uv` working also for torch to further speed up my builds though. 

# Environment
- OS: Ubuntu 22.04 / Linux x86_64
- Python: 3.10.16
- `uv`: 0.5.18
- `pip`: 24.3.1


---

_Comment by @charliermarsh on 2025-01-14 17:11_

Did you try with `--index-strategy unsafe-best-match`?

---

_Comment by @pharmapsychotic on 2025-01-14 17:15_

> Did you try with `--index-strategy unsafe-best-match`?

Thank you, I was unfamiliar with this option. It works!

---

_Closed by @pharmapsychotic on 2025-01-14 17:23_

---

_Comment by @zanieb on 2025-01-14 17:29_

https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes

---
