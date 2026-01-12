```yaml
number: 21181
title: "Delete unused `AsciiCharSet` in `FURB156`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/furb156-set
created_at: 2025-11-01T02:22:16Z
updated_at: 2025-11-03T13:38:36Z
url: https://github.com/astral-sh/ruff/pull/21181
synced_at: 2026-01-12T15:57:18Z
```

# Delete unused `AsciiCharSet` in `FURB156`

---

_@ntBre_

Summary
--

This code has been unused since #14233 but not detected by clippy I guess. This should help to remove the temptation to use the set comparison again like I suggested in #21144. And we shouldn't do the set comparison because of #13802, which #14233 fixed.

Test Plan
--

Existing tests

---

_Label `internal` added by @ntBre on 2025-11-01 02:22_

---

_Comment by @github-actions[bot] on 2025-11-01 02:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-11-01 12:02_

---

_Merged by @ntBre on 2025-11-03 13:38_

---

_Closed by @ntBre on 2025-11-03 13:38_

---

_Branch deleted on 2025-11-03 13:38_

---
