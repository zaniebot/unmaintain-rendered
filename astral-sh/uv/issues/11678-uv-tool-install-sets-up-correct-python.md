```yaml
number: 11678
title: "`uv tool install` sets up correct Python environment only on second installation"
type: issue
state: closed
author: trallnag
labels:
  - bug
assignees: []
created_at: 2025-02-20T20:16:53Z
updated_at: 2025-02-20T22:05:05Z
url: https://github.com/astral-sh/uv/issues/11678
synced_at: 2026-01-12T16:00:44Z
```

# `uv tool install` sets up correct Python environment only on second installation

---

_@trallnag_

### Summary

Installing a tool that requires a version higher than the system Python version leads to uv installing the tool into an environment with the system Python version.

The following steps describe how to reproduce the bug.

Start by going into a container:

```sh
docker run --rm -it ubuntu:22.04
```

We are now inside the container. Install `curl` and `python3`:

```sh
apt update
apt install -y curl python3
```

The system Python version:

```sh
> python3 --version
Python 3.10.12
```

Install latest uv:

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

Now it gets interesting. Install a tool. The tool requires Python >= 3.12:

```sh
uv tool install --verbose filter-pre-commit-hooks
```

Here are the logs:

```txt
DEBUG uv 0.6.2
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Acquired lock for `/root/.local/share/uv/tools`
DEBUG Checking for Python environment at `/root/.local/share/uv/tools/filter-pre-commit-hooks`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.12
DEBUG Solving with target Python version: >=3.10.12
DEBUG Adding direct dependency: filter-pre-commit-hooks*
DEBUG No cache entry for: https://pypi.org/simple/filter-pre-commit-hooks/
DEBUG Searching for a compatible version of filter-pre-commit-hooks (*)
DEBUG No compatible version found for: Python
DEBUG Recording unit propagation conflict of Python from incompatibility of (filter-pre-commit-hooks)
DEBUG Searching for a compatible version of filter-pre-commit-hooks (<2.0.4 | >2.0.4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filter-pre-commit-hooks)
DEBUG Searching for a compatible version of filter-pre-commit-hooks (<2.0.3 | >2.0.3, <2.0.4 | >2.0.4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filter-pre-commit-hooks)
DEBUG Searching for a compatible version of filter-pre-commit-hooks (<2.0.2 | >2.0.2, <2.0.3 | >2.0.3, <2.0.4 | >2.0.4)
DEBUG No compatible version found for: filter-pre-commit-hooks
DEBUG Refining interpreter with: Python >=3.12, <3.13
DEBUG Searching for Python >=3.12, <3.13 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `>=3.12, <3.13`
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/bin/python3` (search path)
DEBUG Skipping interpreter at `/bin/python3` from search path: does not satisfy request `>=3.12, <3.13`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/root/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
Downloading cpython-3.12.9-linux-x86_64-gnu (20.3MiB)
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250212/cpython-3.12.9%2B20250212-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.temp/.tmpdt3Vry
DEBUG Extracting cpython-3.12.9%2B20250212-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
 Downloaded cpython-3.12.9-linux-x86_64-gnu
DEBUG Moving /root/.local/share/uv/python/.temp/.tmpdt3Vry/python to /root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
DEBUG Re-resolving with Python 3.12.9 (`/root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12`)
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: filter-pre-commit-hooks*
DEBUG Searching for a compatible version of filter-pre-commit-hooks (*)
DEBUG Selecting: filter-pre-commit-hooks==2.0.4 [compatible] (filter_pre_commit_hooks-2.0.4-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/9e/f1/ae280ceba877a3bb424c56d5153b1426a0b3a4b38ad5e0336a774541ebbc/filter_pre_commit_hooks-2.0.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for filter-pre-commit-hooks==2.0.4: click>=8.1.8, <8.1.8+
DEBUG Adding transitive dependency for filter-pre-commit-hooks==2.0.4: pyyaml>=6.0.2, <6.0.2+
DEBUG No cache entry for: https://pypi.org/simple/click/
DEBUG No cache entry for: https://pypi.org/simple/pyyaml/
DEBUG Searching for a compatible version of click (>=8.1.8, <8.1.8+)
DEBUG Selecting: click==8.1.8 [compatible] (click-8.1.8-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.5.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.0.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.3.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.3.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py3.5.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py3.5.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win32-py3.4.exe
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/2b/614b4752f2e127db5cc206abc23a8c19678e92b23c3db30fc86ab731d3bd/PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of pyyaml (>=6.0.2, <6.0.2+)
DEBUG Selecting: pyyaml==6.0.2 [compatible] (PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Tried 3 versions: click 1, filter-pre-commit-hooks 1, pyyaml 1
DEBUG marker environment resolution took 0.178s
Resolved 3 packages in 178ms
DEBUG Creating environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
DEBUG Using base executable for virtual environment: /usr/bin/python3
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: click==8.1.8
DEBUG Identified uncached distribution: filter-pre-commit-hooks==2.0.4
DEBUG Identified uncached distribution: pyyaml==6.0.2
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b9/2b/614b4752f2e127db5cc206abc23a8c19678e92b23c3db30fc86ab731d3bd/PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/9e/f1/ae280ceba877a3bb424c56d5153b1426a0b3a4b38ad5e0336a774541ebbc/filter_pre_commit_hooks-2.0.4-py3-none-any.whl
Prepared 3 packages in 142ms
Installed 3 packages in 32ms
 + click==8.1.8
 + filter-pre-commit-hooks==2.0.4
 + pyyaml==6.0.2
DEBUG Installing tool executables into: /root/.local/bin
DEBUG Looking at `.dist-info` at: /root/.local/share/uv/tools/filter-pre-commit-hooks/lib/python3.10/site-packages/filter_pre_commit_hooks-2.0.4.dist-info
DEBUG Installing executable: `filter-pre-commit-hooks`
Installed 1 executable: filter-pre-commit-hooks
DEBUG Adding receipt for tool `filter-pre-commit-hooks`
DEBUG Adding metadata entry for tool `filter-pre-commit-hooks` at /root/.local/share/uv/tools/filter-pre-commit-hooks/uv-receipt.toml
DEBUG Released lock at `/root/.local/share/uv/tools/.lock`
```

Uv correctly detects the necessary Python version but at the end it still decides to set up the tool in Python 3.10 for some reason.

```
filter-pre-commit-hooks --version
Traceback (most recent call last):
  File "/root/.local/bin/filter-pre-commit-hooks", line 4, in <module>
    from filter_pre_commit_hooks.filter_pre_commit_hooks import filter_pre_commit_hooks
  File "/root/.local/share/uv/tools/filter-pre-commit-hooks/lib/python3.10/site-packages/filter_pre_commit_hooks/filter_pre_commit_hooks.py", line 34, in <module>
    from enum import StrEnum
ImportError: cannot import name 'StrEnum' from 'enum' (/usr/lib/python3.10/enum.py)
```

And to fix the problem install the tool again:

```sh
uv tool install --verbose filter-pre-commit-hooks
```

The output is:

```txt
DEBUG uv 0.6.2
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.9-linux-x86_64-gnu`
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12` (managed installations)
DEBUG Acquired lock for `/root/.local/share/uv/tools`
DEBUG Checking for Python environment at `/root/.local/share/uv/tools/filter-pre-commit-hooks`
DEBUG Found existing environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
Ignoring existing environment for `filter-pre-commit-hooks`: the requested Python interpreter does not match the environment interpreter
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: filter-pre-commit-hooks*
DEBUG Found fresh response for: https://pypi.org/simple/filter-pre-commit-hooks/
DEBUG Searching for a compatible version of filter-pre-commit-hooks (*)
DEBUG Selecting: filter-pre-commit-hooks==2.0.4 [compatible] (filter_pre_commit_hooks-2.0.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9e/f1/ae280ceba877a3bb424c56d5153b1426a0b3a4b38ad5e0336a774541ebbc/filter_pre_commit_hooks-2.0.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for filter-pre-commit-hooks==2.0.4: click>=8.1.8, <8.1.8+
DEBUG Adding transitive dependency for filter-pre-commit-hooks==2.0.4: pyyaml>=6.0.2, <6.0.2+
DEBUG Found fresh response for: https://pypi.org/simple/click/
DEBUG Searching for a compatible version of click (>=8.1.8, <8.1.8+)
DEBUG Selecting: click==8.1.8 [compatible] (click-8.1.8-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pyyaml/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b9/2b/614b4752f2e127db5cc206abc23a8c19678e92b23c3db30fc86ab731d3bd/PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pyyaml (>=6.0.2, <6.0.2+)
DEBUG Selecting: pyyaml==6.0.2 [compatible] (PyYAML-6.0.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Tried 3 versions: click 1, filter-pre-commit-hooks 1, pyyaml 1
DEBUG marker environment resolution took 0.008s
Resolved 3 packages in 14ms
DEBUG Removed existing environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
DEBUG Creating environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
DEBUG Assessing Python executable as base candidate: /root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Using base executable for virtual environment: /root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Removing executable: `/root/.local/bin/filter-pre-commit-hooks`
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: click==8.1.8
DEBUG Registry requirement already cached: filter-pre-commit-hooks==2.0.4
DEBUG Registry requirement already cached: pyyaml==6.0.2
Installed 3 packages in 27ms
 + click==8.1.8
 + filter-pre-commit-hooks==2.0.4
 + pyyaml==6.0.2
DEBUG Installing tool executables into: /root/.local/bin
DEBUG Looking at `.dist-info` at: /root/.local/share/uv/tools/filter-pre-commit-hooks/lib/python3.12/site-packages/filter_pre_commit_hooks-2.0.4.dist-info
DEBUG Installing executable: `filter-pre-commit-hooks`
Installed 1 executable: filter-pre-commit-hooks
DEBUG Adding receipt for tool `filter-pre-commit-hooks`
DEBUG Adding metadata entry for tool `filter-pre-commit-hooks` at /root/.local/share/uv/tools/filter-pre-commit-hooks/uv-receipt.toml
DEBUG Released lock at `/root/.local/share/uv/tools/.lock`
```

Now the installed tool works fine.

Something seems to be going wrong here.

### Platform

Ubuntu 22.04

### Version

0.6.2

### Python version

_No response_

---

_Label `bug` added by @trallnag on 2025-02-20 20:16_

---

_Comment by @trallnag on 2025-02-20 20:17_

Maybe related: https://github.com/astral-sh/uv/issues/11612

---

_Comment by @charliermarsh on 2025-02-20 20:21_

Interesting, thanks. I'll take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-20 20:30_

---

_Comment by @trallnag on 2025-02-20 20:55_

It has something to do with the dependency "pyyaml == 6.0.2".

I removed the dependency and published the package to TestPyPI (filter-pre-commit-hooks==2.0.9). It all works fine.

```
> root@7b5a1ae2e0c2:/# uv tool install --verbose --index="https://test.pypi.org/simple/" --index-strategy unsafe-best-match --no-cache filter-pre-commit-hooks==2.0.9
DEBUG uv 0.6.2
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.9-linux-x86_64-gnu`
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12` (managed installations)
DEBUG Acquired lock for `/root/.local/share/uv/tools`
DEBUG Checking for Python environment at `/root/.local/share/uv/tools/filter-pre-commit-hooks`
DEBUG Found existing environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
Ignoring existing environment for `filter-pre-commit-hooks`: the requested Python interpreter does not match the environment interpreter
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: filter-pre-commit-hooks>=2.0.9, <2.0.9+
DEBUG No cache entry for: https://test.pypi.org/simple/filter-pre-commit-hooks/
DEBUG No cache entry for: https://pypi.org/simple/filter-pre-commit-hooks/
DEBUG Searching for a compatible version of filter-pre-commit-hooks (>=2.0.9, <2.0.9+)
DEBUG Selecting: filter-pre-commit-hooks==2.0.9 [compatible] (filter_pre_commit_hooks-2.0.9-py3-none-any.whl)
DEBUG No cache entry for: https://test-files.pythonhosted.org/packages/18/43/2a5236f817a8421e576431104b6b3d9a39f0ad73626819995199f1756969/filter_pre_commit_hooks-2.0.9-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for filter-pre-commit-hooks==2.0.9: click>=8.1.8, <8.1.8+
DEBUG No cache entry for: https://test.pypi.org/simple/click/
DEBUG No cache entry for: https://pypi.org/simple/click/
DEBUG Searching for a compatible version of click (>=8.1.8, <8.1.8+)
DEBUG Selecting: click==8.1.8 [compatible] (click-8.1.8-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
DEBUG Tried 2 versions: click 1, filter-pre-commit-hooks 1
DEBUG marker environment resolution took 0.279s
Resolved 2 packages in 282ms
DEBUG Removed existing environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
DEBUG Creating environment for tool `filter-pre-commit-hooks`: /root/.local/share/uv/tools/filter-pre-commit-hooks
DEBUG Assessing Python executable as base candidate: /root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Using base executable for virtual environment: /root/.local/share/uv/python/cpython-3.12.9-linux-x86_64-gnu/bin/python3.12
DEBUG Removing executable: `/root/.local/bin/_filter-pre-commit-hooks`
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: click==8.1.8
DEBUG Identified uncached distribution: filter-pre-commit-hooks==2.0.9
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
DEBUG No cache entry for: https://test-files.pythonhosted.org/packages/18/43/2a5236f817a8421e576431104b6b3d9a39f0ad73626819995199f1756969/filter_pre_commit_hooks-2.0.9-py3-none-any.whl
Prepared 2 packages in 181ms
Installed 2 packages in 34ms
 + click==8.1.8
 + filter-pre-commit-hooks==2.0.9
DEBUG Installing tool executables into: /root/.local/bin
DEBUG Looking at `.dist-info` at: /root/.local/share/uv/tools/filter-pre-commit-hooks/lib/python3.12/site-packages/filter_pre_commit_hooks-2.0.9.dist-info
DEBUG Installing executable: `_filter-pre-commit-hooks`
Installed 1 executable: _filter-pre-commit-hooks
DEBUG Adding receipt for tool `filter-pre-commit-hooks`
DEBUG Adding metadata entry for tool `filter-pre-commit-hooks` at /root/.local/share/uv/tools/filter-pre-commit-hooks/uv-receipt.toml
DEBUG Released lock at `/root/.local/share/uv/tools/.lock`

> root@7b5a1ae2e0c2:/# _filter-pre-commit-hooks --version
_filter-pre-commit-hooks, version 2.0.3
```

If I readd the dependency, build, publish, and the issue reappears (2.0.7 TestPyPI)

---

_Comment by @charliermarsh on 2025-02-20 21:45_

I'm not sure why this is happening in some cases but not others, but it does look like a clear bug in the code (after looking at the relevant routines). We're continuing to use the "bad" interpreter further down, when we should be overriding it.

---

_Closed by @charliermarsh on 2025-02-20 22:05_

---
