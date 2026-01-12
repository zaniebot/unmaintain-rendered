```yaml
number: 6391
title: "minor inconsistent formatting of version specifiers with `uv add`"
type: issue
state: open
author: ChannyClaus
labels: []
assignees: []
created_at: 2024-08-21T23:28:55Z
updated_at: 2024-08-21T23:29:24Z
url: https://github.com/astral-sh/uv/issues/6391
synced_at: 2026-01-12T15:59:03Z
```

# minor inconsistent formatting of version specifiers with `uv add`

---

_@ChannyClaus_

say you have a `pyproject.toml` that looks like the below:
```
[project]
name = "formatting"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "numpy >=2"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
which, looks a bit off with the space between `numpy` and `>=2`. if you run `uv add 'pandas>=2'` on this, you get:
```
[project]
name = "formatting"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "numpy >=2",
    "pandas>=2",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
it would be nice if `uv` formats the dependencies here consistently.

---
