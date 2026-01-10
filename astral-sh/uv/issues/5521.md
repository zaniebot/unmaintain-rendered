```yaml
number: 5521
title: workspace exclude mismatch on linux vs windows
type: issue
state: closed
author: bluss
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-28T18:58:53Z
updated_at: 2024-07-28T21:26:48Z
url: https://github.com/astral-sh/uv/issues/5521
synced_at: 2026-01-10T04:53:49Z
```

# workspace exclude mismatch on linux vs windows

---

_Issue opened by @bluss on 2024-07-28 18:58_

pyprojects like this:

pyproject-local-kernel/pyproject.toml
└── x/pyproject.toml

root declares

```toml
[tool.uv.workspace]
members = []
exclude = ["x", "*", "**"]
```

The child `x` is a test package that should not be part of the workspace, but it depends on the root.

```toml
[project]
name = "x"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = [
    "pyproject-local-kernel",
]

[tool.uv.sources]
pyproject-local-kernel = { path = ".." }
```

- On Linux, this worked as expected and `uv lock` in the `x` package succeedes
- On Windows it errors out with `Package is not included as workspace package in `tool.uv.workspace``

It looks like it reaches different results on linux vs windows. When looking from the `x` perspective, uv thinks `x` is included in the root workspace.

(This bug was found by debugging on CI, so not so much extra information on windows.)
Sorry to pile on another bug.

<details>
<summary>Linux CI uv lock -v</summary>

```
DEBUG uv 0.2.30
warning: `uv lock` is experimental and may change without warning
DEBUG Found workspace root `/home/runner/work/pyproject-local-kernel/pyproject-local-kernel`, but project is excluded
DEBUG Found workspace root: `/home/runner/work/pyproject-local-kernel/pyproject-local-kernel/x`
DEBUG Adding current workspace member: `/home/runner/work/pyproject-local-kernel/pyproject-local-kernel/x`
DEBUG Searching for Python >=3.12 in managed installations or system path
DEBUG Searching for managed installations at `/home/runner/.local/share/uv/python`
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/home/runner/.rye/shims/python3` (search path)
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/home/runner/.rye/shims/python` (search path)
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/bin/python3` (search path)
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/bin/python` (search path)
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/runner/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240726/cpython-3.12.4%2B20240726-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location /home/runner/.local/share/uv/python/.tmpnZpPmh
DEBUG Extracting cpython-3.12.4%2B20240726-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Moving /home/runner/.local/share/uv/python/.tmpnZpPmh/python to /home/runner/.local/share/uv/python/cpython-3.12.4-linux-x86_64-gnu
Using Python 3.12.4
DEBUG Using request timeout of 30s
DEBUG Resolving with existing `uv.lock`
DEBUG Acquired lock for `/home/runner/.cache/uv/built-wheels-v3/editable/72d48d88d28d6a95`
DEBUG Preparing metadata for: x @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel/x
DEBUG No static `PKG-INFO` available for: x @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel/x (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: x @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel/x
DEBUG Found workspace root `/home/runner/work/pyproject-local-kernel/pyproject-local-kernel`, but project is excluded
DEBUG No workspace root found, using project root
warning: `uv.sources` is experimental and may change without warning
DEBUG Acquired lock for `/home/runner/.cache/uv/built-wheels-v3/path/67111aeb387f5d59`
DEBUG Preparing metadata for: pyproject-local-kernel @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel
DEBUG No static `PKG-INFO` available for: pyproject-local-kernel @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: pyproject-local-kernel @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel
DEBUG Found workspace root: `/home/runner/work/pyproject-local-kernel/pyproject-local-kernel`
DEBUG Adding current workspace member: `/home/runner/work/pyproject-local-kernel/pyproject-local-kernel`
DEBUG Solving with installed Python version: 3.12.4
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: x*
DEBUG Searching for a compatible version of x @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel/x (*)
DEBUG Adding transitive dependency for x==0.1.0: pyproject-local-kernel*
DEBUG Searching for a compatible version of pyproject-local-kernel @ file:///home/runner/work/pyproject-local-kernel/pyproject-local-kernel (*)
DEBUG Tried 2 versions: pyproject-local-kernel 1, x 1
DEBUG Split universal resolution took 0.000s
Resolved 2 packages in 4ms
```

</details>

<details>
<summary>Windows CI uv lock -v</summary>

```
DEBUG uv 0.2.30
warning: `uv lock` is experimental and may change without warning
DEBUG Found workspace root `D:\a\pyproject-local-kernel\pyproject-local-kernel`, but project is excluded
DEBUG Found workspace root: `D:\a\pyproject-local-kernel\pyproject-local-kernel\x`
DEBUG Adding current workspace member: `D:\a\pyproject-local-kernel\pyproject-local-kernel\x`
DEBUG Searching for Python >=3.12 in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\runneradmin\AppData\Roaming\uv\data\python`
DEBUG Found `cpython-3.9.13-windows-x86_64-none` at `C:\hostedtoolcache\windows\Python\3.9.13\x64\python3.exe` (search path)
DEBUG Found `cpython-3.9.13-windows-x86_64-none` at `C:\hostedtoolcache\windows\Python\3.9.13\x64\python.exe` (search path)
DEBUG Found `cpython-3.12.4-windows-x86_64-none` at `C:\hostedtoolcache\windows\Python\3.12.4\x64\python.exe` (`py` launcher output)
Using Python 3.12.4 interpreter at: C:\hostedtoolcache\windows\Python\3.12.4\x64\python.exe
DEBUG Using request timeout of 30s
DEBUG Resolving with existing `uv.lock`
DEBUG Acquired lock for `\\?\C:\Users\runneradmin\AppData\Local\uv\cache\built-wheels-v3\editable\2a78b9ffa5c15915`
DEBUG Preparing metadata for: x @ file:///D:/a/pyproject-local-kernel/pyproject-local-kernel/x
DEBUG No static `PKG-INFO` available for: x @ file:///D:/a/pyproject-local-kernel/pyproject-local-kernel/x (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: x @ file:///D:/a/pyproject-local-kernel/pyproject-local-kernel/x
DEBUG Found workspace root: `D:\a\pyproject-local-kernel\pyproject-local-kernel`
DEBUG Adding root workspace member: `D:\a\pyproject-local-kernel\pyproject-local-kernel`
DEBUG Adding current workspace member: `D:\a\pyproject-local-kernel\pyproject-local-kernel\x`
DEBUG Resolution with `uv.lock` failed: Failed to download and build: `x @ file:///D:/a/pyproject-local-kernel/pyproject-local-kernel/x`
DEBUG Starting clean resolution
DEBUG Acquired lock for `\\?\C:\Users\runneradmin\AppData\Local\uv\cache\built-wheels-v3\editable\2a78b9ffa5c15915`
DEBUG Using cached metadata for: x @ file:///D:/a/pyproject-local-kernel/pyproject-local-kernel/x
DEBUG Found workspace root: `D:\a\pyproject-local-kernel\pyproject-local-kernel`
DEBUG Adding root workspace member: `D:\a\pyproject-local-kernel\pyproject-local-kernel`
DEBUG Adding current workspace member: `D:\a\pyproject-local-kernel\pyproject-local-kernel\x`
error: Failed to download and build: `x @ file:///D:/a/pyproject-local-kernel/pyproject-local-kernel/x`
  Caused by: Failed to parse entry for: `pyproject-local-kernel`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

</details>

uv 0.2.30


---

_Renamed from "workspace mismatch on linux vs windows" to "workspace exclude mismatch on linux vs windows" by @bluss on 2024-07-28 18:59_

---

_Label `bug` added by @charliermarsh on 2024-07-28 19:04_

---

_Label `preview` added by @charliermarsh on 2024-07-28 19:04_

---

_Comment by @charliermarsh on 2024-07-28 19:04_

@bluss -- Please keep filing bugs, it's super valuable and appreciated!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-28 19:09_

---

_Comment by @bluss on 2024-07-28 19:13_

Ok good. I'd like to document the glob syntax - I think * is wildcard and ** recursive wildcard - but I didn't quite get my understanding of it to line up with the glob crate's docs.

People running into this bug by the way can in this case work around it by setting `[tool.uv.workspace]` in x's pyproject too, then it definitely will not be assigned as a member of the parent directory.

---

_Closed by @charliermarsh on 2024-07-28 21:26_

---

_Closed by @charliermarsh on 2024-07-28 21:26_

---
