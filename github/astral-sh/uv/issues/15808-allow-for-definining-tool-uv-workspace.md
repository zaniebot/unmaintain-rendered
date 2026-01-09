---
number: 15808
title: "Allow for definining `[tool.uv.workspace.dependencies]` or use the versions from the project root instead, so there can be workspace managed dependencies."
type: issue
state: closed
author: sylbeth
labels:
  - enhancement
assignees: []
created_at: 2025-09-12T12:36:57Z
updated_at: 2025-09-12T13:33:17Z
url: https://github.com/astral-sh/uv/issues/15808
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow for definining `[tool.uv.workspace.dependencies]` or use the versions from the project root instead, so there can be workspace managed dependencies.

---

_Issue opened by @sylbeth on 2025-09-12 12:36_

### Summary

As it stands right now, one cannot use UV to automagically infer the version from the workspace root. In that case, I suggest one of two possible solutions. Either to define them for the project root package in either dependencies or dependency-groups, or to make it in `uv.tool.workspace.dependencies`. In either case, to use them, one can specify either a dependency or a group dependency in a workspace member and add to `[tool.uv.sources]` `.workspace = true` or `.workspace-dep = true`, any one goes (maybe workspace members are automatically added to workspace dependencies, not to break compatibility).

### Example

In this example I'll show a workspace made of the root and a package named `package`. The root package needs numpy and ruff as a dev dependency, whilst `package` needs numpy, scipy and ruff as a dev dependency. Both definition of dependency methods allow for defining dependencies the root package does not need to use.

# Definition 1
```toml
# ./pyproject.toml
[package]
dependencies = ["numpy"]

[dependency-groups]
dev = ["ruff"]

[tool.uv.sources]
ruff.workspace-dep = true
numpy.workspace-dep = true

[tool.uv.workspace]
members = ["project"]
dependencies = [
    "numpy>=2.3.3",
    "scipy>=1.16.2",
    "ruff>=0.12.7",
]
```

# Definition 2
```toml
# ./pyproject.toml
[package]
dependencies = ["numpy>=2.3.3"]

[dependency-groups]
dev = ["ruff>=0.12.7"]
workspace = ["scipy>=1.16.2"]

[tool.uv.workspace]
members = ["project"]
```


# Usage
```toml
# ./project/pyproject.toml
[project]
dependencies = ["numpy", "scipy"]

[dependency-groups]
dev = ["ruff"]

[tool.uv.sources]
scipy.workspace-dep = true
ruff.workspace-dep = true
numpy.workspace-dep = true
```

---

_Label `enhancement` added by @sylbeth on 2025-09-12 12:36_

---

_Renamed from "[workspace.dependencies] or something similar" to "Allow for definining `[tool.uv.workspace.dependencies]` or use the versions from the project root instead, so there can be workspace managed dependencies." by @sylbeth on 2025-09-12 12:40_

---

_Comment by @sylbeth on 2025-09-12 12:49_

I could try and write a PR but I don't have the knowledge of all the working pieces needed for this configuration. I suppose, if the first option was chosen as the best one, I would only have to change the configuration to allow for `tool.uv.workspace.dependencies`, and change the code for `tool.uv.sources`, but I don't know the location of neither of those.

---

_Comment by @zanieb on 2025-09-12 13:05_

This is a duplicate of https://github.com/astral-sh/uv/issues/8949

---

_Closed by @sylbeth on 2025-09-12 13:33_

---
