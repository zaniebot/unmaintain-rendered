```yaml
number: 15443
title: "[`flake8-bandit`] Check for `builtins` instead of `builtin` (`S102`, `PTH123`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: S102
created_at: 2025-01-12T16:49:45Z
updated_at: 2025-01-13T00:56:18Z
url: https://github.com/astral-sh/ruff/pull/15443
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-bandit`] Check for `builtins` instead of `builtin` (`S102`, `PTH123`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-12 16:49_

## Summary

Resolves #15442.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-12 16:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2025-01-12 23:19_

Should these rules be renamed as well?

---

_@charliermarsh approved on 2025-01-13 00:45_

Thanks!

---

_Label `bug` added by @charliermarsh on 2025-01-13 00:45_

---

_Merged by @charliermarsh on 2025-01-13 00:45_

---

_Closed by @charliermarsh on 2025-01-13 00:45_

---

_Branch deleted on 2025-01-13 00:56_

---
