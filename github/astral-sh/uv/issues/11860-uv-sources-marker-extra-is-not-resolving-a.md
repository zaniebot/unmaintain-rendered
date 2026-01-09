---
number: 11860
title: Uv sources marker extra is not resolving a package correctly
type: issue
state: closed
author: krystianMoras
labels:
  - question
assignees: []
created_at: 2025-02-28T16:38:00Z
updated_at: 2025-03-14T00:38:47Z
url: https://github.com/astral-sh/uv/issues/11860
synced_at: 2026-01-07T13:12:18-06:00
---

# Uv sources marker extra is not resolving a package correctly

---

_Issue opened by @krystianMoras on 2025-02-28 16:38_

### Summary

To reproduce, example pyproject.toml
```
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pytube>=15.0.0",
]

[project.optional-dependencies]
dev = []

[[tool.uv.index]]
name = "my_index"
url = "https://pypi.org/simple"


[tool.uv.sources]
pytube = [
    { index = "my_index", marker = "extra!='dev'"},
    { path = "./pytube", editable = true, marker = "extra=='dev'"}
]
```

Running ` uv sync --extra=dev` will correctly install pytube from my path as editable
```
DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24)
DEBUG Found project root: `/Users/krystianmoras/Documents/test-uv`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/krystianmoras/Documents/test-uv`
DEBUG Reading Python requests from version file at `/Users/krystianmoras/Documents/test-uv/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `/var/folders/1_/btj1k7sn3ls02qcwrbhlp5zc0000gp/T/uv-046f04b84c3c71d1.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-uv @ file:///Users/krystianmoras/Documents/test-uv
DEBUG No workspace root found, using project root
DEBUG Found static `pyproject.toml` for: pytube @ file:///Users/krystianmoras/Documents/test-uv/pytube
DEBUG Project is contained in non-workspace project: `/Users/krystianmoras/Documents/test-uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 2 packages in 0.72ms
DEBUG Using request timeout of 30s
DEBUG Directory source requirement already cached: pytube==0.1.0 (from file:///Users/krystianmoras/Documents/test-uv/pytube)
Installed 1 package in 2ms
 + pytube==0.1.0 (from file:///Users/krystianmoras/Documents/test-uv/pytube)
```

However running just `uv sync` should resolve to use 'my_index', instead pytube package is uninstalled
```
DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24)
DEBUG Found project root: `/Users/krystianmoras/Documents/test-uv`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/krystianmoras/Documents/test-uv`
DEBUG Reading Python requests from version file at `/Users/krystianmoras/Documents/test-uv/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `3.12`
DEBUG Released lock at `/var/folders/1_/btj1k7sn3ls02qcwrbhlp5zc0000gp/T/uv-046f04b84c3c71d1.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-uv @ file:///Users/krystianmoras/Documents/test-uv
DEBUG No workspace root found, using project root
DEBUG Found static `pyproject.toml` for: pytube @ file:///Users/krystianmoras/Documents/test-uv/pytube
DEBUG Project is contained in non-workspace project: `/Users/krystianmoras/Documents/test-uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 2 packages in 0.75ms
DEBUG Using request timeout of 30s
DEBUG Unnecessary package: pytube==0.1.0 (from file:///Users/krystianmoras/Documents/test-uv/pytube)
DEBUG Uninstalled pytube (10 files, 1 directory)
Uninstalled 1 package in 0.61ms
 - pytube==0.1.0 (from file:///Users/krystianmoras/Documents/test-uv/pytube)
```

I know of existence of workspaces, I'm just trying to illustrate the problem. This is a different problem where we want to be able to specify different package sources including our private indices. 

This is an attempt to go around limitations of not being able to have multiple pyproject.toml and no flags that would enable selecting sources. Perhaps it could be solved if sources were allowed in uv.toml https://github.com/astral-sh/uv/issues/6772

We already have a workaround where we build a package and generate an index with `dumb-pypi` + `uv sync --index $LOCAL_INDEX_PATH --no-sources` however it would be much easier to omit that building step.

Nevertheless I think this is a bug and if `marker = "extra!='dev'"` it should install from `my_index`

### Platform

all

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

_No response_

---

_Label `bug` added by @krystianMoras on 2025-02-28 16:38_

---

_Comment by @charliermarsh on 2025-02-28 21:09_

Hmm, we don't support extras in that `marker` field. I think it would be dubious to allow extras to modify packages that aren't installed as part of that extra. In general, you can put `extra = "dev"` on sources to ensure that a source only applies when an dependecy is included as part of an optional group.

---

_Label `bug` removed by @charliermarsh on 2025-02-28 21:09_

---

_Label `question` added by @charliermarsh on 2025-02-28 21:09_

---

_Closed by @charliermarsh on 2025-03-14 00:38_

---
