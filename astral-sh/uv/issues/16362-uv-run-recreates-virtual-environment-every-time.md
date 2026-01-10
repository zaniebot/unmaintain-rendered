---
number: 16362
title: uv run recreates virtual environment every time when using asdf-managed Python
type: issue
state: closed
author: joelleibow
labels: []
assignees: []
created_at: 2025-10-19T21:42:25Z
updated_at: 2025-10-19T22:10:39Z
url: https://github.com/astral-sh/uv/issues/16362
synced_at: 2026-01-10T01:26:05Z
---

# uv run recreates virtual environment every time when using asdf-managed Python

---

_Issue opened by @joelleibow on 2025-10-19 21:42_

## Summary

When using Python managed by `asdf`, `uv run` recreates the virtual environment on every invocation, even when the Python version is correct and satisfies requirements. This happens because `uv` compares the asdf shim path (`~/.asdf/shims/python`) with the actual interpreter path (`~/.asdf/installs/python/3.13.5/bin/python`) and considers them different, triggering a venv rebuild.

## Minimal Reproducible Example

**Prerequisites:**
- Python managed by `asdf`
- `uv` installed via Homebrew

**Steps to reproduce:**

1. Create a new project:
```bash
uv init test-project --no-pin-python
cd test-project
```

2. Sync and verify venv is created:
```bash
uv sync
# Virtual environment created successfully
```

3. Run a command twice:
```bash
uv run python --version
# Works, uses existing venv

uv run python --version
# Recreates venv unnecessarily
```

**Observed output:**
```
$ uv run python --version
Python 3.13.5

$ uv run python --version
Using CPython 3.13.5 interpreter at: ~/.asdf/installs/python/3.13.5/bin/python
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Installed 1 package in 250ms
Python 3.13.5
```

**With verbose output:**
```bash
$ uv run --verbose python --version 2>&1 | grep -A2 "not satisfy"

DEBUG Using Python request \`~/.asdf/shims/python\` from explicit request
DEBUG Checking for Python environment at: \`.venv\`
DEBUG The project environment's Python version does not satisfy the request: \`path \`~/.asdf/shims/python\`\`
DEBUG Checking for Python interpreter at path \`~/.asdf/shims/python\`
Using CPython 3.13.5 interpreter at: ~/.asdf/installs/python/3.13.5/bin/python
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```

The issue is clear: uv is comparing **paths** rather than resolving shims to their actual interpreter and comparing **versions**.

## Expected Behavior

`uv` should resolve the asdf shim to the actual Python interpreter before comparing paths, or compare Python versions rather than paths. The virtual environment should persist across `uv run` invocations when the Python version is compatible.

## Additional Context

- This issue is similar to #14884, but occurs even **without** `.python-version` file
- The `pyproject.toml` has `requires-python = ">=3.13.5"`
- The venv's `pyvenv.cfg` correctly points to the actual interpreter
- Direct venv usage (`.venv/bin/python`) works fine
- The issue affects all `uv run` commands, adding ~250ms overhead per invocation

## Workaround

Setting `UV_PYTHON` to the actual interpreter path works but is impractical:
```bash
export UV_PYTHON=~/.asdf/installs/python/3.13.5/bin/python
uv run python --version  # Now works without rebuilding
```

However, this is not a proper solution as:
- It's machine-specific (breaks on different machines)
- It breaks when upgrading Python versions
- It requires maintaining environment variables per project

## Platform

macOS 14.6 (Darwin 24.6.0) arm64

## Version

uv 0.9.4 (Homebrew 2025-10-18)

## Python Version

Python 3.13.5 (managed by asdf 0.18.0)

## Configuration Files

**pyproject.toml:**
```toml
[project]
name = "test-project"
version = "0.1.0"
requires-python = ">=3.13.5"
dependencies = []
```

**Shell environment:**
```bash
$ which python
~/.asdf/shims/python

$ python --version
Python 3.13.5

$ asdf which python
~/.asdf/installs/python/3.13.5/bin/python
```

**After venv creation - pyvenv.cfg:**
```ini
home = ~/.asdf/installs/python/3.13.5/bin
implementation = CPython
uv = 0.9.4
version_info = 3.13.5
include-system-site-packages = false
```

## Suggested Fix

`uv` should:
1. Resolve shims/symlinks to actual interpreter paths before comparison
2. Compare Python versions (semantic versioning) rather than exact paths
3. Consider a venv valid if the Python version satisfies `requires-python`, regardless of interpreter path

This would make `uv` work seamlessly with version managers like `asdf`, `pyenv`, `mise`, etc., which all use shim-based approaches.

---

_Comment by @joelleibow on 2025-10-19 22:10_

## Resolution: User Error

We've identified the root cause - this was **not a bug in uv**, but rather an environment configuration issue on my end.

**Root Cause:**
- `UV_PYTHON=/Users/joelleibow/.asdf/shims/python` was set in my `.zshrc`
- This caused uv to recreate the virtual environment on every invocation

**The Fix:**
1. Removed the `UV_PYTHON` export from `.zshrc`
2. Restarted VS Code to pick up the clean environment
3. Now uv uses its default Python resolution and doesn't recreate `.venv`

**Verification:**
After the fix, running `uv run` commands no longer shows:
```
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Installed 133 packages in XXXms
```

The `.venv` directory now persists correctly across invocations.

Apologies for the noise - closing this issue as it was a configuration error on my side.

---

_Closed by @joelleibow on 2025-10-19 22:10_

---
