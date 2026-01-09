---
number: 16087
title: Specify a path and index for package source
type: issue
state: open
author: zacharymostowsky
labels:
  - enhancement
assignees: []
created_at: 2025-10-01T16:03:23Z
updated_at: 2025-12-18T10:15:17Z
url: https://github.com/astral-sh/uv/issues/16087
synced_at: 2026-01-07T13:12:19-06:00
---

# Specify a path and index for package source

---

_Issue opened by @zacharymostowsky on 2025-10-01 16:03_

### Question

Hello,

I am trying to specify both a path and index to a dependency. I want to define some dependency in my project dependencies and have it pulled from a custom index but when I develop on the project locally, I want that dependency to be resolved from a local path. Is this possible?

I do not want to use 2 optional dependency groups because this dependency is in fact required for normal use of scripts in this virtual environment.

My hack around right now is to use the marker option and pick which source based on the system platform. This works okay for me as my deployed envs use Linux and locally I am on a Mac but is really not the right solution.

Stack Overflow Issue with same ask/request: https://stackoverflow.com/questions/79471982/uv-index-and-path-for-the-same-dependency-in-one-pyproject-toml-file

I want to be able to do something like this but not sure how to / if possible to reference the main project dependencies.

```toml
[project]
dependencies = [
    "my-package"
]

[dependency-groups]
dev = [
    "my-package",
]

[tool.uv.sources]
my-package = [
  { path = "../path/to/my_package" },
  { index = "my-ado-index" },
]

[[tool.uv.index]]
name = "my-ado-index"
url = "https://my-ado-index/"
```

### Platform

Mac OS 15.6.1 (24G90) arm64

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

---

_Label `question` added by @zacharymostowsky on 2025-10-01 16:03_

---

_Comment by @konstin on 2025-12-18 10:15_

We're working on index proxy support, which should enable this workflow by "proxying" the production index with a file URL.

---

_Label `question` removed by @konstin on 2025-12-18 10:15_

---

_Label `enhancement` added by @konstin on 2025-12-18 10:15_

---
