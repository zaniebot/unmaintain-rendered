```yaml
number: 11645
title: Use char index rather than position for indent slice
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pos
created_at: 2024-05-31T19:01:08Z
updated_at: 2024-05-31T20:58:21Z
url: https://github.com/astral-sh/ruff/pull/11645
synced_at: 2026-01-10T21:56:00Z
```

# Use char index rather than position for indent slice

---

_Pull request opened by @charliermarsh on 2024-05-31 19:01_

## Summary

A beginner's mistake :)

Closes https://github.com/astral-sh/ruff/issues/11641.


---

_Label `bug` added by @charliermarsh on 2024-05-31 19:01_

---

_Merged by @charliermarsh on 2024-05-31 19:04_

---

_Closed by @charliermarsh on 2024-05-31 19:04_

---

_Branch deleted on 2024-05-31 19:04_

---

_Comment by @github-actions[bot] on 2024-05-31 19:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-05-31 19:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/stylist.rs`:109 on 2024-05-31 19:40_

I think you could do `line.find(|c| !c.is_whitespace())`

---

_@charliermarsh reviewed on 2024-05-31 20:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_codegen/src/stylist.rs`:109 on 2024-05-31 20:58_

Wow TIL.

---
