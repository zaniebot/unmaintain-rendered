```yaml
number: 16149
title: "Don't warn when dependency is constraint by other dependency"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/no_warning_without_and_with_lower_bound
created_at: 2025-10-07T08:40:11Z
updated_at: 2025-10-07T15:59:03Z
url: https://github.com/astral-sh/uv/pull/16149
synced_at: 2026-01-10T06:36:15Z
```

# Don't warn when dependency is constraint by other dependency

---

_Pull request opened by @konstin on 2025-10-07 08:40_

Currently, `uv lock --resolution lowest-direct` warns above the setup below, as we visit the unbounded `anyio[trio]` first.

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "anyio[trio]",
    "anyio>=4"
]
```


---

_Label `error messages` added by @konstin on 2025-10-07 08:40_

---

_@zanieb approved on 2025-10-07 14:50_

---

_Merged by @konstin on 2025-10-07 15:59_

---

_Closed by @konstin on 2025-10-07 15:59_

---

_Branch deleted on 2025-10-07 15:59_

---
