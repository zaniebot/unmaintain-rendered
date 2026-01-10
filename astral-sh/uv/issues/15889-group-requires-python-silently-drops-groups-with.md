---
number: 15889
title: "Group `requires-python` silently drops groups with `uv pip compile --group`"
type: issue
state: open
author: konstin
labels:
  - error messages
assignees: []
created_at: 2025-09-16T09:24:12Z
updated_at: 2025-09-16T13:17:38Z
url: https://github.com/astral-sh/uv/issues/15889
synced_at: 2026-01-10T01:26:01Z
---

# Group `requires-python` silently drops groups with `uv pip compile --group`

---

_Issue opened by @konstin on 2025-09-16 09:24_

**pyproject.toml**
```toml
[tool.uv.dependency-groups.ml1]
requires-python = ">=3.14"

[tool.uv.dependency-groups.ml2]
requires-python = ">=3.12"

[dependency-groups]
ml1 = ["tqdm"]
ml2 = ["tqdm"]
```

Running `uv-debug pip compile --group pyproject.toml:ml1` in this scenario emits an empty group when Python 3.13 is active. This is technically correct: Under Python 3.13, this group should be skipped. It's confusing to users though since we're ignoring the group they just explicitly selected, the user most likely didn't intend to create an empty lockfile.

---

_Label `error messages` added by @konstin on 2025-09-16 09:24_

---

_Renamed from "`tool.uv.dependency-groups.{group}.requires-python` silently drops groups with `uv pip compile --group`" to "Grouo `requires-python` silently drops groups with `uv pip compile --group`" by @konstin on 2025-09-16 09:24_

---

_Renamed from "Grouo `requires-python` silently drops groups with `uv pip compile --group`" to "Group `requires-python` silently drops groups with `uv pip compile --group`" by @konstin on 2025-09-16 09:40_

---

_Comment by @zanieb on 2025-09-16 13:17_

Hm yeah I think we should error in the same way we do for projects.

---

_Referenced in [astral-sh/uv#16089](../../astral-sh/uv/pulls/16089.md) on 2025-10-01 17:36_

---
