```yaml
number: 19207
title: "[ty] Upgrade mypy_primer"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/primer
created_at: 2025-07-08T14:31:30Z
updated_at: 2025-07-08T14:56:56Z
url: https://github.com/astral-sh/ruff/pull/19207
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Upgrade mypy_primer

---

_@AlexWaygood_

## Summary

Upgrade to a newer version of mypy_primer, adding one new project to good.txt and one new project to bad.txt (due to https://github.com/astral-sh/ty/issues/764).

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-07-08 14:32_

---

_Label `ty` added by @AlexWaygood on 2025-07-08 14:32_

---

_Comment by @github-actions[bot] on 2025-07-08 14:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-07-08 14:38_

The job was incorrectly marked as a success for some reason, but we can see from [the logs](https://github.com/astral-sh/ruff/actions/runs/16146184615/job/45565526203?pr=19207) that we do panic here when checking `zope.interface` (due to https://github.com/astral-sh/ty/issues/764 )

---

_Comment by @AlexWaygood on 2025-07-08 14:47_

No panics now: https://github.com/astral-sh/ruff/actions/runs/16146387466/job/45566242622?pr=19207

---

_Marked ready for review by @AlexWaygood on 2025-07-08 14:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-08 14:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-08 14:47_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-08 14:47_

---

_Comment by @github-actions[bot] on 2025-07-08 14:51_

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

_Merged by @AlexWaygood on 2025-07-08 14:56_

---

_Closed by @AlexWaygood on 2025-07-08 14:56_

---

_Branch deleted on 2025-07-08 14:56_

---
