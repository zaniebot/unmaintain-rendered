```yaml
number: 9322
title: Remove source path from parser errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/source-path
created_at: 2023-12-30T20:10:35Z
updated_at: 2023-12-30T20:43:45Z
url: https://github.com/astral-sh/ruff/pull/9322
synced_at: 2026-01-10T23:07:18Z
```

# Remove source path from parser errors

---

_Pull request opened by @charliermarsh on 2023-12-30 20:10_

## Summary

I always found it odd that we had to pass this in, since it's really higher-level context for the error. The awkwardness is further evidenced by the fact that we pass in fake values everywhere (even outside of tests). The source path isn't actually used to display the error; it's only accessed elsewhere to _re-display_ the error in certain cases. This PR modifies to instead pass the path directly in those cases.

---

_Label `internal` added by @charliermarsh on 2023-12-30 20:11_

---

_Marked ready for review by @charliermarsh on 2023-12-30 20:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/logging.rs`:165 on 2023-12-30 20:13_

Only place the path is actually used. Instead, we thread it through here.

---

_@charliermarsh reviewed on 2023-12-30 20:13_

---

_Merged by @charliermarsh on 2023-12-30 20:33_

---

_Closed by @charliermarsh on 2023-12-30 20:33_

---

_Branch deleted on 2023-12-30 20:33_

---

_Comment by @github-actions[bot] on 2023-12-30 20:43_

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
