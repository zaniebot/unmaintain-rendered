```yaml
number: 16186
title: "`uv sync --resolution=lowest-direct` selects incompatible version for `bottleneck` on macOS+arm64"
type: issue
state: open
author: neutrinoceros
labels:
  - error messages
assignees: []
created_at: 2025-10-08T12:55:21Z
updated_at: 2025-10-08T13:53:26Z
url: https://github.com/astral-sh/uv/issues/16186
synced_at: 2026-01-10T03:23:54Z
```

# `uv sync --resolution=lowest-direct` selects incompatible version for `bottleneck` on macOS+arm64

---

_Issue opened by @neutrinoceros on 2025-10-08 12:55_

### Summary

MRE `pyproject.toml`
```toml
[project]
name = "test"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "bottleneck>=1.3.3",
]

[project.scripts]
test = "test:main"

[build-system]
requires = ["uv_build>=0.9.0,<0.10.0"]
build-backend = "uv_build"

[tool.uv]
no-build-package = [
    "bottleneck",
]
```

```
❯ uv venv -c -p 3.11
❯ rm uv.lock
❯ RUST_LOG=uv=debug uv sync --resolution=lowest-direct --no-cache
```

<details><summary>detailled logs</summary>

```log
DEBUG uv 0.9.0 (39b688653 2025-10-07)
DEBUG Disabling the uv cache due to `--no-cache`
DEBUG Acquired shared lock for `/var/folders/47/t9k4zdz96vzd4v1rxyv07nvh0000gn/T/.tmp5yW0nM`
DEBUG Found project root: `/private/tmp/test`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/private/tmp/test`
DEBUG Reading Python requests from version file at `/private/tmp/test/.python-version`
DEBUG Using Python request `3.14` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.14`
DEBUG Released lock at `/var/folders/47/t9k4zdz96vzd4v1rxyv07nvh0000gn/T/uv-a0e9bb00a68d9699.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test @ file:///private/tmp/test
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.14
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///private/tmp/test (*)
DEBUG Adding direct dependency: bottleneck>=1.3.3
DEBUG No cache entry for: https://pypi.org/simple/bottleneck/
WARN Skipping file for bottleneck: Bottleneck-0.4.0_32bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.0_64bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.1_32bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.1_64bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.2_32bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.2_64bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.3_32bitOS.tar.gz
WARN Skipping file for bottleneck: Bottleneck-0.4.3_64bitOS.tar.gz
DEBUG Searching for a compatible version of bottleneck (>=1.3.3)
DEBUG Searching for a compatible version of bottleneck (>1.3.3)
DEBUG Searching for a compatible version of bottleneck (>1.3.3, <1.3.4 | >1.3.4)
DEBUG Searching for a compatible version of bottleneck (>1.3.3, <1.3.4 | >1.3.4, <1.3.5 | >1.3.5)
DEBUG Searching for a compatible version of bottleneck (>1.3.3, <1.3.4 | >1.3.4, <1.3.5 | >1.3.5, <1.3.6 | >1.3.6)
DEBUG Selecting: bottleneck==1.3.7 [compatible] (Bottleneck-1.3.7-cp311-cp311-macosx_10_9_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/11/8e/97b5d2cce0733ce27abda9288b5e6c85a6e4dade374b25e9eaaa780ac3c8/Bottleneck-1.3.7-cp311-cp311-macosx_10_9_x86_64.whl.metadata
DEBUG Adding transitive dependency for bottleneck==1.3.7: numpy*
DEBUG No cache entry for: https://pypi.org/simple/numpy/
WARN Skipping file for numpy: numpy-1.0.1.dev3460.win32-py2.4.exe
WARN Skipping file for numpy: numpy-1.3.0.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.5.0.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py2.5-nosse.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py2.6-nosse.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py2.7-nosse.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py3.1-nosse.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py3.3.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py3.3.exe
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==2.3.3 [compatible] (numpy-2.3.3-cp311-cp311-macosx_10_9_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7a/45/e80d203ef6b267aa29b22714fb558930b27960a0c5ce3c19c999232bb3eb/numpy-2.3.3-cp311-cp311-macosx_10_9_x86_64.whl.metadata
DEBUG Tried 3 versions: bottleneck 1, numpy 1, test 1
DEBUG all marker environments resolution took 0.233s
Resolved 3 packages in 260ms
DEBUG Released lock at `/private/tmp/test/.venv/.lock`
DEBUG Released lock at `/var/folders/47/t9k4zdz96vzd4v1rxyv07nvh0000gn/T/.tmp5yW0nM/.lock`
error: Distribution `bottleneck==1.3.7 @ registry+https://pypi.org/simple` can't be installed because it is marked as `--no-build` but has no binary distribution
```

</details>

crucial excerpt 
```
...
DEBUG Selecting: bottleneck==1.3.7 [compatible] (Bottleneck-1.3.7-cp311-cp311-macosx_10_9_x86_64.whl)
...
error: Distribution `bottleneck==1.3.7 @ registry+https://pypi.org/simple` can't be installed because it is marked as `--no-build` but has no binary distribution
```

so uv ends up marking some version as compatible even though it doesn't have a wheel for the platform where it's running the resolution (macOS arm64). Is it a coincidence that it's showing another macos wheel (`cp311-macosx_10_9_x86_64`) as evidence for compatibility ? is this a bug, or a limitation of universal resolution ? In the latter case, is there a more elegant workaround than bumping my minimal requirement or adding a constraint ?

### Platform

macOS 15.7.1 arm64

### Version

uv 0.9.0 (39b688653 2025-10-07)

### Python version

3.11.x

---

_Label `bug` added by @neutrinoceros on 2025-10-08 12:55_

---

_Comment by @konstin on 2025-10-08 13:13_

In this case, you need to set `tool.uv.required-environments` to your macOS arm64 environment (https://docs.astral.sh/uv/concepts/resolution/#required-environments). `no-build-package` influences the resolution for `uv pip install`, but can't influence the universal resolution for `uv sync`, which needs to be independent of the platform where uv is currently running on. Usually we show a hint for `tool.uv.required-environments`, but it looks like the `--no-build` hint takes precedence.

---

_Label `bug` removed by @konstin on 2025-10-08 13:13_

---

_Label `error messages` added by @konstin on 2025-10-08 13:13_

---

_Comment by @neutrinoceros on 2025-10-08 13:53_

excellent, thank you ! Indeed I didn't know about `required-environments` and that's exactly what I needed. Hopefully it's possible to make the logs point to it.

---
