---
number: 8702
title: Overwrite pyproject when workspaces have same normalized name
type: issue
state: open
author: juan-abia
labels:
  - question
assignees: []
created_at: 2024-10-30T15:42:00Z
updated_at: 2024-10-31T14:19:27Z
url: https://github.com/astral-sh/uv/issues/8702
synced_at: 2026-01-07T13:12:18-06:00
---

# Overwrite pyproject when workspaces have same normalized name

---

_Issue opened by @juan-abia on 2024-10-30 15:42_

I have the following folder structure:

```
/private/tmp/super-project git:[main]
tree
.
├── README.md
├── pyproject.toml
├── super_project
│   ├── README.md
│   └── pyproject.toml
└── uv.lock
```

This is a common use case in my company. The repo name is `super-project` and the name of the python package we publish is `super_project`. 

This is the content of the root pyproject.toml:
```
[project]
name = "super-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "super_project"
]

[tool.uv.workspace]
members = ["super_project"]

[tool.uv.sources]
super_project = { workspace = true }

[group-dependencies]
dev = [
    "pytest >=7.2.2",
]
```

This is the pyproject of the package:
```
[project]
name = "super_project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "matplotlib"
]
```


When I run `uv run pytest` y get the next error:
```
error: Failed to spawn: `pytest`
  Caused by: No such file or directory (os error 2
```

And worse. If I try to do uv add pytest, my root pyproject gets overwriten with the pyproject of the package.

I guess the issue is related to https://github.com/astral-sh/uv/issues/8591




---

_Comment by @CarlosRicGuerrero on 2024-10-30 16:06_

I got the same issue too. 

---

_Comment by @alex47ST3 on 2024-10-30 16:12_

same problem here as well

---

_Comment by @zanieb on 2024-10-30 16:18_

I don't think you can have an outer and inner `pyproject.toml` with the same name — those names must be unique when normalized and we must normalize them for comparison per the Python standards.



---

_Label `question` added by @zanieb on 2024-10-30 16:18_

---

_Comment by @zanieb on 2024-10-30 16:18_

Can you explain more about how it's overwritten? And why you're using this structure in the first place?

---

_Comment by @charliermarsh on 2024-10-30 16:45_

Yeah that's not a valid structure. It's like saying you want two packages on PyPI called `foo_bar` and `foo-bar`, and you want to be able to use them distinctly.

---

_Comment by @juan-abia on 2024-10-31 09:26_

So for this example:
```
/private/tmp/super-project git:[main]
tree
.
├── super_project
│   ├── README.md
│   └── pyproject.toml
├── super_project_2
│   ├── README.md
│   └── pyproject.toml
├── README.md
├── pyproject.toml
└── uv.lock
```

I want only two packages published to Pypi: `super-project` and `super-project-2`.

I'd like the two folders to be uv workspaces. When I'm developing locally I'd like to install dependencies of both packages. Is there any way I can do this with uv?

---

_Comment by @juan-abia on 2024-10-31 09:28_

> Can you explain more about how it's overwritten?

With this structure, if I do `uv add --dev pytest` for example. The pyproject of the root folder gets completely overwritten by:

pyproject of `super_project` (with underscore, the package) **+** dev group dependency with pytest

---
