```yaml
number: 10208
title: "[`pycodestyle`] Fix indentation in #10096"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - internal
assignees: []
merged: true
base: blank-lines-isort
head: hoel/blank-lines-isort-fix-indent
created_at: 2024-03-03T12:44:09Z
updated_at: 2024-03-03T18:59:07Z
url: https://github.com/astral-sh/ruff/pull/10208
synced_at: 2026-01-10T22:47:01Z
```

# [`pycodestyle`] Fix indentation in #10096

---

_Pull request opened by @hoel-bagard on 2024-03-03 12:44_

Some ifs are over-indented in #10096 as a result of refactoring, this PR simply fixes that.

---

_Comment by @github-actions[bot] on 2024-03-03 12:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2024-03-03 18:58_

---

_@MichaReiser approved on 2024-03-03 18:58_

Strange that rustfmt doesn't remove the indent automatically. Thank you

---

_Merged by @MichaReiser on 2024-03-03 18:59_

---

_Closed by @MichaReiser on 2024-03-03 18:59_

---
