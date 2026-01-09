---
number: 12140
title: "Discrepancy in environment creation between projects and `uv run`"
type: issue
state: open
author: antoinebrl
labels:
  - bug
assignees: []
created_at: 2025-03-12T18:02:17Z
updated_at: 2025-03-12T22:03:13Z
url: https://github.com/astral-sh/uv/issues/12140
synced_at: 2026-01-07T13:12:18-06:00
---

# Discrepancy in environment creation between projects and `uv run`

---

_Issue opened by @antoinebrl on 2025-03-12 18:02_

### Summary

Hello 👋! I am observing a discrepancy between packages installed as part of a project or as a one off command. More specifically, in certain contexts I get an error when running `import torch.utils.benchmark as benchmark`. This module depends on `setuptools`. Here is the error:

```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/coder/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/benchmark/__init__.py", line 2, in <module>
    from torch.utils.benchmark.utils.timer import *  # noqa: F403
  File "/home/coder/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/benchmark/utils/timer.py", line 8, in <module>
    from torch.utils.benchmark.utils import common, cpp_jit
  File "/home/coder/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/benchmark/utils/cpp_jit.py", line 13, in <module>
    from torch.utils import cpp_extension
  File "/home/coder/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/cpp_extension.py", line 11, in <module>
    import setuptools
ModuleNotFoundError: No module named 'setuptools'
```

If I run this command things work fine, and the process exists with error code zero:
```shell
$ uv run -v --no-project --no-config --no-cache \
      --with torch==2.6 --python 3.10 --python-preference only-managed \
      -- python -c "import torch.utils.benchmark as benchmark"
```

<details markdown="1">
<summary>uv run - logs</summary>

```
$ uv run -v --no-project --no-config --no-cache --with torch==2.6 --python 3.10 --python-preference only-managed -- python -c "import torch.utils.benchmark as benchmark"

DEBUG uv 0.6.6 (c1a0bb85e 2025-03-12)
DEBUG Found project root: `/Users/antoine/Projects/tmp/uv-venv-bug`
DEBUG No workspace root found, using project root
DEBUG Ignoring discovered project due to `--no-project`
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.10 in virtual environments or managed installations
DEBUG Searching for managed installations at `/Users/antoine/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.8-macos-aarch64-none`
DEBUG Found managed installation `cpython-3.10.16-macos-aarch64-none`
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `/Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10` (managed installations)
DEBUG Using Python 3.10.16 interpreter at: /Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
DEBUG At least one requirement is not satisfied in the base environment: torch==2.6
DEBUG Syncing ephemeral requirements
DEBUG Assessing Python executable as base candidate: /Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
DEBUG Caching via base interpreter: `/Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.16
DEBUG Solving with target Python version: >=3.10.16
DEBUG Adding direct dependency: torch>=2.6, <2.6+
DEBUG No cache entry for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (>=2.6, <2.6+)
DEBUG Selecting: torch==2.6.0 [compatible] (torch-2.6.0-cp310-none-macosx_11_0_arm64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e5/16/ea1b7842413a7b8a5aaa5e99e8eaf3da3183cc3ab345ad025a07ff636301/torch-2.6.0-cp310-none-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for torch==2.6.0: filelock*
DEBUG Adding transitive dependency for torch==2.6.0: typing-extensions>=4.10.0
DEBUG Adding transitive dependency for torch==2.6.0: networkx*
DEBUG Adding transitive dependency for torch==2.6.0: jinja2*
DEBUG Adding transitive dependency for torch==2.6.0: fsspec*
DEBUG Adding transitive dependency for torch==2.6.0: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+
DEBUG No cache entry for: https://pypi.org/simple/filelock/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG No cache entry for: https://pypi.org/simple/networkx/
DEBUG No cache entry for: https://pypi.org/simple/fsspec/
DEBUG No cache entry for: https://pypi.org/simple/sympy/
DEBUG No cache entry for: https://pypi.org/simple/jinja2/
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (>=1.13.1, <1.13.1+)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: sympy==1.13.1
DEBUG Adding transitive dependency for sympy==1.13.1: sympy{python_full_version >= '3.9'}==1.13.1
DEBUG Searching for a compatible version of sympy (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b2/fe/81695a1aa331a842b582453b605175f419fe8540355886031328089d840a/sympy-1.13.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG No cache entry for: https://pypi.org/simple/mpmath/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/56/53/eb690efa8513166adef3e0669afd31e95ffde69fb3c52ec2ac7223ed6018/fsspec-2025.3.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.17.0 [compatible] (filelock-3.17.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/89/ec/00d68c4ddfedfe64159999e5f8a98fb8442729a63e2077eb9dcd89623d27/filelock-3.17.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=4.10.0)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.4.2 [compatible] (networkx-3.4.2-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.6 [compatible] (jinja2-3.1.6-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.6: markupsafe>=2.0
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Selecting: fsspec==2025.3.0 [compatible] (fsspec-2025.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of mpmath (>=1.1.0, <1.4)
DEBUG Selecting: mpmath==1.3.0 [compatible] (mpmath-1.3.0-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/markupsafe/
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [compatible] (MarkupSafe-3.0.2-cp310-cp310-macosx_11_0_arm64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/e1/6e2194baeae0bca1fae6629dc0cbbb968d4d941469cbab11a3872edff374/MarkupSafe-3.0.2-cp310-cp310-macosx_11_0_arm64.whl.metadata
DEBUG Tried 9 versions: filelock 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, networkx 1, sympy 1, torch 1, typing-extensions 1
DEBUG marker environment resolution took 4.145s
Resolved 9 packages in 4.14s
DEBUG Assessing Python executable as base candidate: /Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
DEBUG Using base executable for virtual environment: /Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
DEBUG Ignoring empty directory
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: markupsafe==3.0.2
DEBUG Identified uncached distribution: torch==2.6.0
DEBUG Identified uncached distribution: jinja2==3.1.6
DEBUG Identified uncached distribution: filelock==3.17.0
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Identified uncached distribution: fsspec==2025.3.0
DEBUG Identified uncached distribution: mpmath==1.3.0
DEBUG Identified uncached distribution: networkx==3.4.2
DEBUG Identified uncached distribution: sympy==1.13.1
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e5/16/ea1b7842413a7b8a5aaa5e99e8eaf3da3183cc3ab345ad025a07ff636301/torch-2.6.0-cp310-none-macosx_11_0_arm64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b2/fe/81695a1aa331a842b582453b605175f419fe8540355886031328089d840a/sympy-1.13.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/56/53/eb690efa8513166adef3e0669afd31e95ffde69fb3c52ec2ac7223ed6018/fsspec-2025.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/89/ec/00d68c4ddfedfe64159999e5f8a98fb8442729a63e2077eb9dcd89623d27/filelock-3.17.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/e1/6e2194baeae0bca1fae6629dc0cbbb968d4d941469cbab11a3872edff374/MarkupSafe-3.0.2-cp310-cp310-macosx_11_0_arm64.whl
Downloading sympy (5.9MiB)
Downloading torch (63.4MiB)
Downloading networkx (1.6MiB)
 Downloaded networkx
 Downloaded sympy
 Downloaded torch
Prepared 9 packages in 1m 00s
Installed 9 packages in 295ms
 + filelock==3.17.0
 + fsspec==2025.3.0
 + jinja2==3.1.6
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + sympy==1.13.1
 + torch==2.6.0
 + typing-extensions==4.12.2
DEBUG Checking for Python environment at `/var/folders/3n/98crvx1j4fj5fl84shkwgdd00000gn/T/.tmpV7uHWX/archive-v0/DqKWe2CY-LbimB-eGNsVz`
DEBUG Running `python -c import torch.utils.benchmark as benchmark`
DEBUG Spawned child 57502 in process group 56857
/var/folders/3n/98crvx1j4fj5fl84shkwgdd00000gn/T/.tmpV7uHWX/archive-v0/DqKWe2CY-LbimB-eGNsVz/lib/python3.10/site-packages/torch/_subclasses/functional_tensor.py:275: UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at /Users/runner/work/pytorch/pytorch/pytorch/torch/csrc/utils/tensor_numpy.cpp:81.)
  cpu = _conversion_method_template(device=torch.device("cpu"))
DEBUG Command exited with code: 0
```
</details>

However, with this `pyproject.toml` referencing the same package:
```toml
[project]
name = "my-test"
version = "0.0.0"
requires-python = "~=3.10.0"
dependencies = [
    "torch==2.6.0",
]
```
this command crashes with the error above
```
$ rm -rf .venv uv.lock && \
    uv sync -v --no-config --no-cache --python 3.10 --python-preference only-managed && \
    source .venv/bin/activate && \
    python -c "import torch.utils.benchmark as benchmark" 
```

<details markdown="1">
<summary>uv project - logs</summary>

```
$ rm -rf .venv uv.lock && uv sync -v --no-config --no-cache --python 3.10 --python-preference only-managed && source .venv/bin/activate && python -c "import torch.utils.benchmark as benchmark"

DEBUG uv 0.6.6 (c1a0bb85e 2025-03-12)
DEBUG Found project root: `/Users/antoine/Projects/tmp/uv-venv-bug`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/antoine/Projects/tmp/uv-venv-bug`
DEBUG Using Python request `3.10` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.10 in managed installations
DEBUG Searching for managed installations at `/Users/antoine/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.8-macos-aarch64-none`
DEBUG Found managed installation `cpython-3.10.16-macos-aarch64-none`
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `/Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10` (managed installations)
Using CPython 3.10.16
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
DEBUG Using base executable for virtual environment: /Users/antoine/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
DEBUG Released lock at `/var/folders/3n/98crvx1j4fj5fl84shkwgdd00000gn/T/uv-2b18024b68954af5.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: my-test @ file:///Users/antoine/Projects/tmp/uv-venv-bug
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.10.16
DEBUG Solving with target Python version: >=3.10.0, <3.11
DEBUG Adding direct dependency: my-test*
DEBUG Searching for a compatible version of my-test @ file:///Users/antoine/Projects/tmp/uv-venv-bug (*)
DEBUG Adding direct dependency: torch>=2.6.0, <2.6.0+
DEBUG No cache entry for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (>=2.6.0, <2.6.0+)
DEBUG Selecting: torch==2.6.0 [compatible] (torch-2.6.0-cp310-cp310-manylinux1_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/37/81/aa9ab58ec10264c1abe62c8b73f5086c3c558885d6beecebf699f0dbeaeb/torch-2.6.0-cp310-cp310-manylinux1_x86_64.whl.metadata
DEBUG Adding transitive dependency for torch==2.6.0: filelock*
DEBUG Adding transitive dependency for torch==2.6.0: fsspec*
DEBUG Adding transitive dependency for torch==2.6.0: jinja2*
DEBUG Adding transitive dependency for torch==2.6.0: networkx*
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.4.5.8, <12.4.5.8+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.4.127, <12.4.127+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.4.127, <12.4.127+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.4.127, <12.4.127+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=9.1.0.70, <9.1.0.70+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.2.1.3, <11.2.1.3+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=10.3.5.147, <10.3.5.147+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.6.1.9, <11.6.1.9+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.3.1.170, <12.3.1.170+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-cusparselt-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=0.6.2, <0.6.2+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=2.21.5, <2.21.5+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-nvjitlink-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.4.127, <12.4.127+
DEBUG Adding transitive dependency for torch==2.6.0: nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.4.127, <12.4.127+
DEBUG Adding transitive dependency for torch==2.6.0: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+
DEBUG Adding transitive dependency for torch==2.6.0: triton{platform_machine == 'x86_64' and sys_platform == 'linux'}>=3.2.0, <3.2.0+
DEBUG Adding transitive dependency for torch==2.6.0: typing-extensions>=4.10.0
DEBUG No cache entry for: https://pypi.org/simple/filelock/
DEBUG No cache entry for: https://pypi.org/simple/fsspec/
DEBUG No cache entry for: https://pypi.org/simple/networkx/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG No cache entry for: https://pypi.org/simple/jinja2/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cuda-cupti-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cuda-runtime-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cuda-nvrtc-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cudnn-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-curand-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cusolver-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cusparse-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cufft-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-nvtx-cu12/
DEBUG No cache entry for: https://pypi.org/simple/triton/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-cusparselt-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-nvjitlink-cu12/
DEBUG No cache entry for: https://pypi.org/simple/nvidia-nccl-cu12/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG No cache entry for: https://pypi.org/simple/sympy/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/56/53/eb690efa8513166adef3e0669afd31e95ffde69fb3c52ec2ac7223ed6018/fsspec-2025.3.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/89/ec/00d68c4ddfedfe64159999e5f8a98fb8442729a63e2077eb9dcd89623d27/filelock-3.17.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.4.5.8, <12.4.5.8+)
DEBUG Selecting: nvidia-cublas-cu12==12.4.5.8 [compatible] (nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cublas-cu12==12.4.5.8: nvidia-cublas-cu12==12.4.5.8
DEBUG Adding transitive dependency for nvidia-cublas-cu12==12.4.5.8: nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.4.5.8
DEBUG Searching for a compatible version of nvidia-cublas-cu12 (==12.4.5.8)
DEBUG Selecting: nvidia-cublas-cu12==12.4.5.8 [compatible] (nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/7f/7fbae15a3982dc9595e49ce0f19332423b260045d0a6afe93cdbe2f1f624/nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.4.5.8)
DEBUG Selecting: nvidia-cublas-cu12==12.4.5.8 [compatible] (nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.4.127, <12.4.127+)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.4.127 [compatible] (nvidia_cuda_cupti_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cuda-cupti-cu12==12.4.127: nvidia-cuda-cupti-cu12==12.4.127
DEBUG Adding transitive dependency for nvidia-cuda-cupti-cu12==12.4.127: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.4.127
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12 (==12.4.127)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.4.127 [compatible] (nvidia_cuda_cupti_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/93/b5/9fb3d00386d3361b03874246190dfec7b206fd74e6e287b26a8fcb359d95/nvidia_cuda_cupti_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.4.127)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.4.127 [compatible] (nvidia_cuda_cupti_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.4.127, <12.4.127+)
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.4.127 [compatible] (nvidia_cuda_nvrtc_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cuda-nvrtc-cu12==12.4.127: nvidia-cuda-nvrtc-cu12==12.4.127
DEBUG Adding transitive dependency for nvidia-cuda-nvrtc-cu12==12.4.127: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.4.127
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12 (==12.4.127)
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.4.127 [compatible] (nvidia_cuda_nvrtc_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/77/aa/083b01c427e963ad0b314040565ea396f914349914c298556484f799e61b/nvidia_cuda_nvrtc_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.4.127)
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.4.127 [compatible] (nvidia_cuda_nvrtc_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.4.127, <12.4.127+)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.4.127 [compatible] (nvidia_cuda_runtime_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cuda-runtime-cu12==12.4.127: nvidia-cuda-runtime-cu12==12.4.127
DEBUG Adding transitive dependency for nvidia-cuda-runtime-cu12==12.4.127: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.4.127
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12 (==12.4.127)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.4.127 [compatible] (nvidia_cuda_runtime_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a1/aa/b656d755f474e2084971e9a297def515938d56b466ab39624012070cb773/nvidia_cuda_runtime_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.4.127)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.4.127 [compatible] (nvidia_cuda_runtime_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=9.1.0.70, <9.1.0.70+)
DEBUG Selecting: nvidia-cudnn-cu12==9.1.0.70 [compatible] (nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cudnn-cu12==9.1.0.70
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==9.1.0.70
DEBUG Searching for a compatible version of nvidia-cudnn-cu12 (==9.1.0.70)
DEBUG Selecting: nvidia-cudnn-cu12==9.1.0.70 [compatible] (nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/9f/fd/713452cd72343f682b1c7b9321e23829f00b842ceaedcda96e742ea0b0b3/nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cublas-cu12*
DEBUG Searching for a compatible version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==9.1.0.70)
DEBUG Selecting: nvidia-cudnn-cu12==9.1.0.70 [compatible] (nvidia_cudnn_cu12-9.1.0.70-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.1.0.70: nvidia-cublas-cu12*
DEBUG Searching for a compatible version of nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=11.2.1.3, <11.2.1.3+)
DEBUG Selecting: nvidia-cufft-cu12==11.2.1.3 [compatible] (nvidia_cufft_cu12-11.2.1.3-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.2.1.3: nvidia-cufft-cu12==11.2.1.3
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.2.1.3: nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==11.2.1.3
DEBUG Searching for a compatible version of nvidia-cufft-cu12 (==11.2.1.3)
DEBUG Selecting: nvidia-cufft-cu12==11.2.1.3 [compatible] (nvidia_cufft_cu12-11.2.1.3-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7a/8a/0e728f749baca3fbeffad762738276e5df60851958be7783af121a7221e7/nvidia_cufft_cu12-11.2.1.3-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.2.1.3: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==11.2.1.3)
DEBUG Selecting: nvidia-cufft-cu12==11.2.1.3 [compatible] (nvidia_cufft_cu12-11.2.1.3-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.2.1.3: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=10.3.5.147, <10.3.5.147+)
DEBUG Selecting: nvidia-curand-cu12==10.3.5.147 [compatible] (nvidia_curand_cu12-10.3.5.147-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-curand-cu12==10.3.5.147: nvidia-curand-cu12==10.3.5.147
DEBUG Adding transitive dependency for nvidia-curand-cu12==10.3.5.147: nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==10.3.5.147
DEBUG Searching for a compatible version of nvidia-curand-cu12 (==10.3.5.147)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f6/74/86a07f1d0f42998ca31312f998bd3b9a7eff7f52378f4f270c8679c77fb9/nvidia_nvjitlink_cu12-12.8.93-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl.metadata
DEBUG Selecting: nvidia-curand-cu12==10.3.5.147 [compatible] (nvidia_curand_cu12-10.3.5.147-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/80/9c/a79180e4d70995fdf030c6946991d0171555c6edf95c265c6b2bf7011112/nvidia_curand_cu12-10.3.5.147-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==10.3.5.147)
DEBUG Selecting: nvidia-curand-cu12==10.3.5.147 [compatible] (nvidia_curand_cu12-10.3.5.147-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=11.6.1.9, <11.6.1.9+)
DEBUG Selecting: nvidia-cusolver-cu12==11.6.1.9 [compatible] (nvidia_cusolver_cu12-11.6.1.9-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-cusolver-cu12==11.6.1.9
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==11.6.1.9
DEBUG Searching for a compatible version of nvidia-cusolver-cu12 (==11.6.1.9)
DEBUG Selecting: nvidia-cusolver-cu12==11.6.1.9 [compatible] (nvidia_cusolver_cu12-11.6.1.9-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/46/6b/a5c33cf16af09166845345275c34ad2190944bcc6026797a39f8e0a282e0/nvidia_cusolver_cu12-11.6.1.9-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-cublas-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-cusparse-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==11.6.1.9)
DEBUG Selecting: nvidia-cusolver-cu12==11.6.1.9 [compatible] (nvidia_cusolver_cu12-11.6.1.9-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-cublas-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-cusparse-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.1.9: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.3.1.170, <12.3.1.170+)
DEBUG Selecting: nvidia-cusparse-cu12==12.3.1.170 [compatible] (nvidia_cusparse_cu12-12.3.1.170-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.3.1.170: nvidia-cusparse-cu12==12.3.1.170
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.3.1.170: nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.3.1.170
DEBUG Searching for a compatible version of nvidia-cusparse-cu12 (==12.3.1.170)
DEBUG Selecting: nvidia-cusparse-cu12==12.3.1.170 [compatible] (nvidia_cusparse_cu12-12.3.1.170-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/bc/f7/cd777c4109681367721b00a106f491e0d0d15cfa1fd59672ce580ce42a97/nvidia_cusparse_cu12-12.5.8.93-py3-none-manylinux2014_aarch64.manylinux_2_17_aarch64.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/96/a9/c0d2f83a53d40a4a41be14cea6a0bf9e668ffcf8b004bd65633f433050c0/nvidia_cusparse_cu12-12.3.1.170-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.3.1.170: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.3.1.170)
DEBUG Selecting: nvidia-cusparse-cu12==12.3.1.170 [compatible] (nvidia_cusparse_cu12-12.3.1.170-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.3.1.170: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusparselt-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=0.6.2, <0.6.2+)
DEBUG Selecting: nvidia-cusparselt-cu12==0.6.2 [compatible] (nvidia_cusparselt_cu12-0.6.2-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-cusparselt-cu12==0.6.2: nvidia-cusparselt-cu12==0.6.2
DEBUG Adding transitive dependency for nvidia-cusparselt-cu12==0.6.2: nvidia-cusparselt-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==0.6.2
DEBUG Searching for a compatible version of nvidia-cusparselt-cu12 (==0.6.2)
DEBUG Selecting: nvidia-cusparselt-cu12==0.6.2 [compatible] (nvidia_cusparselt_cu12-0.6.2-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/98/8e/675498726c605c9441cf46653bd29cb1b8666da1fb1469ffa25f67f20c58/nvidia_cusparselt_cu12-0.6.2-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-cusparselt-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==0.6.2)
DEBUG Selecting: nvidia-cusparselt-cu12==0.6.2 [compatible] (nvidia_cusparselt_cu12-0.6.2-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=2.21.5, <2.21.5+)
DEBUG Selecting: nvidia-nccl-cu12==2.21.5 [compatible] (nvidia_nccl_cu12-2.21.5-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-nccl-cu12==2.21.5: nvidia-nccl-cu12==2.21.5
DEBUG Adding transitive dependency for nvidia-nccl-cu12==2.21.5: nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==2.21.5
DEBUG Searching for a compatible version of nvidia-nccl-cu12 (==2.21.5)
DEBUG Selecting: nvidia-nccl-cu12==2.21.5 [compatible] (nvidia_nccl_cu12-2.21.5-py3-none-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/df/99/12cd266d6233f47d00daf3a72739872bdc10267d0383508b0b9c84a18bb6/nvidia_nccl_cu12-2.21.5-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==2.21.5)
DEBUG Selecting: nvidia-nccl-cu12==2.21.5 [compatible] (nvidia_nccl_cu12-2.21.5-py3-none-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-nvjitlink-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.4.127, <12.4.127+)
DEBUG Selecting: nvidia-nvjitlink-cu12==12.4.127 [compatible] (nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-nvjitlink-cu12==12.4.127: nvidia-nvjitlink-cu12==12.4.127
DEBUG Adding transitive dependency for nvidia-nvjitlink-cu12==12.4.127: nvidia-nvjitlink-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.4.127
DEBUG Searching for a compatible version of nvidia-nvjitlink-cu12 (==12.4.127)
DEBUG Selecting: nvidia-nvjitlink-cu12==12.4.127 [compatible] (nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/02/45/239d52c05074898a80a900f49b1615d81c07fceadd5ad6c4f86a987c0bc4/nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-nvjitlink-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.4.127)
DEBUG Selecting: nvidia-nvjitlink-cu12==12.4.127 [compatible] (nvidia_nvjitlink_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=12.4.127, <12.4.127+)
DEBUG Selecting: nvidia-nvtx-cu12==12.4.127 [compatible] (nvidia_nvtx_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for nvidia-nvtx-cu12==12.4.127: nvidia-nvtx-cu12==12.4.127
DEBUG Adding transitive dependency for nvidia-nvtx-cu12==12.4.127: nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}==12.4.127
DEBUG Searching for a compatible version of nvidia-nvtx-cu12 (==12.4.127)
DEBUG Selecting: nvidia-nvtx-cu12==12.4.127 [compatible] (nvidia_nvtx_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/06/39/471f581edbb7804b39e8063d92fc8305bdc7a80ae5c07dbe6ea5c50d14a5/nvidia_nvtx_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl.metadata
DEBUG Searching for a compatible version of nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'} (==12.4.127)
DEBUG Selecting: nvidia-nvtx-cu12==12.4.127 [compatible] (nvidia_nvtx_cu12-12.4.127-py3-none-manylinux2014_aarch64.whl)
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (>=1.13.1, <1.13.1+)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: sympy==1.13.1
DEBUG Adding transitive dependency for sympy==1.13.1: sympy{python_full_version >= '3.9'}==1.13.1
DEBUG Searching for a compatible version of sympy (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b2/fe/81695a1aa331a842b582453b605175f419fe8540355886031328089d840a/sympy-1.13.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of triton{platform_machine == 'x86_64' and sys_platform == 'linux'} (>=3.2.0, <3.2.0+)
DEBUG Selecting: triton==3.2.0 [compatible] (triton-3.2.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for triton==3.2.0: triton==3.2.0
DEBUG Adding transitive dependency for triton==3.2.0: triton{platform_machine == 'x86_64' and sys_platform == 'linux'}==3.2.0
DEBUG Searching for a compatible version of triton (==3.2.0)
DEBUG Selecting: triton==3.2.0 [compatible] (triton-3.2.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.org/simple/mpmath/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/01/65/3ffa90e158a2c82f0716eee8d26a725d241549b7d7aaf7e4f44ac03ebd89/triton-3.2.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of triton{platform_machine == 'x86_64' and sys_platform == 'linux'} (==3.2.0)
DEBUG Selecting: triton==3.2.0 [compatible] (triton-3.2.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.17.0 [compatible] (filelock-3.17.0-py3-none-any.whl)
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Selecting: fsspec==2025.3.0 [compatible] (fsspec-2025.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.6 [compatible] (jinja2-3.1.6-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.6: markupsafe>=2.0
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.4.2 [compatible] (networkx-3.4.2-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=4.10.0)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/markupsafe/
DEBUG Searching for a compatible version of mpmath (>=1.1.0, <1.4)
DEBUG Selecting: mpmath==1.3.0 [compatible] (mpmath-1.3.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [compatible] (MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/90/d08277ce111dd22f77149fd1a5d4653eeb3b3eaacbdfcbae5afb2600eebd/MarkupSafe-3.0.2-cp310-cp310-macosx_10_9_universal2.whl.metadata
DEBUG Tried 24 versions: filelock 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, my-test 1, networkx 1, nvidia-cublas-cu12 1, nvidia-cuda-cupti-cu12 1, nvidia-cuda-nvrtc-cu12 1, nvidia-cuda-runtime-cu12 1, nvidia-cudnn-cu12 1, nvidia-cufft-cu12 1, nvidia-curand-cu12 1, nvidia-cusolver-cu12 1, nvidia-cusparse-cu12 1, nvidia-cusparselt-cu12 1, nvidia-nccl-cu12 1, nvidia-nvjitlink-cu12 1, nvidia-nvtx-cu12 1, sympy 1, torch 1, triton 1, typing-extensions 1
DEBUG all marker environments resolution took 1.936s
Resolved 24 packages in 1.95s
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: torch==2.6.0
DEBUG Identified uncached distribution: filelock==3.17.0
DEBUG Identified uncached distribution: fsspec==2025.3.0
DEBUG Identified uncached distribution: jinja2==3.1.6
DEBUG Identified uncached distribution: networkx==3.4.2
DEBUG Identified uncached distribution: sympy==1.13.1
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Identified uncached distribution: markupsafe==3.0.2
DEBUG Identified uncached distribution: mpmath==1.3.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e5/16/ea1b7842413a7b8a5aaa5e99e8eaf3da3183cc3ab345ad025a07ff636301/torch-2.6.0-cp310-none-macosx_11_0_arm64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b2/fe/81695a1aa331a842b582453b605175f419fe8540355886031328089d840a/sympy-1.13.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/56/53/eb690efa8513166adef3e0669afd31e95ffde69fb3c52ec2ac7223ed6018/fsspec-2025.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/62/a1/3d680cbfd5f4b8f15abc1d571870c5fc3e594bb582bc3b64ea099db13e56/jinja2-3.1.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/89/ec/00d68c4ddfedfe64159999e5f8a98fb8442729a63e2077eb9dcd89623d27/filelock-3.17.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/e1/6e2194baeae0bca1fae6629dc0cbbb968d4d941469cbab11a3872edff374/MarkupSafe-3.0.2-cp310-cp310-macosx_11_0_arm64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
Downloading networkx (1.6MiB)
Downloading torch (63.4MiB)
Downloading sympy (5.9MiB)
 Downloaded networkx
 Downloaded sympy
 Downloaded torch
Prepared 9 packages in 41.13s
Installed 9 packages in 186ms
 + filelock==3.17.0
 + fsspec==2025.3.0
 + jinja2==3.1.6
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + sympy==1.13.1
 + torch==2.6.0
 + typing-extensions==4.12.2
  cpu = _conversion_method_template(device=torch.device("cpu"))
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/antoine/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/benchmark/__init__.py", line 2, in <module>
    from torch.utils.benchmark.utils.timer import *  # noqa: F403
  File "/Users/antoine/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/benchmark/utils/timer.py", line 8, in <module>
    from torch.utils.benchmark.utils import common, cpp_jit
  File "/Users/antoine/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/benchmark/utils/cpp_jit.py", line 13, in <module>
    from torch.utils import cpp_extension
  File "/Users/antoine/Projects/tmp/uv-venv-bug/.venv/lib/python3.10/site-packages/torch/utils/cpp_extension.py", line 11, in <module>
    import setuptools
ModuleNotFoundError: No module named 'setuptools'

```
</details>

As PyTorch has fewer dependencies in MacOS, I'm sharing the logs for this plateform. However, I have the exact same outcome on Ubuntu 24.03. I've also tried to switch from managed and system Python, but without any luck.

If I add `setuptools` to the project dependencies, or if I seed its venv things run smoothly.
```shell
$ rm -rf .venv uv.lock && \
    uv venv -v --seed --no-config --no-cache --python 3.10 --python-preference only-managed && \
    uv sync && source .venv/bin/activate && python -c "import torch.utils.benchmark as benchmark"
```

### Platform

macOS 15.4, Ubuntu 22.04

### Version

0.6.6 (initial investigation started under 0.6.5)

### Python version

Python 3.10

---

_Label `bug` added by @antoinebrl on 2025-03-12 18:02_

---

_Comment by @zanieb on 2025-03-12 20:02_

The difference here is that when you create a virtual environment, it is isolated from the system environment. However, when uv creates ephemeral environments for `uv run --with`, it layers them on top of a parent environment. We create a `.pth` file to do this:

https://github.com/astral-sh/uv/blob/e096ab24116cd1a2a542ba4936351523dd777ce2/crates/uv/src/commands/project/run.rs#L922-L936

In this case, since there is no existing virtual environment, it is layered on top of the system environment which includes `setuptools` on Python 3.10

```
❯ ls /Users/zb/.local/share/uv/python/cpython-3.10.15-macos-aarch64-none/lib/python3.10/site-packages/
README.txt			pip				setuptools
_distutils_hack			pip-24.1.2.dist-info		setuptools-70.3.0.dist-info
distutils-precedence.pth	pkg_resources
```

I'm hesitant to call this a bug, though I understand the behavior is confusing.

---

_Comment by @zanieb on 2025-03-12 20:03_

@charliermarsh do you think we should not insert an overlay if the parent environment is non-virtual? It seems reasonable? but perhaps breaking and a bit surprising in the case where you _do_ want `setuptools`.

---

_Comment by @zanieb on 2025-03-12 20:04_

(Thank you for the thorough report!)

---

_Comment by @antoinebrl on 2025-03-12 21:52_

Thanks for you reactivity @zanieb !

This raises two questions:
- is the environment overlay the same if we use either `--python-preference=only-system` or `--python-preference=only-managed`?
- if `setuptools` is part of the site packages, how comes it doesn't make its way into the virtual environment of the project? You've already shared that new environments are isolated from the system packages, but to me this is the surprising part. For py3.10, it seems like `torch` expects `setuptools` to be just be there. A less surprising solution would be to add the overlay for non-ephemeral environments too, maybe going against your suggestion of not including overlay for all environments with non-virtual parent. Is there an equivalent for `uv venv --seed` for projects?



---
