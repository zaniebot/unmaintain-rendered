```yaml
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
synced_at: 2026-01-12T16:01:55Z
```

# Add a hint to use `uv self version` if `uv version` cannot find a project

---

_@zanieb_

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

_Closed by @zanieb on 2025-07-22 13:32_

---
