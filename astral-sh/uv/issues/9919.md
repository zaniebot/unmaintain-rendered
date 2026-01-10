```yaml
number: 9919
title: "`uv publish` cannot find index when configured as `explicit=true`"
type: issue
state: closed
author: CarrotManMatt
labels:
  - bug
assignees: []
created_at: 2024-12-15T18:59:08Z
updated_at: 2024-12-17T08:56:17Z
url: https://github.com/astral-sh/uv/issues/9919
synced_at: 2026-01-10T04:36:21Z
```

# `uv publish` cannot find index when configured as `explicit=true`

---

_Issue opened by @CarrotManMatt on 2024-12-15 18:59_

A `pyproject.toml` that creates this issue is as follows:
```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[project]
name= "My-Test-Project"
version = "0.1.0"

[tool.uv]
trusted-publishing = "always"

[[tool.uv.index]]
explicit = true
name = "testpypi"
publish-url = "https://test.pypi.org/legacy"
url = "https://test.pypi.org/simple"
```

Running `uv publish --index testpypi` results in the following message:
```text
warning: `uv publish` is experimental and may change without warning
error: No indexes were found, can't use index: `testpypi`
```

---

_Renamed from "`uv publish` cannot find index when configured as `excplicit=true`" to "`uv publish` cannot find index when configured as `explicit=true`" by @CarrotManMatt on 2024-12-15 18:59_

---

_Label `bug` added by @zanieb on 2024-12-15 21:03_

---

_Assigned to @konstin by @zanieb on 2024-12-15 21:03_

---

_Closed by @konstin on 2024-12-16 10:40_

---

_Comment by @CarrotManMatt on 2024-12-17 08:56_

Thank you for the quick fix! I barely had time to notice my issue had been closed💚💚

---
