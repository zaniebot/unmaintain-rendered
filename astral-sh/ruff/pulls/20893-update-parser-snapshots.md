```yaml
number: 20893
title: Update parser snapshots
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: alex/snapshots-merge-race
created_at: 2025-10-15T14:17:15Z
updated_at: 2025-10-15T14:34:26Z
url: https://github.com/astral-sh/ruff/pull/20893
synced_at: 2026-01-12T15:57:11Z
```

# Update parser snapshots

---

_@AlexWaygood_

## Summary

#20867 had a merge race with #20848, which means that the parser snapshot tests are now failing on `main` and all PRs (see e.g. https://github.com/astral-sh/ruff/actions/runs/18530391729)

## Test Plan

`cargo insta test -p ruff_python_parser --accept`


---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-15 14:17_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-10-15 14:17_

---

_Label `internal` added by @AlexWaygood on 2025-10-15 14:17_

---

_Label `testing` added by @AlexWaygood on 2025-10-15 14:17_

---

_Comment by @ntBre on 2025-10-15 14:18_

Thank you! Just noticed and was starting a branch too!

---

_@ntBre approved on 2025-10-15 14:18_

---

_Merged by @AlexWaygood on 2025-10-15 14:21_

---

_Closed by @AlexWaygood on 2025-10-15 14:21_

---

_Branch deleted on 2025-10-15 14:21_

---

_Comment by @github-actions[bot] on 2025-10-15 14:34_

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
