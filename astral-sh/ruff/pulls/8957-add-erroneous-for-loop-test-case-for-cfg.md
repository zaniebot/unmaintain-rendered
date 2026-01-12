```yaml
number: 8957
title: Add erroneous for-loop test case for CFG
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cfg
created_at: 2023-12-01T23:04:16Z
updated_at: 2023-12-01T23:17:34Z
url: https://github.com/astral-sh/ruff/pull/8957
synced_at: 2026-01-12T15:55:27Z
```

# Add erroneous for-loop test case for CFG

---

_@charliermarsh_

_No description provided._

---

_@charliermarsh reviewed on 2023-12-01 23:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/snapshots/ruff_linter__rules__ruff__rules__unreachable__tests__for.py.md.snap`:267 on 2023-12-01 23:06_

`block0` here (`pass`)` should point to `block2` (`for`).

In the next example, you can see that `block1` (`pass`) gets pointed to `black3` (`for`). But the structures are identical. It has to do with the lack of a statement following the loop.

---

_Label `internal` added by @charliermarsh on 2023-12-01 23:06_

---

_Merged by @charliermarsh on 2023-12-01 23:11_

---

_Closed by @charliermarsh on 2023-12-01 23:11_

---

_Branch deleted on 2023-12-01 23:11_

---

_Comment by @github-actions[bot] on 2023-12-01 23:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
