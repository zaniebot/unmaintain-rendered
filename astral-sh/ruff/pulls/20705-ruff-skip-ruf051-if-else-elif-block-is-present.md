```yaml
number: 20705
title: "[`ruff`] Skip `RUF051` if `else`/`elif` block is present"
type: pull_request
state: merged
author: njhearp
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: RUF051-else-branch
created_at: 2025-10-06T01:40:52Z
updated_at: 2025-10-06T13:08:47Z
url: https://github.com/astral-sh/ruff/pull/20705
synced_at: 2026-01-10T17:34:34Z
```

# [`ruff`] Skip `RUF051` if `else`/`elif` block is present

---

_Pull request opened by @njhearp on 2025-10-06 01:40_

## Summary

Fixes #20700

`else` and `elif` blocks could previously be deleted when applying a fix for this rule. If an `else` or `elif` branch is detected the rule will not trigger. So now the rule will only flag if it is safe.

---

_Comment by @github-actions[bot] on 2025-10-06 01:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-10-06 12:51_

Thank you!

---

_Label `bug` added by @dylwil3 on 2025-10-06 12:51_

---

_Label `fixes` added by @dylwil3 on 2025-10-06 12:51_

---

_Label `fixes` removed by @dylwil3 on 2025-10-06 13:01_

---

_Label `rule` added by @dylwil3 on 2025-10-06 13:01_

---

_Merged by @dylwil3 on 2025-10-06 13:02_

---

_Closed by @dylwil3 on 2025-10-06 13:02_

---

_Renamed from "[`RUF051`] Ignore if `else`/`elif` block is present" to "[`ruff`] Skip `RUF051` if `else`/`elif` block is present" by @dylwil3 on 2025-10-06 13:08_

---
