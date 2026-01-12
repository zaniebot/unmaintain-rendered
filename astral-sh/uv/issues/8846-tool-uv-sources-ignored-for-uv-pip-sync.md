```yaml
number: 8846
title: "`tool.uv.sources` ignored for `uv pip sync`"
type: issue
state: open
author: nathanjmcdougall
labels: []
assignees: []
created_at: 2024-11-05T21:53:46Z
updated_at: 2024-11-05T22:00:50Z
url: https://github.com/astral-sh/uv/issues/8846
synced_at: 2026-01-12T15:59:36Z
```

# `tool.uv.sources` ignored for `uv pip sync`

---

_@nathanjmcdougall_

When using the following `pyproject.toml` file:

```TOML
[project]
name = "torchissue"
version = "0.1.0"
dependencies = ["torch"]

[tool.uv.sources]
torch = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
explicit = true
```

This sequence of commands fails to install `torch`:

```bash
uv venv
uv pip compile pyproject.toml --output-file requirements.txt
uv pip sync requirements.txt
```

Whereas `uv sync` succeeds. Since the `--no-sources` option for `uv pip sync` was not used, I would have expected a successful install.

Full debug trace with `uv pip sync requirements.txt --verbose`:
```bash
DEBUG uv 0.4.30 (61ed2a236 2024-11-04)
DEBUG Searching for default Python interpreter in system path or `py` launcher
DEBUG Found `cpython-3.12.4-windows-x86_64-none` at `C:\Users\namc\Repositories\torchissue\.venv\Scripts\python.exe` (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.4
DEBUG Solving with target Python version: >=3.12.4
DEBUG Adding direct dependency: filelock==3.16.1
DEBUG Adding direct dependency: fsspec==2024.10.0
DEBUG Adding direct dependency: jinja2==3.1.4
DEBUG Adding direct dependency: markupsafe==3.0.2
DEBUG Adding direct dependency: mpmath==1.3.0
DEBUG Adding direct dependency: networkx==3.4.2
DEBUG Adding direct dependency: setuptools==75.3.0
DEBUG Adding direct dependency: sympy==1.13.1
DEBUG Adding direct dependency: torch==2.5.1+cu121
DEBUG Adding direct dependency: typing-extensions==4.12.2
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
DEBUG Searching for a compatible version of filelock (==3.16.1)
DEBUG Found fresh response for: https://pypi.org/simple/mpmath/
DEBUG Selecting: filelock==3.16.1 [compatible] (filelock-3.16.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/sympy/
DEBUG Found fresh response for: https://pypi.org/simple/networkx/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://pypi.org/simple/markupsafe/
DEBUG Found fresh response for: https://pypi.org/simple/fsspec/
DEBUG Found fresh response for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of fsspec (==2024.10.0)
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Selecting: fsspec==2024.10.0 [compatible] (fsspec-2024.10.0-py3-none-any.whl)
DEBUG Searching for a compatible version of jinja2 (==3.1.4)
DEBUG Selecting: jinja2==3.1.4 [compatible] (jinja2-3.1.4-py3-none-any.whl)
DEBUG Searching for a compatible version of markupsafe (==3.0.2)
DEBUG Selecting: markupsafe==3.0.2 [compatible] (MarkupSafe-3.0.2-cp312-cp312-win_amd64.whl)
DEBUG Searching for a compatible version of mpmath (==1.3.0)
DEBUG Selecting: mpmath==1.3.0 [compatible] (mpmath-1.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of networkx (==3.4.2)
DEBUG Selecting: networkx==3.4.2 [compatible] (networkx-3.4.2-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools (==75.3.0)
DEBUG Selecting: setuptools==75.3.0 [compatible] (setuptools-75.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of sympy (==1.13.1)
DEBUG Selecting: sympy==1.13.1 [compatible] (sympy-1.13.1-py3-none-any.whl)
DEBUG Searching for a compatible version of torch (==2.5.1+cu121)
DEBUG No compatible version found for: torch
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.5.1+cu121 and you require torch==2.5.1+cu121, we can conclude that your requirements     
      are unsatisfiable.
DEBUG Released lock at `C:\Users\namc\Repositories\torchissue\.venv\.lock`
```




---

_Comment by @charliermarsh on 2024-11-05 21:55_

The `requirements.txt` doesn't contain the `tool.uv.sources` information. I don't think there's any way to make that work -- it just can't be expressed in the `requirements.txt` format.

---

_Renamed from "`uv.tool.sources` ignored for `uv pip sync`" to "`tool.uv.sources` ignored for `uv pip sync`" by @nathanjmcdougall on 2024-11-05 21:57_

---

_Comment by @nathanjmcdougall on 2024-11-05 21:57_

Ah - does that mean that the `uv pip sync` interface completely ignores `tool.uv.sources` information?
If so, I found this hard to find in the docs and the `--no-sources` flag being available threw me off.

My thinking is that it would still be possible for `uv pip sync` to use the sources information to supplement the requirements file being synced, although I haven't thought about it in detail.

---
