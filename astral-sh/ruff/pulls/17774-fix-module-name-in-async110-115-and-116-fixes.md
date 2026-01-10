```yaml
number: 17774
title: Fix module name in ASYNC110, 115, and 116 fixes
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: micha/fix-async-module-names
created_at: 2025-05-01T17:46:59Z
updated_at: 2025-05-01T21:37:11Z
url: https://github.com/astral-sh/ruff/pull/17774
synced_at: 2026-01-10T18:57:03Z
```

# Fix module name in ASYNC110, 115, and 116 fixes

---

_Pull request opened by @MichaReiser on 2025-05-01 17:46_

## Summary

The `Display` implementation incorrectly mapped `anyio` to `asyncio` and `asyncio` to `anyio`.... 

Fixes https://github.com/astral-sh/ruff/issues/17728

## Test Plan

Update snapshots. I verified that said `anyio` methods exists


---

_Label `bug` added by @MichaReiser on 2025-05-01 17:47_

---

_Comment by @github-actions[bot] on 2025-05-01 17:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-05-01 17:55_

---

_@dhruvmanila approved on 2025-05-01 21:22_

---

_Merged by @MichaReiser on 2025-05-01 21:37_

---

_Closed by @MichaReiser on 2025-05-01 21:37_

---

_Branch deleted on 2025-05-01 21:37_

---
