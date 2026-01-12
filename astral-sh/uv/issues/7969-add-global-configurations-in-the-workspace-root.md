```yaml
number: 7969
title: "Add \"global\" configurations in the workspace root instead of each member"
type: issue
state: open
author: inbalzelig-ugi
labels: []
assignees: []
created_at: 2024-10-07T12:59:21Z
updated_at: 2024-10-07T12:59:21Z
url: https://github.com/astral-sh/uv/issues/7969
synced_at: 2026-01-12T15:59:18Z
```

# Add "global" configurations in the workspace root instead of each member

---

_@inbalzelig-ugi_

Hi, I have a workspace with many members (the workspace itself is just a wrapper and doesn't have any content). I want to add the same dev dependencies to all members (e.g. pytest, ruff). Can we do that in the root workspace pyproject.toml instead of adding the same dev dependencies in each member?
Also, I will want to add ruff configurations to the pyproject.toml. It will be much easier to do it once for the whole project instead of copying the same configuration for each member.

Thanks

---
