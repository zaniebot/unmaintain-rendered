```yaml
number: 6712
title: "`0.3.5` regression: not respecting changes to `pyproject.toml`?"
type: issue
state: closed
author: ringohoffman
labels: []
assignees: []
created_at: 2024-08-27T18:38:32Z
updated_at: 2024-08-27T19:23:28Z
url: https://github.com/astral-sh/uv/issues/6712
synced_at: 2026-01-12T15:59:06Z
```

# `0.3.5` regression: not respecting changes to `pyproject.toml`?

---

_@ringohoffman_

I provided an MRE repo with instructions here: https://github.com/ringohoffman/uv-0.3.5-regression

The setup is start with unsatisfiable dependencies:

```toml
dependencies = [
    "torch==2.3.1",  # compatible only with torchvision~=0.18.1
    "torchvision==0.19.0",
]
```

Create an environment and try to install the package with unsatisfiable dependencies:

```console
$ conda create -n regression python=3.10 -y
$ conda activate regression
$ pip install uv==0.3.5
$ uv pip install -e .
  × No solution found when resolving dependencies:
  ╰─▶ Because torchvision==0.19.0 depends on torch==2.4.0 and uv-regression==0.0.0 depends on torch==2.3.1, we can conclude that torchvision==0.19.0 and uv-regression==0.0.0
      are incompatible.
      And because uv-regression==0.0.0 depends on torchvision==0.19.0, we can conclude that uv-regression==0.0.0 cannot be used.
      And because only uv-regression==0.0.0 is available and you require uv-regression, we can conclude that your requirements are unsatisfiable.
```

Solve the unsatisfiability by downgrading `torchvision` to `0.18.1`.

```console
$ uv pip install -e .
  × No solution found when resolving dependencies:
  ╰─▶ Because torchvision==0.19.0 depends on torch==2.4.0 and uv-regression==0.0.0 depends on torch==2.3.1, we can conclude that torchvision==0.19.0 and uv-regression==0.0.0
      are incompatible.
      And because uv-regression==0.0.0 depends on torchvision==0.19.0, we can conclude that uv-regression==0.0.0 cannot be used.
      And because only uv-regression==0.0.0 is available and you require uv-regression, we can conclude that your requirements are unsatisfiable.
```

The same failure, despite the change to `pyproject.toml`. Try downgrading `uv` to `0.3.4`.

```console
$ pip install uv==0.3.4
$ uv pip install -e .
Resolved 13 packages in 866ms
   Built uv-regression @ file:///Users/ringo/Repositories/uv-0.3.5-regression
Prepared 1 package in 588ms
Installed 13 packages in 508ms
 + filelock==3.15.4
 + fsspec==2024.6.1
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.3
 + numpy==2.1.0
 + pillow==10.4.0
 + sympy==1.13.2
 + torch==2.3.1
 + torchvision==0.18.1
 + typing-extensions==4.12.2
 + uv-regression==0.0.0 (from file:///Users/ringo/Repositories/uv-0.3.5-regression)
```


---

_Comment by @zanieb on 2024-08-27 18:48_

Thanks for the report and the MRE. Can you include debug logs for the failing case, e.g. `RUST_LOG=uv=trace uv pip install -e . -v`?

---

_Comment by @zanieb on 2024-08-27 18:50_

Here's my reproduction

```
❯ RUST_LOG=uv=trace uv pip install -e . -v
DEBUG uv 0.3.5
DEBUG Searching for Python interpreter in system path
TRACE Cached interpreter info for Python 3.10.13, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.10.13-macos-aarch64-none` at `/Users/zb/workspace/uv-0.3.5-regression/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.10.13 environment at .venv/bin/python3
TRACE Checking lock for `.venv`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///Users/zb/workspace/uv-0.3.5-regression
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /Users/zb/workspace/uv-0.3.5-regression in `pyproject.toml` (uv-regression)
TRACE Performing lookahead for uv-regression @ file:///Users/zb/workspace/uv-0.3.5-regression
TRACE Checking lock for `/Users/zb/.cache/uv/built-wheels-v3/editable/0f3addc20395d8c4`
DEBUG Acquired lock for `/Users/zb/.cache/uv/built-wheels-v3/editable/0f3addc20395d8c4`
DEBUG No static `pyproject.toml` available for: uv-regression @ file:///Users/zb/workspace/uv-0.3.5-regression (PyprojectToml(DynamicField("version")))
DEBUG No static `PKG-INFO` available for: uv-regression @ file:///Users/zb/workspace/uv-0.3.5-regression (MissingPkgInfo)
DEBUG Found static `egg-info` for: uv-regression @ file:///Users/zb/workspace/uv-0.3.5-regression
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.10.13
DEBUG Adding direct dependency: uv-regression*
DEBUG Searching for a compatible version of uv-regression @ file:///Users/zb/workspace/uv-0.3.5-regression (*)
DEBUG Adding transitive dependency for uv-regression==0.0.0: torch==2.3.1
DEBUG Adding transitive dependency for uv-regression==0.0.0: torchvision==0.19.0
TRACE Fetching metadata for torch from https://pypi.org/simple/torch/
TRACE Fetching metadata for torchvision from https://pypi.org/simple/torchvision/
TRACE cached request https://pypi.org/simple/torch/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/torch/
TRACE Received package metadata for: torch
TRACE cached request https://pypi.org/simple/torchvision/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/torchvision/
DEBUG Searching for a compatible version of torch (==2.3.1)
TRACE Selecting candidate for torch with range ==2.3.1 with 35 remote versions
TRACE found candidate for package PackageName("torch") with range Range { segments: [(Included("2.3.1"), Included("2.3.1"))] } after 2 steps: "2.3.1" version
DEBUG Selecting: torch==2.3.1 [compatible] (torch-2.3.1-cp310-none-macosx_11_0_arm64.whl)
TRACE Received package metadata for: torchvision
TRACE Selecting candidate for torch with range ==2.3.1 with 35 remote versions
TRACE found candidate for package PackageName("torch") with range Range { segments: [(Included("2.3.1"), Included("2.3.1"))] } after 2 steps: "2.3.1" version
TRACE Selecting candidate for torchvision with range ==0.19.0 with 46 remote versions
TRACE found candidate for package PackageName("torchvision") with range Range { segments: [(Included("0.19.0"), Included("0.19.0"))] } after 1 steps: "0.19.0" version
TRACE cached request https://files.pythonhosted.org/packages/66/11/9f5ce2d6cfb55bdb18bfae6f7604f9bdc41586bed2d90ca50412a689ecba/torchvision-0.19.0-cp310-cp310-macosx_11_0_arm64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/66/11/9f5ce2d6cfb55bdb18bfae6f7604f9bdc41586bed2d90ca50412a689ecba/torchvision-0.19.0-cp310-cp310-macosx_11_0_arm64.whl.metadata
TRACE Received built distribution metadata for: torchvision==0.19.0
TRACE cached request https://files.pythonhosted.org/packages/2c/52/7ab0a00b54aa1651e79a9ebc721d45fba86d8c8ab65c4ec6e0a49f09527a/torch-2.3.1-cp310-none-macosx_11_0_arm64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2c/52/7ab0a00b54aa1651e79a9ebc721d45fba86d8c8ab65c4ec6e0a49f09527a/torch-2.3.1-cp310-none-macosx_11_0_arm64.whl.metadata
TRACE Received built distribution metadata for: torch==2.3.1
DEBUG Adding transitive dependency for torch==2.3.1: filelock*
DEBUG Adding transitive dependency for torch==2.3.1: typing-extensions>=4.8.0
DEBUG Adding transitive dependency for torch==2.3.1: sympy*
DEBUG Adding transitive dependency for torch==2.3.1: networkx*
DEBUG Adding transitive dependency for torch==2.3.1: jinja2*
DEBUG Adding transitive dependency for torch==2.3.1: fsspec*
DEBUG Searching for a compatible version of torchvision (==0.19.0)
TRACE Selecting candidate for torchvision with range ==0.19.0 with 46 remote versions
TRACE Fetching metadata for filelock from https://pypi.org/simple/filelock/
TRACE found candidate for package PackageName("torchvision") with range Range { segments: [(Included("0.19.0"), Included("0.19.0"))] } after 1 steps: "0.19.0" version
DEBUG Selecting: torchvision==0.19.0 [compatible] (torchvision-0.19.0-cp310-cp310-macosx_11_0_arm64.whl)
TRACE Fetching metadata for typing-extensions from https://pypi.org/simple/typing-extensions/
TRACE Fetching metadata for sympy from https://pypi.org/simple/sympy/
DEBUG Adding transitive dependency for torchvision==0.19.0: numpy*
DEBUG Adding transitive dependency for torchvision==0.19.0: torch==2.4.0
DEBUG Adding transitive dependency for torchvision==0.19.0: pillow>=5.3.0, <8.3.dev0 | >=8.4.dev0
TRACE Fetching metadata for networkx from https://pypi.org/simple/networkx/
DEBUG Searching for a compatible version of uv-regression @ file:///Users/zb/workspace/uv-0.3.5-regression (<0.0.0 | >0.0.0)
TRACE Fetching metadata for jinja2 from https://pypi.org/simple/jinja2/
DEBUG No compatible version found for: uv-regression
TRACE Fetching metadata for fsspec from https://pypi.org/simple/fsspec/
TRACE cached request https://pypi.org/simple/filelock/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
TRACE Received package metadata for: filelock
TRACE cached request https://pypi.org/simple/typing-extensions/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
TRACE Received package metadata for: typing-extensions
TRACE cached request https://pypi.org/simple/sympy/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/sympy/
TRACE Received package metadata for: sympy
TRACE Fetching metadata for numpy from https://pypi.org/simple/numpy/
TRACE Fetching metadata for pillow from https://pypi.org/simple/pillow/
TRACE cached request https://pypi.org/simple/networkx/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/networkx/
TRACE Received package metadata for: networkx
TRACE cached request https://pypi.org/simple/fsspec/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/fsspec/
TRACE Received package metadata for: fsspec
TRACE Selecting candidate for filelock with range * with 72 remote versions
TRACE found candidate for package PackageName("filelock") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "3.15.4" version
TRACE cached request https://pypi.org/simple/jinja2/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
TRACE Received package metadata for: jinja2
TRACE Selecting candidate for typing-extensions with range >=4.8.0 with 40 remote versions
TRACE found candidate for package PackageName("typing-extensions") with range Range { segments: [(Included("4.8.0"), Unbounded)] } after 1 steps: "4.12.2" version
TRACE Selecting candidate for sympy with range * with 61 remote versions
TRACE found candidate for package PackageName("sympy") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "1.13.2" version
TRACE Selecting candidate for networkx with range * with 76 remote versions
TRACE found candidate for package PackageName("networkx") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "3.3" version
TRACE Selecting candidate for fsspec with range * with 82 remote versions
TRACE found candidate for package PackageName("fsspec") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "2024.6.1" version
TRACE Selecting candidate for jinja2 with range * with 50 remote versions
TRACE found candidate for package PackageName("jinja2") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "3.1.4" version
TRACE cached request https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: typing-extensions==4.12.2
TRACE cached request https://files.pythonhosted.org/packages/ae/f0/48285f0262fe47103a4a45972ed2f9b93e4c80b8fd609fa98da78b2a5706/filelock-3.15.4-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ae/f0/48285f0262fe47103a4a45972ed2f9b93e4c80b8fd609fa98da78b2a5706/filelock-3.15.4-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: filelock==3.15.4
TRACE cached request https://files.pythonhosted.org/packages/38/e9/5f72929373e1a0e8d142a130f3f97e6ff920070f87f91c4e13e40e0fba5a/networkx-3.3-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/38/e9/5f72929373e1a0e8d142a130f3f97e6ff920070f87f91c4e13e40e0fba5a/networkx-3.3-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: networkx==3.3
TRACE cached request https://files.pythonhosted.org/packages/c1/f9/6845bf8fca0eaf847da21c5d5bc6cd92797364662824a11d3f836423a1a5/sympy-1.13.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/f9/6845bf8fca0eaf847da21c5d5bc6cd92797364662824a11d3f836423a1a5/sympy-1.13.2-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: sympy==1.13.2
TRACE cached request https://files.pythonhosted.org/packages/5e/44/73bea497ac69bafde2ee4269292fa3b41f1198f4bb7bbaaabde30ad29d4a/fsspec-2024.6.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5e/44/73bea497ac69bafde2ee4269292fa3b41f1198f4bb7bbaaabde30ad29d4a/fsspec-2024.6.1-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: fsspec==2024.6.1
TRACE cached request https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: jinja2==3.1.4
TRACE cached request https://pypi.org/simple/pillow/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/pillow/
TRACE Received package metadata for: pillow
TRACE cached request https://pypi.org/simple/numpy/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
TRACE Received package metadata for: numpy
TRACE Resolver derivation tree before reduction
  root==0a0.dev0 depends on uv-regression*
      uv-regression==0.0.0 depends on torchvision==0.19.0
        uv-regression==0.0.0 depends on torch==2.3.1
        torchvision==0.19.0 depends on torch==2.4.0
    no versions of uv-regression<0.0.0 | >0.0.0
TRACE Resolver derivation tree after reduction
  root==0a0.dev0 depends on uv-regression*
      uv-regression==0.0.0 depends on torchvision==0.19.0
        uv-regression==0.0.0 depends on torch==2.3.1
        torchvision==0.19.0 depends on torch==2.4.0
    no versions of uv-regression<0.0.0 | >0.0.0
  × No solution found when resolving dependencies:
  ╰─▶ Because torchvision==0.19.0 depends on torch==2.4.0 and uv-regression==0.0.0 depends on torch==2.3.1, we can conclude that torchvision==0.19.0 and uv-regression==0.0.0 are incompatible.
      And because uv-regression==0.0.0 depends on torchvision==0.19.0, we can conclude that uv-regression==0.0.0 cannot be used.
      And because only uv-regression==0.0.0 is available and you require uv-regression, we can conclude that your requirements are unsatisfiable.
```

---

_Comment by @charliermarsh on 2024-08-27 18:51_

Thank you, will look at this ASAP.

---

_Comment by @zanieb on 2024-08-27 18:52_

Interestingly, including `--no-cache` does not resolve the problem.

---

_Comment by @zanieb on 2024-08-27 18:54_

`uv pip compile` isn't affected

```
❯ uv pip compile pyproject.toml
  × No solution found when resolving dependencies:
  ╰─▶ Because torchvision==0.19.0 depends on torch==2.4.0 and uv-regression depends on torch==2.3.1, we can conclude that your requirements and torchvision==0.19.0 are incompatible.
      And because uv-regression depends on torchvision==0.19.0, we can conclude that your requirements are unsatisfiable.
❯ vi pyproject.toml
❯ uv pip compile pyproject.toml
Resolved 12 packages in 9ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml
filelock==3.15.4
    # via torch
fsspec==2024.6.1
    # via torch
jinja2==3.1.4
    # via torch
markupsafe==2.1.5
    # via jinja2
mpmath==1.3.0
    # via sympy
networkx==3.3
    # via torch
numpy==2.1.0
    # via torchvision
pillow==10.4.0
    # via torchvision
sympy==1.13.2
    # via torch
torch==2.3.1
    # via
    #   uv-regression (pyproject.toml)
    #   torchvision
torchvision==0.18.1
    # via uv-regression (pyproject.toml)
typing-extensions==4.12.2
    # via torch
```

---

_Comment by @zanieb on 2024-08-27 18:57_

The old metadata is used even if I moved the project to another path 😭 

Tested on `main` and reproduced as well.

---

_Comment by @zanieb on 2024-08-27 19:01_

Reverted https://github.com/astral-sh/uv/pull/6655 and it's fixed

---

_Comment by @charliermarsh on 2024-08-27 19:06_

Apologies. You should be able to `rm -rf` any `.egg-info` directory in your project to clear it?

---

_Closed by @charliermarsh on 2024-08-27 19:23_

---

_Closed by @charliermarsh on 2024-08-27 19:23_

---
