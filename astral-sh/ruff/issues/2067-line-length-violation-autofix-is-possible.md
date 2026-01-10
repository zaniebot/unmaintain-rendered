---
number: 2067
title: line-length violation autofix is possible ?
type: issue
state: closed
author: hasanozdem1r
labels: []
assignees: []
created_at: 2023-01-21T19:00:41Z
updated_at: 2023-01-21T19:05:33Z
url: https://github.com/astral-sh/ruff/issues/2067
synced_at: 2026-01-10T01:22:40Z
---

# line-length violation autofix is possible ?

---

_Issue opened by @hasanozdem1r on 2023-01-21 19:00_

Hello everyone,

I am new for ruff and I am still on the way of exploring features
This is my example ruff.toml file

```
line-length = 80

# Enable Pyflakes, pycodestyle, isort, pep8-naming
select = ["E", "F","I"]

# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
]
per-file-ignores = {}
```
I am curious that is there any way to auto-fix line lenght violations if given line is over 80 character for example ?

ruff version -> "0.0.226"

Example commands to generate error

```
ruff .
ruff . --fix
```
Please let me know if there is any way ?

---

_Closed by @hasanozdem1r on 2023-01-21 19:05_

---

_Comment by @hasanozdem1r on 2023-01-21 19:05_

I have seen in other discussions you already mentioned for future scope possible to implement

---
