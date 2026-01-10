---
number: 10878
title: "`exclude` under `[tool.ruff.lint]` does not exclude directory"
type: issue
state: closed
author: ajhynes7
labels: []
assignees: []
created_at: 2024-04-11T12:58:00Z
updated_at: 2024-04-11T14:56:32Z
url: https://github.com/astral-sh/ruff/issues/10878
synced_at: 2026-01-10T01:22:50Z
---

# `exclude` under `[tool.ruff.lint]` does not exclude directory

---

_Issue opened by @ajhynes7 on 2024-04-11 12:58_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
# Description

I've found that `ruff check` doesn't exclude a directory, although the name is included in the `exclude` list under `[tool.ruff.lint]` in `pyproject.toml`.

From the ruff docs, in [Settings/lint/exclude](https://docs.astral.sh/ruff/settings/#lint_exclude):

> Exclusions are based on globs, and can be either:
> - Single-path patterns, like `.mypy_cache` (to exclude any directory named `.mypy_cache` in the tree), `foo.py` (to exclude any file named `foo.py`), or `foo_*.py` (to exclude any file matching `foo_*.py`).

However, I've found that specifying the directory name doesn't work. 

# Environment
```bash
$  ruff --version
ruff 0.3.5
```


# Reproduction
I reproduced this with a minimal project.

Project contents:
```bash       
$ tree
.
├── dir1
│   └── foo.py
├── poetry.lock
└── pyproject.toml
```

`foo.py`:
```py
# foo.py

def foo():
    x = 1
```

`pyproject.toml`:
```toml

[tool.poetry]
name = "foo"
authors = []
description = ""
version = "0.1.0"

[tool.poetry.dependencies]
python = "^3.11"

[tool.poetry.group.dev.dependencies]
ruff = "^0.3.5"

[tool.ruff.lint]
exclude = ["dir1"]

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

Command:
```bash
$  ruff check .
dir1/foo.py:2:5: F841 Local variable `x` is assigned to but never used
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

# Notes
If I use `exclude` in the top-level `[tool.ruff]` section instead, the directory is excluded as expected.

```toml
[tool.ruff]
exclude = ["dir1"]
```

```bash
$  ruff check .
All checks passed!
```

I can also get the directory excluded if I put a wildcard at the end. But according to the docs, the directory name alone should suffice.

```toml
[tool.ruff.lint]
exclude = ["dir1/*"]
```

```bash
$  ruff check .
All checks passed!
```

---

_Comment by @MichaReiser on 2024-04-11 14:56_

Yeah, this is a known bug that we haven't found time to fix yet. I'm sorry you run into this. 

We're tracking this bug in https://github.com/astral-sh/ruff/issues/8267

---

_Closed by @MichaReiser on 2024-04-11 14:56_

---

_Referenced in [brainglobe/brainglobe-atlasapi#584](../../brainglobe/brainglobe-atlasapi/pulls/584.md) on 2025-07-31 09:55_

---
