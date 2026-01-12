```yaml
number: 8735
title: Add all packages from git-backed virtual workspace as a single source
type: issue
state: open
author: mjclarke94
labels:
  - wish
assignees: []
created_at: 2024-10-31T23:18:42Z
updated_at: 2024-11-03T10:25:54Z
url: https://github.com/astral-sh/uv/issues/8735
synced_at: 2026-01-12T15:59:33Z
```

# Add all packages from git-backed virtual workspace as a single source

---

_@mjclarke94_

We are trying to centralise all of our libraries in a mono-repo, some of which are interdependent. 

```
[project]
name = "test"
version = "0.1.0"
requires-python=">=3.12"
dependencies = [
"c",
"d"
]

[tool.uv.sources]
c = { git = "https://github.com/astral-sh/workspace-virtual-root-test", subdirectory = "packages/c" }
d = { git = "https://github.com/astral-sh/workspace-virtual-root-test", subdirectory = "packages/d" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Rather than needing to add each subdirectory as a source, it would be nice to be able to add all the modules belonging to a virtual workspace as one source which exposes multiple packages.

```
[tool.uv.sources]
{ git_workspace = "https://github.com/astral-sh/workspace-virtual-root-test" }
```

EDIT: Realised that 0.42.9 fixed my original issue so turning it from a two parter into a more targeted request!

---

_Renamed from "Conflicting urls when installing interdependent packages from single virtual workspace" to "Add all packages from git-backed virtual workspace as a single source" by @mjclarke94 on 2024-10-31 23:25_

---

_Label `wish` added by @charliermarsh on 2024-11-01 13:36_

---

_Comment by @elupus on 2024-11-03 10:25_

Ran into this need aswell. Was trying to move away from hatch relative packages. But the resolver did not like just pointing to a workspace package in a git repo with sub directory packages as workspace members.

You really just want to specify the git path and revision once for the full workspace set.

---
