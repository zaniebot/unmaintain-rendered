```yaml
number: 17649
title: Resolver pins direct dependency instead of backtracking, causing Python-incompatible resolution
type: issue
state: open
author: alexjbuck
labels:
  - bug
assignees: []
created_at: 2026-01-21T21:48:28Z
updated_at: 2026-01-21T21:49:04Z
url: https://github.com/astral-sh/uv/issues/17649
synced_at: 2026-01-21T22:07:23Z
```

# Resolver pins direct dependency instead of backtracking, causing Python-incompatible resolution

---

_@alexjbuck_

### Summary

This is related to many existing issues that are closed as "working as designed" related to `numba` and incorrect resolution, however I think this is a bit different.

Prior issues:
  - https://github.com/astral-sh/uv/issues/8863 - Closed as duplicate
  - https://github.com/astral-sh/uv/issues/14484 - Closed
  - https://github.com/astral-sh/uv/issues/4022 - Design discussion: uv intentionally ignores requires-python upper
  bounds

  ## Minimal reproduction

  ```toml
  # pyproject.toml
  [project]
  name = "uv-resolution-bug"
  version = "0.1.0"
  requires-python = ">=3.12"
  dependencies = ["openai-whisper>=20250625", "numpy>=1.26.4"]
```

## The issue

  Expected: at some point uv should try backtracking numpy when the latest resolution (2.4.1 in this case) is unable to resolve.

  Actual: uv locks numpy at 2.4.1, backtracks numba to 0.53.1, which pulls llvmlite 0.36.0 (Python <3.10 only), and fails at build time without ever attempting earlier versions of numpy.

  Output
```
$ rm -rf .venv uv.lock && uv sync
$ uv sync
  Using CPython 3.12.11
  Creating virtual environment at: .venv
  Resolved 43 packages in 7ms
    × Failed to build `llvmlite==0.36.0`
    ├─▶ The build backend returned an error
    ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

        [stderr]
        ...
        RuntimeError: Cannot install on Python version 3.12.11; only versions >=3.6,<3.10 are supported.

        hint: This usually indicates a problem with the package or the build environment.
    help: `llvmlite` (v0.36.0) was included because `uv-resolution-bug` (v0.1.0) depends on
          `openai-whisper` (v20250625) which depends on `numba` (v0.53.1) which depends on `llvmlite`
```

[`uv sync --verbose`](https://github.com/user-attachments/files/24780142/verbose.log)

## Workaround

  Adding an upper bound to numpy or adding a lower bound to numba allows resolving. The thing that feels wrong about this is that the valid resolution _is_ within the defined space. `numpy==2.3.5` satisfies `numpy>=1.26.4` and there's no explicit bounds on numba (in the minimal example), but `uv` fails to find this resolution, and apparently fails to do so because it locks in numpy 2.4.1 and doesn't backtrack to find an earlier numpy version that works.

### Working versions

```toml
[project]
name = "uv-resolution-bug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "numba>=0.63.1",
    "numpy>=1.26.4",
    "openai-whisper>=20250625",
]
```
or
```toml
[project]
name = "uv-resolution-bug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "numpy>=1.26.4,<2.4",
    "openai-whisper>=20250625"
]
```
## The difference

  The prior issues focus on uv ignoring `requires-python` upper bounds. This issue is about **uv not
  backtracking a direct dependency when a lower (but still satisfying) version would resolve the conflict**.

  Specifically:
  - With just `openai-whisper>=20250625`: uv correctly resolves `numpy==2.3.5`, `numba==0.63.1`
  - With `openai-whisper>=20250625` + `numpy>=1.26.4` from a clean sync (no venv, no lockfile): uv selects `numpy==2.4.1`, then backtracks numba until it finds 


`numba==0.53.1` (which has no numpy upper bound but requires Python <3.10)

  The constraint `numpy>=1.26.4` is satisfied by `numpy==2.3.5`, yet uv doesn't backtrack numpy and instead keeps backtracking numba until it hits a version incompatible with Python 3.12.

## Environment

  - uv: 0.9.26 (ee4f00362 2026-01-15)
  - OS: macOS 14 (Darwin 24.6.0 arm64)
  - Python: 3.12.11

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.9.26 (ee4f00362 2026-01-15)

### Python version

Python 3.12.11

---

_Label `bug` added by @alexjbuck on 2026-01-21 21:48_

---

_Renamed from "Resolution selects Python-incompatible transitive dependency instead of backtracking direct dependency" to "Resolver pins direct dependency instead of backtracking, causing Python-incompatible resolution" by @alexjbuck on 2026-01-21 21:49_

---
