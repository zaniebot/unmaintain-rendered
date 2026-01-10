```yaml
number: 17083
title: "`uv add` fails if UV_GIT_LFS=1"
type: issue
state: closed
author: kod-kristoff
labels:
  - bug
assignees: []
created_at: 2025-12-11T12:25:43Z
updated_at: 2025-12-15T14:26:16Z
url: https://github.com/astral-sh/uv/issues/17083
synced_at: 2026-01-10T03:11:35Z
```

# `uv add` fails if UV_GIT_LFS=1

---

_Issue opened by @kod-kristoff on 2025-12-11 12:25_

### Summary

I can't add dependencies with `uv add`, all fails with trying to resolve the name as a Git repository.

```bash
> uv init uv-import
> cd uv-import
> uv add setuptools
error: `setuptools` did not resolve to a Git repository, but a Git extension (`--lfs`) was provided.
> uv add pandas
error: `pandas` did not resolve to a Git repository, but a Git extension (`--lfs`) was provided.
```

**UPDATE**: If I run `unset UV_GIT_LFS`, the problem is resolved.

With `-vvv`:

```bash
> uv add -vvv setuptools
DEBUG uv 0.9.17 (2b5d65e61 2025-12-09)
TRACE Checking lock for `/home/-/.cache/uv` at `/home/-/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/-/.cache/uv`
DEBUG Found project root: `/home/-/projekt/work/uv-test`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/home/-/projekt/work/uv-test` at `/tmp/uv-43e8c4775b984fd5.lock`
DEBUG Acquired exclusive lock for `/home/-/projekt/work/uv-test`
DEBUG Reading Python requests from version file at `/home/-/projekt/work/uv-test/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
TRACE Found cached interpreter info for Python 3.11.6, skipping query of: .venv/bin/python3
DEBUG The project environment's Python version satisfies the request: `Python 3.11`
TRACE The project environment's Python version meets the Python requirement: `>=3.11`
TRACE The virtual environment's Python interpreter meets the Python preference: `prefer managed`
DEBUG Released lock at `/tmp/uv-43e8c4775b984fd5.lock`
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired exclusive lock for `.venv`
DEBUG Released lock at `/home/-/projekt/work/uv-test/.venv/.lock`
DEBUG Released lock at `/home/-/.cache/uv/.lock`
TRACE Error trace: `setuptools` did not resolve to a Git repository, but a Git extension (`--lfs`) was provided.
error: `setuptools` did not resolve to a Git repository, but a Git extension (`--lfs`) was provided.
```

**Expected behavoiur**: The given package is added to `pyproject.toml` and installed in `.venv`.

**Workaround**: Add it manually to `pyproject.toml` and run `uv sync`

### Platform

Linux 6.17.9-arch1-1 x86_64 GNU/Linux (Arch Linux)

### Version

uv 0.9.17 (2b5d65e61 2025-12-09)

### Python version

Python 3.11.6, Python 3.13.11

---

_Label `bug` added by @kod-kristoff on 2025-12-11 12:25_

---

_Renamed from "`uv add` setuptools fails, treats setuptools as Git repository" to "`uv add` fails if UV_GIT_LFS=1" by @kod-kristoff on 2025-12-11 12:29_

---

_Comment by @konstin on 2025-12-11 14:54_

CC @samypr100 

---

_Comment by @zanieb on 2025-12-11 15:15_

The error comes from https://github.com/astral-sh/uv/blob/fee7f9d093b3ef20f4e86aa7f92ebe67145240f6/crates/uv-workspace/src/pyproject.rs#L1660-L1662

We definitely need to relax this. Maybe we need to parse `UV_GIT_LFS` separately from `--lfs` and treat a global lfs toggle as different from the non-global one?

---

_Comment by @samypr100 on 2025-12-11 15:52_

Agreed, in hindsight I should've not used the same env var in https://github.com/astral-sh/uv/blob/59d73fdddf7281033cd9b8f50d6e978b97db1317/crates/uv-cli/src/lib.rs#L4164

---

_Comment by @zanieb on 2025-12-11 15:53_

We have our own `parse_boolish_environment_variable` and `EnvironmentOptions` struct now

---

_Assigned to @samypr100 by @samypr100 on 2025-12-12 01:19_

---

_Closed by @zanieb on 2025-12-15 14:26_

---
