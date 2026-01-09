---
number: 9409
title: Installing torch+xpu on Windows
type: issue
state: closed
author: Stanley5249
labels:
  - question
assignees: []
created_at: 2024-11-25T06:48:13Z
updated_at: 2025-01-07T21:03:37Z
url: https://github.com/astral-sh/uv/issues/9409
synced_at: 2026-01-07T13:12:18-06:00
---

# Installing torch+xpu on Windows

---

_Issue opened by @Stanley5249 on 2024-11-25 06:48_

## Environment
- Windows 11
- uv 0.5.4 (c62c83c 2024-11-20)
- Python 3.12.6

## Description
I want to configure `torch+xpu` on Windows 11. It can be installed using `uv pip install` but not with `uv sync` or `uv add`.

## Reproduction Steps
In an empty directory, create a virtual environment with `uv venv`. Use the following command to install dependencies.

[Here](https://github.com/Stanley5249/torch-test-xpu/blob/master/pyproject.toml) is my pyproject.toml.

### Plan 1

With or without `explicit = true`, running `uv pip install -r pyproject.toml -v` works, but there is a difference in the versions installed.

```diff
  Package           Version
  ----------------- ---------
- filelock          3.13.1
+ filelock          3.16.1
- fsspec            2024.6.1
+ fsspec            2024.10.0
  jinja2            3.1.4
- markupsafe        2.1.5
+ markupsafe        3.0.2
  mpmath            1.3.0
- networkx          3.3
+ networkx          3.4.2
- numpy             1.26.3
+ numpy             2.1.3
  pip               24.3.1
- setuptools        70.0.0
+ setuptools        75.6.0
  sympy             1.13.1
  torch             2.5.1+xpu
  typing-extensions 4.12.2
```

<details><summary>Details</summary>
<p>

explicit = true

```
D:\torch-test-xpu>uv pip install -r pyproject.toml -v
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.6-windows-x86_64-none` at `D:\torch-test-xpu\.venv\Scripts\python.exe` (virtual environment)
DEBUG Using Python 3.12.6 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: numpy>=1.26.3
DEBUG Adding direct dependency: pip>=24.3.1
DEBUG Adding direct dependency: torch>=2.5.0
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/torch/
DEBUG Found stale response for: https://pypi.org/simple/pip/
DEBUG Sending revalidation request for: https://pypi.org/simple/pip/
DEBUG Found stale response for: https://pypi.org/simple/numpy/
DEBUG Sending revalidation request for: https://pypi.org/simple/numpy/
DEBUG Found not-modified response for: https://pypi.org/simple/numpy/
DEBUG Found not-modified response for: https://pypi.org/simple/pip/
DEBUG Searching for a compatible version of numpy (>=1.26.3)
DEBUG Selecting: numpy==2.1.3 [compatible] (numpy-2.1.3-cp312-cp312-win_amd64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ef/7d/500c9ad20238fcfcb4cb9243eede163594d7020ce87bd9610c9e02771876/pip-24.3.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a6/84/fa11dad3404b7634aaab50733581ce11e5350383311ea7a7010f464c0170/numpy-2.1.3-cp312-cp312-win_amd64.whl.metadata
DEBUG Searching for a compatible version of pip (>=24.3.1)
DEBUG Selecting: pip==24.3.1 [compatible] (pip-24.3.1-py3-none-any.whl)
DEBUG Searching for a compatible version of torch (>=2.5.0)
DEBUG Selecting: torch==2.5.1+xpu [compatible] (torch-2.5.1+xpu-cp312-cp312-win_amd64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-win_amd64.whl 
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-win_amd64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-win_amd64.whl
DEBUG Adding transitive dependency for torch==2.5.1+xpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: typing-extensions>=4.8.0
DEBUG Adding transitive dependency for torch==2.5.1+xpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+        
DEBUG Found stale response for: https://pypi.org/simple/filelock/
DEBUG Sending revalidation request for: https://pypi.org/simple/filelock/
DEBUG Found stale response for: https://pypi.org/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.org/simple/typing-extensions/
DEBUG Found stale response for: https://pypi.org/simple/networkx/
DEBUG Sending revalidation request for: https://pypi.org/simple/networkx/
DEBUG Found stale response for: https://pypi.org/simple/jinja2/
DEBUG Sending revalidation request for: https://pypi.org/simple/jinja2/
DEBUG Found stale response for: https://pypi.org/simple/fsspec/
DEBUG Sending revalidation request for: https://pypi.org/simple/fsspec/
DEBUG Found stale response for: https://pypi.org/simple/sympy/
DEBUG Sending revalidation request for: https://pypi.org/simple/sympy/
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/sympy/
DEBUG Found not-modified response for: https://pypi.org/simple/fsspec/
DEBUG Found not-modified response for: https://pypi.org/simple/jinja2/
DEBUG Found not-modified response for: https://pypi.org/simple/typing-extensions/
DEBUG Found not-modified response for: https://pypi.org/simple/filelock/
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.16.1 [compatible] (filelock-3.16.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/b2/454d6e7f0158951d8a78c2e1eb4f69ae81beb8dca5fee9809c6c99e9d0d0/fsspec-2024.10.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/f8/feced7779d755758a52d1f6635d990b8d98dc0a29fa568bbe0625f18fdf3/filelock-3.16.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=4.8.0)
DEBUG Found not-modified response for: https://pypi.org/simple/networkx/
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.4.2 [compatible] (networkx-3.4.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.4 [compatible] (jinja2-3.1.4-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.4: markupsafe>=2.0
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Selecting: fsspec==2024.10.0 [compatible] (fsspec-2024.10.0-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools{python_full_version >= '3.12'} (*)
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools==75.6.0: setuptools==75.6.0
DEBUG Adding transitive dependency for setuptools==75.6.0: setuptools{python_full_version >= '3.12'}==75.6.0
DEBUG Searching for a compatible version of setuptools{python_full_version >= '3.12'} (==75.6.0)
DEBUG Found stale response for: https://pypi.org/simple/markupsafe/
DEBUG Sending revalidation request for: https://pypi.org/simple/markupsafe/
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/21/47d163f615df1d30c094f6c8bbb353619274edccf0327b185cc2493c2c33/setuptools-75.6.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools (==75.6.0)
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (>=1.13.1, <1.13.1+)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: sympy==1.13.1
DEBUG Adding transitive dependency for sympy==1.13.1: sympy{python_full_version >= '3.9'}==1.13.1
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b2/fe/81695a1aa331a842b582453b605175f419fe8540355886031328089d840a/sympy-1.13.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of sympy (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/mpmath/
DEBUG Sending revalidation request for: https://pypi.org/simple/mpmath/
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Found not-modified response for: https://pypi.org/simple/markupsafe/
DEBUG Found not-modified response for: https://pypi.org/simple/mpmath/
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [compatible] (MarkupSafe-3.0.2-cp312-cp312-win_amd64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/80/a61f99dc3a936413c3ee4e1eecac96c0da5ed07ad56fd975f1a9da5bc630/MarkupSafe-3.0.2-cp312-cp312-win_amd64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/e3/7d92a15f894aa0c9c4b49b8ee9ac9850d6e63b03c9c32c0367a13ae62209/mpmath-1.3.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of mpmath (>=1.1.0, <1.4)
DEBUG Selecting: mpmath==1.3.0 [compatible] (mpmath-1.3.0-py3-none-any.whl)
DEBUG Tried 12 versions: filelock 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, networkx 1, numpy 1, pip 1, setuptools 1, sympy 1, torch 1, typing-extensions 1
DEBUG marker environment resolution took 0.606s
Resolved 12 packages in 607ms
DEBUG Requirement already cached: mpmath==1.3.0
DEBUG Requirement already cached: sympy==1.13.1
DEBUG Requirement already cached: setuptools==75.6.0
DEBUG Requirement already cached: torch==2.5.1+xpu
DEBUG Requirement already cached: jinja2==3.1.4
DEBUG Requirement already cached: pip==24.3.1
DEBUG Requirement already cached: filelock==3.16.1
DEBUG Requirement already cached: networkx==3.4.2
DEBUG Requirement already cached: numpy==2.1.3
DEBUG Requirement already cached: markupsafe==3.0.2
DEBUG Requirement already cached: fsspec==2024.10.0
DEBUG Requirement already cached: typing-extensions==4.12.2
Installed 12 packages in 971ms
 + filelock==3.16.1
 + fsspec==2024.10.0
 + jinja2==3.1.4
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + numpy==2.1.3
 + pip==24.3.1
 + setuptools==75.6.0
 + sympy==1.13.1
 + torch==2.5.1+xpu
 + typing-extensions==4.12.2
DEBUG Released lock at `D:\torch-test-xpu\.venv\.lock`
```

</p>
</details> 

### Plan 2

With `explicit = true` and running `uv sync -v`, it fails.

It seems the reason is that `pytorch-triton-xpu` only supports Linux, but it is not required on Windows. Why does uv not skip it?

<details><summary>Details</summary>
<p>

```
(torch-test-xpu) D:\torch-test-xpu>uv sync -v
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG Found project root: `D:\torch-test-xpu`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `D:\torch-test-xpu\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: torch-test-xpu @ file:///D:/torch-test-xpu
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: torch-test-xpu*
DEBUG Searching for a compatible version of torch-test-xpu @ file:///D:/torch-test-xpu (*)
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: numpy>=1.26.3
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: pip>=24.3.1
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: torch>=2.5.0
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/torch/
DEBUG Found fresh response for: https://pypi.org/simple/pip/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ef/7d/500c9ad20238fcfcb4cb9243eede163594d7020ce87bd9610c9e02771876/pip-24.3.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (>=1.26.3)
DEBUG Selecting: numpy==2.1.3 [compatible] (numpy-2.1.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8a/f0/385eb9970309643cbca4fc6eebc8bb16e560de129c91258dfaa18498da8b/numpy-2.1.3-cp312-cp312-macosx_10_13_x86_64.whl.metadata
DEBUG Searching for a compatible version of pip (>=24.3.1)
DEBUG Selecting: pip==24.3.1 [compatible] (pip-24.3.1-py3-none-any.whl)
DEBUG Searching for a compatible version of torch (>=2.5.0)
DEBUG Selecting: torch==2.5.1+xpu [compatible] (torch-2.5.1+xpu-cp312-cp312-linux_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Adding transitive dependency for torch==2.5.1+xpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}>=3.1.0, <3.1.0+
DEBUG Adding transitive dependency for torch==2.5.1+xpu: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+        
DEBUG Adding transitive dependency for torch==2.5.1+xpu: typing-extensions>=4.8.0
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.16.1 [compatible] (filelock-3.16.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/fsspec/
DEBUG No cache entry for: https://pypi.org/simple/pytorch-triton-xpu/
DEBUG Found fresh response for: https://pypi.org/simple/networkx/
DEBUG Found fresh response for: https://pypi.org/simple/sympy/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/b2/454d6e7f0158951d8a78c2e1eb4f69ae81beb8dca5fee9809c6c99e9d0d0/fsspec-2024.10.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/f8/feced7779d755758a52d1f6635d990b8d98dc0a29fa568bbe0625f18fdf3/filelock-3.16.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/54/dd730b32ea14ea797530a4479b2ed46a6fb250f682a9cfb997e968bf0261/networkx-3.4.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Selecting: fsspec==2024.10.0 [compatible] (fsspec-2024.10.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.4 [compatible] (jinja2-3.1.4-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.4: markupsafe>=2.0
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.4.2 [compatible] (networkx-3.4.2-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/markupsafe/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/22/09/d1f21434c97fc42f09d290cbb6350d44eb12f09cc62c9476effdb33a18aa/MarkupSafe-3.0.2-cp312-cp312-macosx_10_13_universal2.whl.metadata
DEBUG No netrc file found
DEBUG Searching for a compatible version of pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'} (>=3.1.0, <3.1.0+)
DEBUG No compatible version found for: pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}
DEBUG Searching for a compatible version of numpy (>=1.26.3)
DEBUG Selecting: numpy==2.1.3 [compatible] (numpy-2.1.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of pip (>=24.3.1)
DEBUG Selecting: pip==24.3.1 [compatible] (pip-24.3.1-py3-none-any.whl)
DEBUG Searching for a compatible version of torch (>=2.5.0, <2.5.1+xpu | >2.5.1+xpu)
DEBUG Selecting: torch==2.5.0+xpu [compatible] (torch-2.5.0+xpu-cp312-cp312-linux_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.0%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/xpu/torch-2.5.0%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.0%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Adding transitive dependency for torch==2.5.0+xpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.0+xpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.0+xpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.0+xpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.0+xpu: pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}>=3.1.0, <3.1.0+
DEBUG Adding transitive dependency for torch==2.5.0+xpu: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.5.0+xpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+        
DEBUG Adding transitive dependency for torch==2.5.0+xpu: typing-extensions>=4.8.0
DEBUG Searching for a compatible version of numpy (>=1.26.3)
DEBUG Selecting: numpy==2.1.3 [compatible] (numpy-2.1.3-cp312-cp312-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of pip (>=24.3.1)
DEBUG Selecting: pip==24.3.1 [compatible] (pip-24.3.1-py3-none-any.whl)
DEBUG Searching for a compatible version of torch (>=2.5.0, <2.5.0+xpu | >2.5.0+xpu, <2.5.1+xpu | >2.5.1+xpu)
DEBUG No compatible version found for: torch
DEBUG Searching for a compatible version of torch-test-xpu @ file:///D:/torch-test-xpu (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: torch-test-xpu
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of pytorch-triton-xpu{python_full_version < '3.13' and
      platform_machine == 'x86_64' and platform_system == 'Linux'}==3.1.0 and torch>=2.5.0+xpu depends on
      pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system ==
      'Linux'}==3.1.0, we can conclude that torch>=2.5.0+xpu cannot be used.
      And because only the following versions of torch are available:
          torch<2.5.0
          torch==2.5.0+xpu
          torch==2.5.1+xpu
      and your project depends on torch>=2.5.0, we can conclude that your project's requirements are unsatisfiable. 
```
</p>
</details> 

### Plan 3

Removing `explicit = true` and trying `uv sync -v`, it still fails.

It should fall back to `markupsafe` 2.1.5 like `uv pip install` without `explicit = true` does. `markupsafe` 3.0.2 has a Windows wheel on the PyPI index but only has a Linux wheel on the XPU index.

If I manually change the version of `markupsafe` to 2.1.5 in the lock file, then `uv sync` works without errors.

<https://github.com/Stanley5249/torch-test-xpu/commit/50a24098229cc3da11aae8248aac962398aab5f6>

<details><summary>Details</summary>
<p>

```
(torch-test-xpu) D:\torch-test-xpu>uv sync -v
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG Found project root: `D:\torch-test-xpu`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `D:\torch-test-xpu\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: torch-test-xpu @ file:///D:/torch-test-xpu
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: torch-test-xpu*
DEBUG Searching for a compatible version of torch-test-xpu @ file:///D:/torch-test-xpu (*)
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: numpy>=1.26.3
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: pip>=24.3.1
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: torch>=2.5.0
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/numpy/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/pip/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/torch/
DEBUG No netrc file found
DEBUG Found fresh response for: https://pypi.org/simple/pip/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ef/7d/500c9ad20238fcfcb4cb9243eede163594d7020ce87bd9610c9e02771876/pip-24.3.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of numpy (>=1.26.3)
DEBUG Selecting: numpy==1.26.3 [compatible] (numpy-1.26.3-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/numpy-1.26.3-cp312-cp312-macosx_10_9_x86_64.whl 
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/numpy-1.26.3-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/numpy-1.26.3-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Searching for a compatible version of pip (>=24.3.1)
DEBUG Selecting: pip==24.3.1 [compatible] (pip-24.3.1-py3-none-any.whl)
DEBUG Searching for a compatible version of torch (>=2.5.0)
DEBUG Selecting: torch==2.5.1+xpu [compatible] (torch-2.5.1+xpu-cp312-cp312-linux_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Adding transitive dependency for torch==2.5.1+xpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}>=3.1.0, <3.1.0+
DEBUG Adding transitive dependency for torch==2.5.1+xpu: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+        
DEBUG Adding transitive dependency for torch==2.5.1+xpu: typing-extensions>=4.8.0
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/filelock/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/fsspec/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/jinja2/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/networkx/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/pytorch-triton-xpu/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/setuptools/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/sympy/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/typing-extensions/
DEBUG Found stale response for: https://download.pytorch.org/whl/test/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Found stale response for: https://download.pytorch.org/whl/test/fsspec-2024.6.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/fsspec-2024.6.1-py3-none-any.whl        
DEBUG Found stale response for: https://download.pytorch.org/whl/test/networkx-3.3-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/networkx-3.3-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/fsspec-2024.6.1-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/networkx-3.3-py3-none-any.whl
DEBUG Found stale response for: https://download.pytorch.org/whl/test/Jinja2-3.1.4-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/Jinja2-3.1.4-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/Jinja2-3.1.4-py3-none-any.whl
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.13.1 [compatible] (filelock-3.13.1-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/filelock-3.13.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/filelock-3.13.1-py3-none-any.whl        
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/filelock-3.13.1-py3-none-any.whl
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Selecting: fsspec==2024.6.1 [compatible] (fsspec-2024.6.1-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.4 [compatible] (Jinja2-3.1.4-py3-none-any.whl)
DEBUG Adding transitive dependency for jinja2==3.1.4: markupsafe>=2.0
DEBUG Searching for a compatible version of networkx (*)
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/markupsafe/
DEBUG Selecting: networkx==3.3 [compatible] (networkx-3.3-py3-none-any.whl)
DEBUG Searching for a compatible version of pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'} (>=3.1.0, <3.1.0+)
DEBUG Selecting: pytorch-triton-xpu==3.1.0 [compatible] (pytorch_triton_xpu-3.1.0-cp312-cp312-linux_x86_64.whl)       
DEBUG Adding transitive dependency for pytorch-triton-xpu==3.1.0: pytorch-triton-xpu==3.1.0
DEBUG Adding transitive dependency for pytorch-triton-xpu==3.1.0: pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}==3.1.0
DEBUG Searching for a compatible version of pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'} (==3.1.0)
DEBUG Selecting: pytorch-triton-xpu==3.1.0 [compatible] (pytorch_triton_xpu-3.1.0-cp312-cp312-linux_x86_64.whl)       
DEBUG Found stale response for: https://download.pytorch.org/whl/test/pytorch_triton_xpu-3.1.0-cp312-cp312-linux_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/pytorch_triton_xpu-3.1.0-cp312-cp312-linux_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/pytorch_triton_xpu-3.1.0-cp312-cp312-linux_x86_64.whl
DEBUG Adding transitive dependency for pytorch-triton-xpu==3.1.0: packaging*
DEBUG Searching for a compatible version of pytorch-triton-xpu (==3.1.0)
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/packaging/
DEBUG Selecting: pytorch-triton-xpu==3.1.0 [compatible] (pytorch_triton_xpu-3.1.0-cp312-cp312-linux_x86_64.whl)       
DEBUG Adding transitive dependency for pytorch-triton-xpu==3.1.0: packaging*
DEBUG Searching for a compatible version of setuptools{python_full_version >= '3.12'} (*)
DEBUG Selecting: setuptools==70.0.0 [compatible] (setuptools-70.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools==70.0.0: setuptools==70.0.0
DEBUG Adding transitive dependency for setuptools==70.0.0: setuptools{python_full_version >= '3.12'}==70.0.0
DEBUG Searching for a compatible version of setuptools{python_full_version >= '3.12'} (==70.0.0)
DEBUG Selecting: setuptools==70.0.0 [compatible] (setuptools-70.0.0-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/setuptools-70.0.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/setuptools-70.0.0-py3-none-any.whl      
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/setuptools-70.0.0-py3-none-any.whl       
DEBUG Searching for a compatible version of setuptools (==70.0.0)
DEBUG Selecting: setuptools==70.0.0 [compatible] (setuptools-70.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (>=1.13.1, <1.13.1+)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: sympy==1.13.1
DEBUG Adding transitive dependency for sympy==1.13.1: sympy{python_full_version >= '3.9'}==1.13.1
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/sympy-1.13.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/sympy-1.13.1-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/sympy-1.13.1-py3-none-any.whl
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of sympy (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/mpmath/
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of typing-extensions (>=4.8.0)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [compatible] (MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Found stale response for: https://download.pytorch.org/whl/test/mpmath-1.3.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/mpmath-1.3.0-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/mpmath-1.3.0-py3-none-any.whl
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==22.0 [compatible] (packaging-22.0-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/packaging-22.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/packaging-22.0-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/packaging-22.0-py3-none-any.whl
DEBUG Searching for a compatible version of mpmath (>=1.1.0, <1.4)
DEBUG Selecting: mpmath==1.3.0 [compatible] (mpmath-1.3.0-py3-none-any.whl)
DEBUG Tried 15 versions: filelock 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, networkx 1, numpy 1, packaging 1, pip 1, pytorch-triton-xpu 1, setuptools 1, sympy 1, torch 1, torch-test-xpu 1, typing-extensions 1
DEBUG all marker environments resolution took 2.377s
Resolved 15 packages in 2.37s
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/test/xpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```
</p>
</details> 

## Question
I am not sure what causes the error, but I am certain that `torch>=2.5.0` supports the Windows platform as the PyTorch documentation states. The error only occurs with uv. Can you help me identify and resolve the issue? Thanks in advance!


---

_Comment by @FishAlchemist on 2024-11-25 09:53_

Actually, the dependency of  ``uv sync`` project series  find cross-platform compatible packages by default.

 I think adding the following constraint will make the packages installed by ``uv sync`` similar to those installed by ``uv pip install``.
```
[tool.uv]
environments = [
    "platform_system == 'Windows'",
]
```
![image](https://github.com/user-attachments/assets/e8fc16b2-91f4-403f-972f-0bf28c92d816)


As for writing a cross-platform compatible pyproject.toml file, I don't have the time to help with that right now.

---

_Label `question` added by @charliermarsh on 2024-11-25 13:48_

---

_Comment by @zanieb on 2024-11-25 23:05_

Did you read the PyTorch guide? https://docs.astral.sh/uv/guides/integration/pytorch/

---

_Comment by @Stanley5249 on 2024-11-26 02:45_

@FishAlchemist, @zanieb

Yes, I read the guide, but it did not help. Neither adding a marker in the source nor setting `explicit = true` worked. However, setting `platform_system == 'Windows` helps when `explicit = true` . Thanks.

```toml
# pyproject.toml

[project]
name = "torch-test-xpu"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["torch>=2.5.0"]

[tool.uv]
environments = ["platform_system == 'Windows'"]

[tool.uv.sources]
torch = [{ index = "pytorch-test-xpu" }]

[[tool.uv.index]]
name = "pytorch-test-xpu"
url = "https://download.pytorch.org/whl/test/xpu"
explicit = true
``` 

```
# This file was autogenerated by uv via the following command:
#    uv export --no-hashes
filelock==3.16.1 ; platform_system == 'Windows'
fsspec==2024.10.0 ; platform_system == 'Windows'
jinja2==3.1.4 ; platform_system == 'Windows'
markupsafe==3.0.2 ; platform_system == 'Windows'
mpmath==1.3.0 ; platform_system == 'Windows'
networkx==3.4.2 ; platform_system == 'Windows'
setuptools==75.6.0 ; platform_system == 'Windows'
sympy==1.13.1 ; platform_system == 'Windows'
torch==2.5.1+xpu ; platform_system == 'Windows'
typing-extensions==4.12.2 ; platform_system == 'Windows'
``` 

---

⬇️ But I still have some questions. These versions are from the XPU index, so I assume you did not set `explicit = true`.

> ![image](https://github.com/user-attachments/assets/ec470b0f-e7ba-471c-8ab9-11fe98ff3413)

⬇️ And this still happens, so I cannot install the exact versions like yours.

> ### Plan 3
> Removing `explicit = true` and trying `uv sync -v`, it still fails.
> 
> It should fall back to `markupsafe` 2.1.5 like `uv pip install` without `explicit = true` does. `markupsafe` 3.0.2 has a Windows wheel on the PyPI index but only has a Linux wheel on the XPU index.

<details><summary>Details</summary>
<p>

```
DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG Found project root: `D:\torch-test-xpu`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `D:\torch-test-xpu\.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to `--upgrade`
DEBUG Found static `pyproject.toml` for: torch-test-xpu @ file:///D:/torch-test-xpu
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12
DEBUG Solving split (platform_system == 'Windows') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.12" }]), range: RequiresPythonRange(LowerBound(Included("3.12")), UpperBound(Unbounded)) })
DEBUG Adding direct dependency: torch-test-xpu*
DEBUG Searching for a compatible version of torch-test-xpu @ file:///D:/torch-test-xpu (*)
DEBUG Adding transitive dependency for torch-test-xpu==0.1.0: torch>=2.5.0
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/torch/
DEBUG Searching for a compatible version of torch (>=2.5.0)
DEBUG Selecting: torch==2.5.1+xpu [compatible] (torch-2.5.1+xpu-cp312-cp312-linux_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/xpu/torch-2.5.1%2Bxpu-cp312-cp312-linux_x86_64.whl
DEBUG Adding transitive dependency for torch==2.5.1+xpu: filelock*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: fsspec*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: jinja2*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: networkx*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.5.1+xpu: sympy{python_full_version >= '3.9'}>=1.13.1, <1.13.1+        
DEBUG Adding transitive dependency for torch==2.5.1+xpu: typing-extensions>=4.8.0
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/filelock/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/fsspec/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/jinja2/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/networkx/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/setuptools/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/sympy/
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/typing-extensions/
DEBUG Searching for a compatible version of filelock (*)
DEBUG Selecting: filelock==3.13.1 [compatible] (filelock-3.13.1-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/filelock-3.13.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/filelock-3.13.1-py3-none-any.whl        
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/filelock-3.13.1-py3-none-any.whl
DEBUG Found stale response for: https://download.pytorch.org/whl/test/networkx-3.3-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/networkx-3.3-py3-none-any.whl
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/networkx-3.3-py3-none-any.whl
DEBUG Selecting: fsspec==2024.6.1 [compatible] (fsspec-2024.6.1-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/fsspec-2024.6.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/fsspec-2024.6.1-py3-none-any.whl        
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/fsspec-2024.6.1-py3-none-any.whl
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Selecting: jinja2==3.1.4 [compatible] (Jinja2-3.1.4-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/Jinja2-3.1.4-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/Jinja2-3.1.4-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/Jinja2-3.1.4-py3-none-any.whl
DEBUG Adding transitive dependency for jinja2==3.1.4: markupsafe>=2.0
DEBUG Searching for a compatible version of networkx (*)
DEBUG Selecting: networkx==3.3 [compatible] (networkx-3.3-py3-none-any.whl)
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/markupsafe/
DEBUG Searching for a compatible version of setuptools{python_full_version >= '3.12'} (*)
DEBUG Selecting: setuptools==70.0.0 [compatible] (setuptools-70.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools==70.0.0: setuptools==70.0.0
DEBUG Adding transitive dependency for setuptools==70.0.0: setuptools{python_full_version >= '3.12'}==70.0.0
DEBUG Searching for a compatible version of setuptools{python_full_version >= '3.12'} (==70.0.0)
DEBUG Selecting: setuptools==70.0.0 [compatible] (setuptools-70.0.0-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/setuptools-70.0.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/setuptools-70.0.0-py3-none-any.whl      
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/setuptools-70.0.0-py3-none-any.whl       
DEBUG Searching for a compatible version of setuptools (==70.0.0)
DEBUG Selecting: setuptools==70.0.0 [compatible] (setuptools-70.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (>=1.13.1, <1.13.1+)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Adding transitive dependency for sympy==1.13.1: sympy==1.13.1
DEBUG Adding transitive dependency for sympy==1.13.1: sympy{python_full_version >= '3.9'}==1.13.1
DEBUG Searching for a compatible version of sympy{python_full_version >= '3.9'} (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/sympy-1.13.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/sympy-1.13.1-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/sympy-1.13.1-py3-none-any.whl
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of sympy (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG No cache entry for: https://download.pytorch.org/whl/test/xpu/mpmath/
DEBUG Adding transitive dependency for sympy==1.13.1: mpmath>=1.1.0, <1.4
DEBUG Searching for a compatible version of typing-extensions (>=4.8.0)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Selecting: markupsafe==3.0.2 [compatible] (MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG Searching for a compatible version of mpmath (>=1.1.0, <1.4)
DEBUG Selecting: mpmath==1.3.0 [compatible] (mpmath-1.3.0-py3-none-any.whl)
DEBUG Found stale response for: https://download.pytorch.org/whl/test/mpmath-1.3.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://download.pytorch.org/whl/test/mpmath-1.3.0-py3-none-any.whl
DEBUG Found not-modified response for: https://download.pytorch.org/whl/test/mpmath-1.3.0-py3-none-any.whl
DEBUG Tried 11 versions: filelock 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, networkx 1, setuptools 1, sympy 1, torch 1, torch-test-xpu 1, typing-extensions 1
DEBUG split `platform_system == 'Windows'` resolution took 2.350s
DEBUG Distinct solution for split (platform_system == 'Windows') with 11 packages
Resolved 11 packages in 2.35s
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/test/xpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
``` 
</p>
</details> 

---

To sum up, it works fine with setting `platform_system == 'Windows` and `explicit = true`, but it does not work when removing `explicit = true`. I know the guide suggests using `explicit = true`.

For the `uv sync` command, I understand it generates a lock file for cross-platform compatible packages (I didn't know before). But I think it's strange not to install packages for your platform like `uv pip install -r pyproject.toml` does.


---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-07 19:56_

---

_Comment by @charliermarsh on 2025-01-07 21:03_

If you remove `environments = ["platform_system == 'Windows'"]`, it fails with:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of pytorch-triton-xpu{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}==3.1.0 and torch>=2.5.0+xpu,<=2.5.1+xpu depends on pytorch-triton-xpu{python_full_version
      < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}==3.1.0, we can conclude that torch>=2.5.0+xpu,<=2.5.1+xpu cannot be used.
      And because only the following versions of torch are available:
          torch<2.5.0
          torch==2.5.0+xpu
          torch==2.5.1+xpu
          torch>2.6.0
      and your project depends on torch>=2.5.0,<2.6.0, we can conclude that your project's requirements are unsatisfiable.
```

That seems correct to me, because you have `explicit = true`, so it can't find `pytorch-triton-xpu` anywhere.

When `environments = ["platform_system == 'Windows'"]` is included, we never have to solve for Linux, which is where `pytorch-triton-xpu` gets included.

For reference, this also succeeds:

```toml
[project]
name = "torch-test-xpu"
version = "0.1.0"
description = "Testing PyTorch functionality with Intel GPUs (XPU)"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["numpy>=1.26.3", "pip>=24.3.1", "torch>=2.5.0,<2.6.0", "pytorch-triton-xpu"]

[tool.uv.sources]
torch = [{ index = "pytorch-test-xpu" }]
pytorch-triton-xpu = [{ index = "pytorch-test-xpu" }]

[[tool.uv.index]]
name = "pytorch-test-xpu"
url = "https://download.pytorch.org/whl/test/xpu"
explicit = true
```


---

_Closed by @charliermarsh on 2025-01-07 21:03_

---
