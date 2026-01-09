---
number: 5650
title: Allow top level dev decencies in top level workspace
type: issue
state: closed
author: jpambrun-vida
labels:
  - enhancement
  - projects
  - preview
assignees: []
created_at: 2024-07-31T13:39:06Z
updated_at: 2024-08-01T21:04:31Z
url: https://github.com/astral-sh/uv/issues/5650
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow top level dev decencies in top level workspace

---

_Issue opened by @jpambrun-vida on 2024-07-31 13:39_

I feel like the workspace top level virtual workspace would be the ideal place to have dev dependencies such as mypy, pytest, etc.

```toml
[tool.ruff]
line-length = 120

[tool.mypy]
ignore_missing_imports = true

[tool.uv.workspace]
members = [
  "a/b",
  "c/d",
]

[tool.uv]
dev-dependencies = [
  "mypy~=1.11.0",
  "pytest~=8.3.2",
  "coverage~=7.6.0",
]
```

As of now, uv seems to ignore them.

---

_Label `projects` added by @zanieb on 2024-07-31 13:40_

---

_Label `enhancement` added by @zanieb on 2024-07-31 13:53_

---

_Comment by @zanieb on 2024-07-31 13:54_

👍 yeah we should support this

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-31 14:21_

---

_Label `preview` added by @charliermarsh on 2024-07-31 14:21_

---

_Comment by @charliermarsh on 2024-07-31 16:10_

This looks more difficult than I thought, since we don't have an entry for the virtual root in the lockfile. I'll figure something out though...

---

_Comment by @charliermarsh on 2024-08-01 17:08_

Ok, working on this one now, but not sure how to solve it yet.

---

_Referenced in [astral-sh/uv#5709](../../astral-sh/uv/pulls/5709.md) on 2024-08-01 20:12_

---

_Closed by @charliermarsh on 2024-08-01 21:04_

---

_Closed by @charliermarsh on 2024-08-01 21:04_

---
