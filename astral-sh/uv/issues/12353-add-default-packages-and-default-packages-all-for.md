---
number: 12353
title: "Add `default-packages = [...]` and `default-packages = \"all\"` for workspaces"
type: issue
state: open
author: zanieb
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-03-20T22:43:51Z
updated_at: 2025-03-20T22:44:47Z
url: https://github.com/astral-sh/uv/issues/12353
synced_at: 2026-01-10T01:25:18Z
---

# Add `default-packages = [...]` and `default-packages = "all"` for workspaces

---

_Issue opened by @zanieb on 2025-03-20 22:43_

We should consider allowing setting the default sync packages as we do for dependency groups.

People should be able to set the default sync packages without adding them as explicit project dependencies. I guess they could add them to a dependency group, but that doesn't address the `all` case. `default-packages = "all"` would be equivalent to `--all-packages`.

Related 

- #10934
- https://github.com/astral-sh/uv/issues/4730
- https://github.com/astral-sh/uv/issues/12130

---

_Comment by @zanieb on 2025-03-20 22:43_

cc @@Tremeschin

---

_Label `configuration` added by @zanieb on 2025-03-20 22:44_

---

_Label `needs-decision` added by @zanieb on 2025-03-20 22:44_

---

_Referenced in [astral-sh/uv#12130](../../astral-sh/uv/issues/12130.md) on 2025-03-20 22:45_

---
