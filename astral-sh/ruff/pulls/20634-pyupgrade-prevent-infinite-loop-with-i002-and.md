```yaml
number: 20634
title: "[`pyupgrade`] Prevent infinite loop with `I002` and `UP026`"
type: pull_request
state: merged
author: IDrokin117
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: feature/gh-20601
created_at: 2025-09-29T17:48:47Z
updated_at: 2025-09-30T21:11:35Z
url: https://github.com/astral-sh/ruff/pull/20634
synced_at: 2026-01-12T15:57:06Z
```

# [`pyupgrade`] Prevent infinite loop with `I002` and `UP026`

---

_@IDrokin117_

## Summary
Closes #20601

Do not treat imports as unused for the rule [unnecessary-builtin-import (UP029)](https://docs.astral.sh/ruff/rules/unnecessary-builtin-import/) if they are required by `isort`([missing-required-import](https://docs.astral.sh/ruff/rules/missing-required-import/))

## Test Plan
- Added test case `i002_up029_conflict` to ensure there is no conflict


---

_Comment by @github-actions[bot] on 2025-09-29 17:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-09-30 21:04_

---

_Label `fixes` added by @ntBre on 2025-09-30 21:04_

---

_@ntBre approved on 2025-09-30 21:11_

Looks great, thank you!

---

_Merged by @ntBre on 2025-09-30 21:11_

---

_Closed by @ntBre on 2025-09-30 21:11_

---
