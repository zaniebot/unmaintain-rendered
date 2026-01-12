```yaml
number: 5251
title: "Should `uv init` create nested workspaces?"
type: issue
state: closed
author: j178
labels:
  - bug
  - good first issue
  - preview
assignees: []
created_at: 2024-07-20T16:06:05Z
updated_at: 2024-07-22T23:48:41Z
url: https://github.com/astral-sh/uv/issues/5251
synced_at: 2026-01-12T15:58:55Z
```

# Should `uv init` create nested workspaces?

---

_@j178_

```sh
$ uv init work
Initialized project work in /tmp/work
$ uv init work/pkg-a
Adding pkg-a as member of workspace /tmp/work
Initialized project pkg-a in /tmp/work/pkg-a
$ uv init work/pkg-a/pkg-inside-a
Adding pkg-inside-a as member of workspace /tmp/work
Initialized project pkg-inside-a in /tmp/work/pkg-a/pkg-inside-a
```

Content of `work/pyproject.toml`:
```toml
[project]
name = "work"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []

[tool.uv]
dev-dependencies = []

[tool.uv.workspace]
members = ["pkg-a"]
```

Content of `work/pkg-a/pyproject.toml`:
```toml
[project]
name = "pkg-a"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []

[tool.uv]
dev-dependencies = []

[tool.uv.workspace]
members = ["pkg-inside-a"]
```

The message from `uv init work/pkg-a/pkg-inside-a` indicates `Adding pkg-inside-a as member of workspace /tmp/work`. However it seems to be added to `/tmp/work/pkg-inside-a`, essentially  creating a workspace inside a workspace. Do we allow a workspace member itself being a workspace?

---

_Label `bug` added by @konstin on 2024-07-22 11:39_

---

_Comment by @konstin on 2024-07-22 11:40_

That's a bug, we should add `pkg-inside-a` to the workspace root at `work/pyproject.toml` and error when there's a workspace declaration in a workspace member.

---

_Label `good first issue` added by @konstin on 2024-07-22 11:40_

---

_Label `preview` added by @konstin on 2024-07-22 11:41_

---

_Closed by @charliermarsh on 2024-07-22 23:48_

---
