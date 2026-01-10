```yaml
number: 14345
title: "[`pylint`] Remove check for dot in alias name in `useless-import-alias (PLC0414)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: isort-followup
created_at: 2024-11-14T21:48:21Z
updated_at: 2024-11-17T14:28:07Z
url: https://github.com/astral-sh/ruff/pull/14345
synced_at: 2026-01-10T20:50:57Z
```

# [`pylint`] Remove check for dot in alias name in `useless-import-alias (PLC0414)`

---

_Pull request opened by @dylwil3 on 2024-11-14 21:48_

Follow-up to #14287 : when checking that `name` is the same as `as_name` in `import name as as_name`, we do not need to first do an early return if `'.'` is found in `name`.


---

_Comment by @github-actions[bot] on 2024-11-14 22:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2024-11-14 22:26_

---

_Closed by @dylwil3 on 2024-11-14 22:26_

---

_Branch deleted on 2024-11-14 22:26_

---

_Label `internal` added by @dhruvmanila on 2024-11-15 10:56_

---

_Branch restored on 2024-11-17 14:28_

---
