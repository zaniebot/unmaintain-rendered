---
number: 16136
title: "`tool.uv.package = false` complains about missing `project.requires-python`"
type: issue
state: open
author: konstin
labels:
  - error messages
assignees: []
created_at: 2025-10-06T09:32:29Z
updated_at: 2025-10-06T23:18:10Z
url: https://github.com/astral-sh/uv/issues/16136
synced_at: 2026-01-07T13:12:19-06:00
---

# `tool.uv.package = false` complains about missing `project.requires-python`

---

_Issue opened by @konstin on 2025-10-06 09:32_

First reported in https://github.com/astral-sh/uv/issues/10204#issuecomment-3369249174.

`tool.uv.package = false` complains about missing `project.requires-python` even though in this case, we have a `requires-python` value, it's just on the group instead of on the project.

```
$ uv sync --no-build
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.13`.
```

```toml
[tool.uv]
package = false

[tool.uv.dependency-groups.docs]
requires-python = ">=3.13"

[dependency-groups]
docs = [
  "numpy>=2",
]
```

This belongs to category of better `[project]`-less project support.


---

_Label `error messages` added by @konstin on 2025-10-06 09:32_

---

_Referenced in [astral-sh/uv#10204](../../astral-sh/uv/issues/10204.md) on 2025-10-06 09:33_

---

_Comment by @zanieb on 2025-10-06 23:18_

Also discussed in https://github.com/astral-sh/uv/pull/14113#discussion_r2152853769

---
