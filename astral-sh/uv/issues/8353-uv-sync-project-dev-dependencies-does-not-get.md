```yaml
number: 8353
title: "uv sync: project dev dependencies does not get installed"
type: issue
state: closed
author: bugzpodder
labels: []
assignees: []
created_at: 2024-10-19T00:35:39Z
updated_at: 2024-10-19T05:49:43Z
url: https://github.com/astral-sh/uv/issues/8353
synced_at: 2026-01-12T15:59:24Z
```

# uv sync: project dev dependencies does not get installed

---

_@bugzpodder_

prior to uv 0.4.9, if i do `uv sync` in workspace, both dev-dependencies in worspace and project gets installed.  after uv 0.4.10+ `uv sync` in workspace removes project dev-dependencies

root workspace pyproject.yaml
```
[project]
name = "workspace"
version = "0.1.0"
dependencies = ["project"]

[tool.uv.sources]
project = { workspace = true }

[tool.uv.workspace]
members = ["project"]

[tool.uv]
package=false
dev-dependencies = [
    "coverage==7.5.3",
]
```

project pyproject.yaml
```
[project]
name = "project"
version = "0.1.0"
description = ""
dependencies = [
    ...
]

[tool.uv]
dev-dependencies = [
    "basedpyright==1.17.0"
]

[tools.uv]
package=false

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["workspace"]
```

---

_Renamed from "dev dependencies" to "two layers of dev dependencies does not get installed" by @bugzpodder on 2024-10-19 00:36_

---

_Renamed from "two layers of dev dependencies does not get installed" to "uv sync: project dev dependencies does not get installed" by @bugzpodder on 2024-10-19 00:40_

---

_Comment by @FishAlchemist on 2024-10-19 03:28_

I believe this is related to modification https://github.com/astral-sh/uv/pull/7318. 
The PR and its related issue should provide you with more details.

**Please note that I am not a member of the UV team, so I can only provide PR that I believe are relevant for your reference. If you have any other ideas, I am not in a position to make decisions.**

---

_Comment by @zanieb on 2024-10-19 04:00_

That sounds correct, thanks @FishAlchemist.

cc @charliermarsh 

---

_Comment by @bugzpodder on 2024-10-19 05:49_

closing as a dupe of #7541.  thanks for the quick feedback guys

---

_Closed by @bugzpodder on 2024-10-19 05:49_

---
