```yaml
number: 13226
title: "Inconsistent import ordering via `ruff check --fix --select I`?"
type: issue
state: closed
author: pansen
labels: []
assignees: []
created_at: 2024-09-03T09:51:09Z
updated_at: 2024-09-03T09:56:48Z
url: https://github.com/astral-sh/ruff/issues/13226
synced_at: 2026-01-12T15:54:52Z
```

# Inconsistent import ordering via `ruff check --fix --select I`?

---

_@pansen_

Thanks for your wonderful work! 

## Situation 

We do see some unexplained behaviour of `ruff check --fix --select I`. 

Given I have two different macOS devices, with this spec

- macOS 14.6.1 (23G93)
- Python 3.11.9 via Homebrew
- equally created venv
- ruff 0.6.3 via `poetry install` or `uv pip install`, no difference

The reduced execution looks like this: 

```shell
$ .venv/bin/python -V
Python 3.11.9
$ .venv/bin/ruff -V
ruff 0.6.3
$ .venv/bin/python -m poetry run ruff check --config ruff.toml --fix --select I cluster_api/alembic_/env.py
All checks passed!
```

## Issue

On one of both devices we see the file to stay like this: 
<img width="707" alt="image" src="https://github.com/user-attachments/assets/5024a191-e5a6-4a73-b8eb-0d62b42e9e9b">

on the other one it will be changed like so:
![image (32)](https://github.com/user-attachments/assets/abafa1fb-dbbf-4706-bc5e-5f3520af756d)

This applies to other files as well, but lets focus on one example. Do you have any suggestion what may be wrong?

## Config

Ruff configuration in place for both of them

```shell
$ cat ruff.toml 
# https://docs.astral.sh/ruff/configuration/

# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".ipynb_checkpoints",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pyenv",
    ".pytest_cache",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    ".vscode",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "site-packages",
    "venv",
]

# The GitHub editor is 127 chars wide
line-length = 120
indent-width = 4

target-version = "py311"

[lint]
# 1. Enable flake8-bugbear (`B`) rules, in addition to the defaults.
# select = ["E4", "E7", "E9", "F", "B"]

# Disable fix for unused imports (`F401`).
unfixable = ["F401"]

[lint.per-file-ignores]
"tests/*" = ["F811"]
"smoke_tests/*" = ["F811"]

[format]
# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false
```

---

_Comment by @pansen on 2024-09-03 09:56_

Turned out that we had an `alembic` package top-level, which was refactored to be moved. That top-level `alembic` still existed on some devices, which caused `ruff` to detect it as local package. 

Thanks @hossainemruz for figuring!

---

_Closed by @pansen on 2024-09-03 09:56_

---
