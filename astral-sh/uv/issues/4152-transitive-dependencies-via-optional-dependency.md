```yaml
number: 4152
title: Transitive dependencies via optional dependency aliases fails URL installation
type: issue
state: closed
author: ringohoffman
labels:
  - bug
assignees: []
created_at: 2024-06-07T22:57:00Z
updated_at: 2024-06-08T01:10:19Z
url: https://github.com/astral-sh/uv/issues/4152
synced_at: 2026-01-12T15:58:48Z
```

# Transitive dependencies via optional dependency aliases fails URL installation

---

_@ringohoffman_

Continuing my report from the comment here:

* https://github.com/astral-sh/uv/issues/1932#issuecomment-2155660516

I am still seeing a failure to install transitive URL dependencies when using optional dependency aliases as in:

```toml
[project]
name = "transitive_optional_dependency"
dynamic = ["version"]

[project.optional-dependencies]
all = [
    "transitive_optional_dependency[docs]",
]
docs = [
    "mike @ git+https://github.com/jimporter/mike@3f7d756#egg=mike",
]

```

I've provided a minimally reproducible repo here: https://github.com/ringohoffman/transitive-optional-dependency

With steps:

```console
$ git clone git@github.com:ringohoffman/transitive-optional-dependency.git
$ cd transitive-optional-dependency
$ conda create -n tod python=3.10 -y
$ conda activate tod
$ pip install uv
Collecting uv
  Using cached uv-0.2.9-py3-none-macosx_11_0_arm64.whl.metadata (33 kB)
Using cached uv-0.2.9-py3-none-macosx_11_0_arm64.whl (9.7 MB)
Installing collected packages: uv
Successfully installed uv-0.2.9
$ uv pip install -e .[all]
error: Package `mike` attempted to resolve via URL: git+https://github.com/jimporter/mike@3f7d756#egg=mike. URL dependencies must be expressed as direct requirements or constraints. Consider adding `mike @ git+https://github.com/jimporter/mike@3f7d756#egg=mike` to your dependencies or constraints file.
```

cc: @charliermarsh 

---

_Comment by @charliermarsh on 2024-06-07 23:11_

Ah in a recursive extra? Maybe we're missing that case, thanks.

---

_Label `bug` added by @charliermarsh on 2024-06-07 23:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-08 01:02_

---

_Closed by @charliermarsh on 2024-06-08 01:10_

---
