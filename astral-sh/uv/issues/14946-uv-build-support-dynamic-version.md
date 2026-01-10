---
number: 14946
title: "[`uv_build`] support `dynamic = [\"version\"]`"
type: issue
state: closed
author: chirizxc
labels:
  - enhancement
assignees: []
created_at: 2025-07-28T20:37:32Z
updated_at: 2025-07-28T20:54:08Z
url: https://github.com/astral-sh/uv/issues/14946
synced_at: 2026-01-10T01:25:50Z
---

# [`uv_build`] support `dynamic = ["version"]`

---

_Issue opened by @chirizxc on 2025-07-28 20:37_

### Summary

```toml
[project]
dynamic = ["version"]
```

Will there be support for dynamic versioning, e.g. so that it is taken from GitHub tags like [hatch-vcs](https://pypi.org/project/hatch-vcs/)?

```toml
[build-system]
requires = ["hatchling", "hatch-vcs"]
build-backend = "hatchling.build"

[tool.hatch.version]
source = "vcs"

[tool.hatch.version.vcs]
tag-pattern = "*v"

[project]
dynamic = ["version"]
```



### Example
```toml
[build-system]
requires = ["uv_build>=0.8.3,<0.9.0"]
build-backend = "uv_build"

[tool.uv.build-backend.version]
source = "vcs"
tag-pattern = "*v"

[project]
dynamic = ["version"]
```

---

_Label `enhancement` added by @chirizxc on 2025-07-28 20:37_

---

_Comment by @zanieb on 2025-07-28 20:54_

See

- https://github.com/astral-sh/uv/issues/14037
- https://github.com/astral-sh/uv/issues/8714

---

_Closed by @zanieb on 2025-07-28 20:54_

---
