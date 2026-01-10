---
number: 10216
title: "`uv export` does not include workspace packages"
type: issue
state: closed
author: brownj85
labels:
  - question
assignees: []
created_at: 2024-12-28T22:15:15Z
updated_at: 2024-12-30T20:09:56Z
url: https://github.com/astral-sh/uv/issues/10216
synced_at: 2026-01-10T01:24:51Z
---

# `uv export` does not include workspace packages

---

_Issue opened by @brownj85 on 2024-12-28 22:15_

UV version: `0.5.13`

I'm trying to switch Poetry to UV, and I've encountered a difference in behaviour between `poetry export` and `uv export`. I have a project laid out like this:

```
.
├── apps
│   ├── lib-1
│   │   ├── README.md
│   │   ├── hello.py
│   │   ├── pyproject.toml
│   │   └── uv.lock
│   └── lib-2
│       ├── README.md
│       ├── hello.py
│       └── pyproject.toml
├── pyproject.toml
└── uv.lock
```

With `lib-1` and `lib-2` listed as dependencies in the root `pyproject.toml`:
```
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "lib-1",
    "lib-2",
]

[tool.uv.workspace]
members = ["apps/lib-1", "apps/lib-2"]

[tool.uv.sources]
lib-1 = { workspace = true }
lib-2 = { workspace = true }
```

With a similar layout, `poetry export` will include the sub-packages as path dependencies in the generated requirements file.  I have a Docker workflow that uses this to install packages into a directory using `pip install --target`. However, `uv export` does not do this, it only includes the dependencies of the workspace members.

I'm not sure if this is intended behaviour - the documentation for [--no-emit-workspace](https://docs.astral.sh/uv/reference/cli/#uv-export) seems to imply that the workspace packages themselves should be included, not just the dependencies.





---

_Renamed from "`uv export` does not include workspace dependencies" to "`uv export` does not include workspace packages" by @brownj85 on 2024-12-28 22:15_

---

_Comment by @charliermarsh on 2024-12-29 00:30_

Do your packages have build systems defined? Otherwise, they're considered virtual: https://docs.astral.sh/uv/concepts/projects/config/#build-systems

---

_Label `question` added by @charliermarsh on 2024-12-29 00:30_

---

_Comment by @brownj85 on 2024-12-30 20:09_

Yes, that works! Thanks

---

_Closed by @brownj85 on 2024-12-30 20:09_

---
