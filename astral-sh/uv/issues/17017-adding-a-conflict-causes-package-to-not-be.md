```yaml
number: 17017
title: Adding a conflict causes package to not be installed
type: issue
state: open
author: meastham
labels:
  - bug
assignees: []
created_at: 2025-12-06T17:03:05Z
updated_at: 2025-12-06T19:04:09Z
url: https://github.com/astral-sh/uv/issues/17017
synced_at: 2026-01-12T16:02:42Z
```

# Adding a conflict causes package to not be installed

---

_@meastham_

### Summary

Hi,

I've encountered some either buggy or surprising behavior where adding a workspace package `foo` which conflicts with another package `bar` seems to cause `bar` to not be installed when I would expect it to be.

I put together a small repro at https://github.com/meastham/uv-conflict-repro. All of the commands are using uv 0.9.16 on Ubuntu 20.04.

The setup is:
* Workspace member packages/foo has a dependency on numpy~=1.0
* Workspace member packages/bar has a dependency on numpy~=2.0
* The root member has dependency groups named foo and bar which depend on packages foo and bar respectively

Using group `bar` I would expect to be able to import `numpy` 2.0, but that doesn't work:
```
❯ ~/latest_uv run -v --isolated --group bar python
DEBUG uv 0.9.16
DEBUG Acquired shared lock for `/home/mike.eastham/.cache/uv`
DEBUG Found project root: `/home/mike.eastham/repro`
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Adding root workspace member: `/home/mike.eastham/repro`
DEBUG Adding discovered workspace member: `/home/mike.eastham/repro/packages/foo`
DEBUG Adding discovered workspace member: `/home/mike.eastham/repro/packages/bar`
DEBUG Discovered project `repro` at: /home/mike.eastham/repro
DEBUG Creating isolated virtual environment
DEBUG Reading Python requests from version file at `/home/mike.eastham/repro/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Using request timeout of 30s
DEBUG Searching for Python 3.13 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.13.7-linux-x86_64-gnu` at `/home/mike.eastham/repro/.venv/bin/python3` (virtual environment)
DEBUG Assessing Python executable as base candidate: /home/mike.eastham/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu/bin/python3.13
DEBUG Using base executable for virtual environment: /home/mike.eastham/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu/bin/python3.13
DEBUG Acquired exclusive lock for `/home/mike.eastham/.cache/uv/builds-v0/.tmpwylVym`
warning: Declaring conflicts for packages (`package = ...`) is experimental and may change without warning. Pass `--preview-features package-conflicts` to disable this warning.
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: bar @ file:///home/mike.eastham/repro/packages/bar
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Found static `pyproject.toml` for: foo @ file:///home/mike.eastham/repro/packages/foo
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Found static `pyproject.toml` for: repro @ file:///home/mike.eastham/repro
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 5 packages in 0.93ms
DEBUG Using request timeout of 30s
Audited in 0.00ms
DEBUG Released lock at `/home/mike.eastham/.cache/uv/builds-v0/.tmpwylVym/.lock`
DEBUG Using Python 3.13.7 interpreter at: /home/mike.eastham/.cache/uv/builds-v0/.tmpwylVym/bin/python
DEBUG Running `python`
DEBUG Spawned child 140066 in process group 140063
Python 3.13.7 (main, Aug 28 2025, 17:07:40) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

If I comment out all references to foo and the entire conflicts section like this:
```
[project]
name = "repro"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []

[tool.uv.workspace]
members = [
#     "packages/foo",
    "packages/bar",
]

[tool.uv.sources]
# foo = { workspace = true }
bar = { workspace = true }

[dependency-groups]
bar = [
    "bar",
]
# foo = [
#     "foo",
# ]

# [tool.uv]
# conflicts = [
#   [
#     { package = "foo" },
#     { package = "bar" },
#   ],
# ]
```

Then I can import numpy as expected:
```
❯ ~/latest_uv run -v --isolated --group bar python
DEBUG uv 0.9.16
DEBUG Acquired shared lock for `/home/mike.eastham/.cache/uv`
DEBUG Found project root: `/home/mike.eastham/repro`
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Adding root workspace member: `/home/mike.eastham/repro`
DEBUG Adding discovered workspace member: `/home/mike.eastham/repro/packages/bar`
DEBUG Discovered project `repro` at: /home/mike.eastham/repro
DEBUG Creating isolated virtual environment
DEBUG Reading Python requests from version file at `/home/mike.eastham/repro/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Using request timeout of 30s
DEBUG Searching for Python 3.13 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.13.7-linux-x86_64-gnu` at `/home/mike.eastham/repro/.venv/bin/python3` (virtual environment)
DEBUG Assessing Python executable as base candidate: /home/mike.eastham/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu/bin/python3.13
DEBUG Using base executable for virtual environment: /home/mike.eastham/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu/bin/python3.13
DEBUG Acquired exclusive lock for `/home/mike.eastham/.cache/uv/builds-v0/.tmpiQR2QD`
DEBUG Using request timeout of 30s
DEBUG Resolving despite existing lockfile due to change in conflicting groups: `Conflicts([])` vs. `Conflicts([ConflictSet { set: {ConflictItem { package: PackageName("bar"), kind: Project }, ConflictItem { package: PackageName("foo"), kind: Project }} }])`
DEBUG Found static `pyproject.toml` for: bar @ file:///home/mike.eastham/repro/packages/bar
DEBUG Found static `pyproject.toml` for: repro @ file:///home/mike.eastham/repro
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Found workspace root: `/home/mike.eastham/repro`
DEBUG Solving with installed Python version: 3.13.7
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: bar*
DEBUG Adding direct dependency: repro*
DEBUG Adding direct dependency: repro:bar*
DEBUG Searching for a compatible version of bar @ file:///home/mike.eastham/repro/packages/bar (*)
DEBUG Adding direct dependency: numpy>=2.0, <3.dev0
DEBUG Searching for a compatible version of repro @ file:///home/mike.eastham/repro (*)
DEBUG Searching for a compatible version of repro @ file:///home/mike.eastham/repro (*)
DEBUG Adding direct dependency: repro:bar==0.1.0
DEBUG Searching for a compatible version of repro @ file:///home/mike.eastham/repro (==0.1.0)
DEBUG Adding direct dependency: bar*
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (>=2.0, <3.dev0)
DEBUG Selecting: numpy==2.3.5 [preference] (numpy-2.3.5-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/db/69/9cde09f36da4b5a505341180a3f2e6fadc352fd4d2b7096ce9778db83f1a/numpy-2.3.5-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Tried 3 versions: bar 1, numpy 1, repro 1
DEBUG all marker environments resolution took 0.002s
Resolved 3 packages in 3ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Timestamp(SystemTime { tv_sec: 1765039189, tv_nsec: 65798283 }), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1765038884, tv_nsec: 453763794 })))}. Most recently modified: packages/bar/pyproject.toml
DEBUG Directory source requirement already cached: bar==0.1.0 (from file:///home/mike.eastham/repro/packages/bar)
DEBUG Registry requirement already cached: numpy==2.3.5
Installed 2 packages in 20ms
 + bar==0.1.0 (from file:///home/mike.eastham/repro/packages/bar)
 + numpy==2.3.5
DEBUG Released lock at `/home/mike.eastham/.cache/uv/builds-v0/.tmpiQR2QD/.lock`
DEBUG Using Python 3.13.7 interpreter at: /home/mike.eastham/.cache/uv/builds-v0/.tmpiQR2QD/bin/python
DEBUG Running `python`
DEBUG Spawned child 141923 in process group 141883
Python 3.13.7 (main, Aug 28 2025, 17:07:40) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy.version
>>> numpy.version.version
'2.3.5'
>>>
```

### Platform

Ubuntu 20.04 amd64

### Version

uv 0.9.16

### Python version

Python 3.13.7

---

_Label `bug` added by @meastham on 2025-12-06 17:03_

---

_Assigned to @zanieb by @zanieb on 2025-12-06 19:04_

---
