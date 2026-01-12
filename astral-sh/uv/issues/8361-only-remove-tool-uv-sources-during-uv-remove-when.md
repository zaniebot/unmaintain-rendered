```yaml
number: 8361
title: "Only remove `[tool.uv.sources]` during `uv remove` when no longer relevant"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-10-19T13:24:29Z
updated_at: 2024-10-19T15:52:02Z
url: https://github.com/astral-sh/uv/issues/8361
synced_at: 2026-01-12T15:59:24Z
```

# Only remove `[tool.uv.sources]` during `uv remove` when no longer relevant

---

_@zanieb_

e.g., as demonstrated by https://github.com/astral-sh/uv/pull/8360 we remove the sources entry on the first `uv remove` call even though the dependency is still required in the `optional-dependencies` and `dev-dependency` sections.

---

_Label `bug` added by @zanieb on 2024-10-19 13:24_

---

_Label `help wanted` added by @zanieb on 2024-10-19 13:24_

---

_Closed by @zanieb on 2024-10-19 15:52_

---

_Closed by @zanieb on 2024-10-19 15:52_

---
