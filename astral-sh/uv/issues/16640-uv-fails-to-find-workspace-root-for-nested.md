---
number: 16640
title: uv fails to find workspace root for nested packages
type: issue
state: open
author: stewartadam
labels:
  - bug
assignees: []
created_at: 2025-11-08T00:50:33Z
updated_at: 2025-11-10T20:56:13Z
url: https://github.com/astral-sh/uv/issues/16640
synced_at: 2026-01-10T01:26:08Z
---

# uv fails to find workspace root for nested packages

---

_Issue opened by @stewartadam on 2025-11-08 00:50_

### Summary

In a multi-level workspace, uv appears to treat nested members differently from top-level members:
```
mkdir -p foo/a/b
cd foo; uv init
cd a; uv init
cd b; uv init

cd ../ # we're under 'foo/a' now
uv sync # creates .venv and uv.lock at the workspace root 'foo/'

cd b
uv sync # creates a new .venv and uv.lock under 'foo/a/b'
```

I would expect that `foo` workspace member `a/b` continues to use the workspace-level .venv/uv.lock as it did for the first-level workspace member `a`.

`uv sync -v --all-packages` from within the `foo/a` folder:
```
DEBUG uv 0.9.7 (Homebrew 2025-10-30)
DEBUG Acquired shared lock for `/Users/stewartadam/.cache/uv`
DEBUG Found project root: `/Users/stewartadam/foo/a`
DEBUG Found workspace root: `/Users/stewartadam/foo`
DEBUG Adding root workspace member: `/Users/stewartadam/foo`
DEBUG Adding discovered workspace member: `/Users/stewartadam/foo/a`
DEBUG Adding discovered workspace member: `/Users/stewartadam/foo/a/b`
DEBUG Acquired lock for `/Users/stewartadam/foo`
DEBUG Reading Python requests from version file at `/Users/stewartadam/foo/.python-version`
DEBUG Using Python request `3.12` from version file at `/Users/stewartadam/foo/.python-version`
DEBUG Checking for Python environment at: `/Users/stewartadam/foo/.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.12`
DEBUG Released lock at `/var/folders/1m/xxn68ngs7bb6h6x6bt442tth0000gn/T/uv-d4d39f85f2047a7c.lock`
DEBUG Acquired lock for `/Users/stewartadam/foo/.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: a @ file:///Users/stewartadam/foo/a
DEBUG Found workspace root: `/Users/stewartadam/foo`
DEBUG Found static `pyproject.toml` for: b @ file:///Users/stewartadam/foo/a/b
DEBUG Project is contained in non-workspace project: `/Users/stewartadam/foo/a`
DEBUG No workspace root found, using project root
DEBUG Found static `pyproject.toml` for: foo @ file:///Users/stewartadam/foo
DEBUG Found workspace root: `/Users/stewartadam/foo`
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 3 packages in 5ms
DEBUG Using request timeout of 30s
Audited in 0.00ms
DEBUG Released lock at `/Users/stewartadam/foo/.venv/.lock`
DEBUG Released lock at `/Users/stewartadam/.cache/uv/.lock`
```

`uv sync -v --all-packages` from within the `foo/a/b` folder:
```
DEBUG uv 0.9.7 (Homebrew 2025-10-30)
DEBUG Acquired shared lock for `/Users/stewartadam/.cache/uv`
DEBUG Found project root: `/Users/stewartadam/foo/a/b`
DEBUG Project is contained in non-workspace project: `/Users/stewartadam/foo/a`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/stewartadam/foo/a/b`
DEBUG No Python version file found in workspace: /Users/stewartadam/foo/a/b
DEBUG Using Python request `>=3.12` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.12`
DEBUG Released lock at `/var/folders/1m/xxn68ngs7bb6h6x6bt442tth0000gn/T/uv-38125e2740c2addc.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: b @ file:///Users/stewartadam/foo/a/b
DEBUG Project is contained in non-workspace project: `/Users/stewartadam/foo/a`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 3ms
DEBUG Using request timeout of 30s
Audited in 0.00ms
DEBUG Released lock at `/Users/stewartadam/foo/a/b/.venv/.lock`
DEBUG Released lock at `/Users/stewartadam/.cache/uv/.lock`
```

### Platform

MacOS Tahoe

### Version

uv 0.9.7 (Homebrew 2025-10-30)

### Python version

Python 3.12.12

---

_Label `bug` added by @stewartadam on 2025-11-08 00:50_

---

_Comment by @charliermarsh on 2025-11-08 03:08_

I thought nested members were rejected entirely but that may have been made more permissive at some point. I think @konstin would know.

---

_Comment by @stewartadam on 2025-11-08 06:53_

I wasn't sure either, but https://github.com/astral-sh/uv/issues/5251 seems to indicate that this is unexpected behavior and that nested members all sharing the root-level workspace was the desired behavior.

---

_Comment by @konstin on 2025-11-10 20:56_

Nested packages in workspaces shouldn't be a thing. Given that they do somewhat work, I would:

* Add a warning that nesting packages is an anti-pattern
* Change workspace discovery so that `foo/a/b` discovery doesn't stop at `foo/a` but discovers `foo` too.

---
