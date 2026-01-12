```yaml
number: 9094
title: "Allow `matplotlib.use` calls to intersperse imports"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/matplotlib
created_at: 2023-12-11T15:31:24Z
updated_at: 2023-12-11T17:13:23Z
url: https://github.com/astral-sh/ruff/pull/9094
synced_at: 2026-01-12T15:55:27Z
```

# Allow `matplotlib.use` calls to intersperse imports

---

_@charliermarsh_

This PR allows `matplotlib.use` calls to intersperse imports without triggering `E402`. This is a pragmatic choice as it's common to require `matplotlib.use` calls prior to importing from within `matplotlib` itself.

Closes https://github.com/astral-sh/ruff/issues/9091.

---

_@konstin approved on 2023-12-11 15:37_

Sounds like the right solution to me

---

_Comment by @github-actions[bot] on 2023-12-11 15:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @charliermarsh on 2023-12-11 17:01_

---

_Label `rule` added by @charliermarsh on 2023-12-11 17:01_

---

_Merged by @charliermarsh on 2023-12-11 17:06_

---

_Closed by @charliermarsh on 2023-12-11 17:06_

---

_Branch deleted on 2023-12-11 17:06_

---
