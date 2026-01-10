```yaml
number: 12027
title: "[Ruff v0.5] Fix `ZeroDivisionError`s in the ecosystem check"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: ruff-0.5
head: 0.5-fix-ecosystem
created_at: 2024-06-25T12:15:03Z
updated_at: 2024-06-25T12:48:26Z
url: https://github.com/astral-sh/ruff/pull/12027
synced_at: 2026-01-10T21:56:00Z
```

# [Ruff v0.5] Fix `ZeroDivisionError`s in the ecosystem check

---

_Pull request opened by @AlexWaygood on 2024-06-25 12:15_

Seen in CI in https://github.com/astral-sh/ruff/pull/12026

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-25 12:15_

---

_Review requested from @zanieb by @AlexWaygood on 2024-06-25 12:15_

---

_Review comment by @MichaReiser on `python/ruff-ecosystem/ruff_ecosystem/check.py`:156 on 2024-06-25 12:17_

This comment is kind of hard to discover (when reading `max_display_per_rule`). I suggest to move the comment to line 168 and refer to the explanation in this comment. 

---

_@MichaReiser approved on 2024-06-25 12:17_

---

_Label `ci` added by @MichaReiser on 2024-06-25 12:17_

---

_@AlexWaygood reviewed on 2024-06-25 12:28_

---

_Review comment by @AlexWaygood on `python/ruff-ecosystem/ruff_ecosystem/check.py`:156 on 2024-06-25 12:28_

I kept this comment here (so that if you see *this* TODO, you know that you also need to fix the other location), but also added a comment above the other location referring back to this comment

---

_@zanieb approved on 2024-06-25 12:35_

---

_Comment by @github-actions[bot] on 2024-06-25 12:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @AlexWaygood on 2024-06-25 12:48_

---

_Closed by @AlexWaygood on 2024-06-25 12:48_

---

_Branch deleted on 2024-06-25 12:48_

---
