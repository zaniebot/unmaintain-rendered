```yaml
number: 14475
title: Only test release builds on main
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/ci-release
created_at: 2024-11-20T04:37:04Z
updated_at: 2024-11-20T04:47:47Z
url: https://github.com/astral-sh/ruff/pull/14475
synced_at: 2026-01-10T20:50:57Z
```

# Only test release builds on main

---

_Pull request opened by @zanieb on 2024-11-20 04:37_

This is one of the slowest remaining jobs in the pull request CI. We could use a larger runner for a trivial speed-up (in exchange for $$), but I don't think this is going to break often enough to merit testing on every pull request commit? It's not a required job, so I don't feel strongly about it, but it feels like a bit of a waste of compute.

Originally added in https://github.com/astral-sh/ruff/pull/11182

---

_Label `ci` added by @zanieb on 2024-11-20 04:37_

---

_Marked ready for review by @zanieb on 2024-11-20 04:39_

---

_@charliermarsh approved on 2024-11-20 04:40_

---

_Comment by @github-actions[bot] on 2024-11-20 04:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @zanieb on 2024-11-20 04:47_

---

_Closed by @zanieb on 2024-11-20 04:47_

---

_Branch deleted on 2024-11-20 04:47_

---
