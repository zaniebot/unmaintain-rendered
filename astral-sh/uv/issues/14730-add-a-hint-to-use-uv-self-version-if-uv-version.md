---
number: 14730
title: "Add a hint to use `uv self version` if `uv version` cannot find a project"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2025-07-18T15:57:27Z
updated_at: 2025-07-22T13:32:46Z
url: https://github.com/astral-sh/uv/issues/14730
synced_at: 2026-01-10T01:25:48Z
---

# Add a hint to use `uv self version` if `uv version` cannot find a project

---

_Issue opened by @zanieb on 2025-07-18 15:57_

Instead of just

```
❯ uv version
error: No `pyproject.toml` found in current directory or any parent directory
```

we should show

```
❯ uv version
error: No `pyproject.toml` found in current directory or any parent directory

hint: If you meant to view uv's version, use `uv self version` instead
```

---

_Label `good first issue` added by @zanieb on 2025-07-18 15:57_

---

_Label `error messages` added by @zanieb on 2025-07-18 15:57_

---

_Assigned to @Copilot by @zanieb on 2025-07-18 18:17_

---

_Referenced in [astral-sh/uv#14738](../../astral-sh/uv/pulls/14738.md) on 2025-07-18 18:17_

---

_Referenced in [astral-sh/uv#14739](../../astral-sh/uv/pulls/14739.md) on 2025-07-18 18:22_

---

_Closed by @zanieb on 2025-07-22 13:32_

---
