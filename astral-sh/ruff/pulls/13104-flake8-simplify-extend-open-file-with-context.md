```yaml
number: 13104
title: "[`flake8-simplify`] - extend open-file-with-context-handler to work with dbm.sqlite3 (`SIM115`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-sim115-for-py3.13
created_at: 2024-08-26T04:57:16Z
updated_at: 2024-08-26T07:11:03Z
url: https://github.com/astral-sh/ruff/pull/13104
synced_at: 2026-01-10T21:38:32Z
```

# [`flake8-simplify`] - extend open-file-with-context-handler to work with dbm.sqlite3 (`SIM115`)

---

_Pull request opened by @diceroll123 on 2024-08-26 04:57_

## Summary

Adds upcoming `dbm.sqlite3` to rule that suggests using context managers to open things with.

See: https://docs.python.org/3.13/library/dbm.html#module-dbm.sqlite3

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-08-26 05:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-26 07:10_

Nice!

---

_Label `rule` added by @AlexWaygood on 2024-08-26 07:10_

---

_Label `preview` added by @AlexWaygood on 2024-08-26 07:10_

---

_Merged by @AlexWaygood on 2024-08-26 07:11_

---

_Closed by @AlexWaygood on 2024-08-26 07:11_

---
