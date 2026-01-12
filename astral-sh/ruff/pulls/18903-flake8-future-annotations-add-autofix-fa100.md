```yaml
number: 18903
title: "[`flake8-future-annotations`] Add autofix (`FA100`) "
type: pull_request
state: merged
author: ntBre
labels:
  - fixes
assignees: []
merged: true
base: main
head: brent/fa100-autofix
created_at: 2025-06-23T20:11:36Z
updated_at: 2025-06-25T12:37:19Z
url: https://github.com/astral-sh/ruff/pull/18903
synced_at: 2026-01-12T15:56:27Z
```

# [`flake8-future-annotations`] Add autofix (`FA100`) 

---

_@ntBre_

Summary
--

This PR resolves the easiest part of https://github.com/astral-sh/ruff/issues/18502 by adding an autofix that just adds
`from __future__ import annotations` at the top of the file, in the same way
as FA102, which already has an identical unsafe fix.

Test Plan
--

Existing snapshots, updated to add the fixes.

---

_Label `fixes` added by @ntBre on 2025-06-23 20:11_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-23 20:12_

---

_Marked ready for review by @ntBre on 2025-06-23 20:21_

---

_Comment by @github-actions[bot] on 2025-06-23 20:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @ntBre on 2025-06-23 20:34_

---

_Marked ready for review by @ntBre on 2025-06-23 20:40_

---

_@MichaReiser approved on 2025-06-25 08:57_

---

_Merged by @ntBre on 2025-06-25 12:37_

---

_Closed by @ntBre on 2025-06-25 12:37_

---

_Branch deleted on 2025-06-25 12:37_

---
