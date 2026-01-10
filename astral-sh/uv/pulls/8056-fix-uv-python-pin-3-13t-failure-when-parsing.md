```yaml
number: 8056
title: "Fix `uv python pin 3.13t` failure when parsing version for project requires check"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-t-check
created_at: 2024-10-09T19:56:13Z
updated_at: 2024-10-10T00:17:21Z
url: https://github.com/astral-sh/uv/pull/8056
synced_at: 2026-01-10T12:54:02Z
```

# Fix `uv python pin 3.13t` failure when parsing version for project requires check

---

_Pull request opened by @zanieb on 2024-10-09 19:56_

Closes https://github.com/astral-sh/uv/issues/7964

We can probably do some restructuring to avoid unwrapping here in the future, but this just fixes the bug quick.

---

_Label `bug` added by @zanieb on 2024-10-09 19:56_

---

_@charliermarsh reviewed on 2024-10-09 22:26_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1960 on 2024-10-09 22:26_

Nit: brief docstring please

---

_@charliermarsh approved on 2024-10-09 22:26_

---

_Merged by @zanieb on 2024-10-10 00:17_

---

_Closed by @zanieb on 2024-10-10 00:17_

---

_Branch deleted on 2024-10-10 00:17_

---
