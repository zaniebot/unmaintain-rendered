```yaml
number: 2376
title: project recognition prefers parent over child
type: issue
state: closed
author: dorschw
labels:
  - configuration
assignees: []
created_at: 2026-01-07T14:52:49Z
updated_at: 2026-01-07T15:49:43Z
url: https://github.com/astral-sh/ty/issues/2376
synced_at: 2026-01-10T01:56:41Z
```

# project recognition prefers parent over child

---

_Issue opened by @dorschw on 2026-01-07 14:52_

### Summary

given this project structure
```
├── README.md
├── bar
│   ├── README.md
│   ├── main.py
│   └── pyproject.toml
├── main.py
└── pyproject.toml
```
With this being `foo/pyproject.toml`
```
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.14"
dependencies = []

[tool.uv.workspace]
members = [
    "bar",
]

[tool.ty.src]
exclude=["bar/**"] // <<<<<< NOTE THIS
```

when running `uvx ty check .` or `uv run ty check .` **in the `bar` folder**, all `bar` files are ignored. 
```
WARN No python files found under the given path(s)
All checks passed!
```

### Version

ty 0.0.9

---

_Label `configuration` added by @AlexWaygood on 2026-01-07 15:00_

---

_Comment by @MichaReiser on 2026-01-07 15:41_

is `foo` the outer directory? 

If so, `ty` searches for the closest `ty.toml` or `pyproject.toml` with a `tool.ty` section and uses that configuration. If you want `bar` to be it's own ty project, add an empty `[tool.ty]` section, so that ty knows, hey, don't search any further, this is the root of the project.

---

_Comment by @dorschw on 2026-01-07 15:49_

yes, `foo` is the outer one (sorry it didn't show in the tree) 

and gotcha, thanks. missed that in the docs.

---

_Closed by @dorschw on 2026-01-07 15:49_

---
