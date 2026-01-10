```yaml
number: 20825
title: "[`pyupgrade`] Extend `UP019` to detect `typing_extensions.Text` (`UP019`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20819
created_at: 2025-10-12T23:35:38Z
updated_at: 2025-10-15T14:12:21Z
url: https://github.com/astral-sh/ruff/pull/20825
synced_at: 2026-01-10T17:34:34Z
```

# [`pyupgrade`] Extend `UP019` to detect `typing_extensions.Text` (`UP019`)

---

_Pull request opened by @danparizher on 2025-10-12 23:35_

## Summary

Fixes #20819


---

_Comment by @github-actions[bot] on 2025-10-12 23:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP019.py.snap`:72 on 2025-10-13 19:12_

It might also make sense to record the module we found so that we can report `typing_extensions.Text` here.

---

_@ntBre reviewed on 2025-10-13 19:12_

Thank you! This looks good to me, but we need to make this a preview change since UP019 is a stable rule and this is a significant increase in the rule's scope.

---

_Review requested from @ntBre by @danparizher on 2025-10-15 00:35_

---

_Label `rule` added by @MichaReiser on 2025-10-15 06:48_

---

_Label `preview` added by @MichaReiser on 2025-10-15 06:48_

---

_Merged by @MichaReiser on 2025-10-15 06:52_

---

_Closed by @MichaReiser on 2025-10-15 06:52_

---

_Branch deleted on 2025-10-15 14:12_

---
