```yaml
number: 19539
title: "[`perflint`] Ignore rule if target is `global` or `nonlocal` (`PERF401`)"
type: pull_request
state: merged
author: IDrokin117
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/ruff-19531
created_at: 2025-07-24T19:39:13Z
updated_at: 2025-07-28T21:03:23Z
url: https://github.com/astral-sh/ruff/pull/19539
synced_at: 2026-01-10T17:58:13Z
```

# [`perflint`] Ignore rule if target is `global` or `nonlocal` (`PERF401`)

---

_Pull request opened by @IDrokin117 on 2025-07-24 19:39_

## Summary

Resolves #19531

I've implemented a check to determine whether the for_stmt target is declared as global or nonlocal. I believe we should skip the rule in all such cases, since variables declared this way are intended for use outside the loop scope, making value changes expected behavior.

## Test Plan

Added two test cases for global and nonlocal variable to snapshot.


---

_Comment by @github-actions[bot] on 2025-07-24 19:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-24 20:42_

---

_Label `bug` added by @ntBre on 2025-07-24 20:42_

---

_Label `fixes` added by @ntBre on 2025-07-24 20:42_

---

_Renamed from "[`perflint `]ignore rule if target is global or nonlocal (`PERF401`)" to "[`perflint`]ignore rule if target is global or nonlocal (`PERF401`)" by @IDrokin117 on 2025-07-25 06:59_

---

_@ntBre approved on 2025-07-28 21:02_

Looks great, thank you!

---

_Renamed from "[`perflint`]ignore rule if target is global or nonlocal (`PERF401`)" to "[`perflint`] Ignore rule if target is `global` or `nonlocal` (`PERF401`)" by @ntBre on 2025-07-28 21:02_

---

_Merged by @ntBre on 2025-07-28 21:03_

---

_Closed by @ntBre on 2025-07-28 21:03_

---
