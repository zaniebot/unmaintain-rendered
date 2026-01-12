```yaml
number: 16756
title: "[ruff] Fix `--statistics` reporting for unsafe fixes"
type: pull_request
state: merged
author: ZedThree
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: fix-for-statistics-unsafe-fixes
created_at: 2025-03-14T16:53:01Z
updated_at: 2025-03-18T07:03:14Z
url: https://github.com/astral-sh/ruff/pull/16756
synced_at: 2026-01-12T15:55:56Z
```

# [ruff] Fix `--statistics` reporting for unsafe fixes

---

_@ZedThree_

Fixes #16751

## Summary

Previously, unsafe fixes were counted as "fixable" in `Printer::write_statistics`, in contrast to the behaviour in `Printer::write_once`. This changes the behaviour to align with `write_once`, including them only if `--unsafe-fixes` is set.

We now also reuse `Printer::write_summary` to avoid duplicating the logic for whether or not to report if there are hidden fixes.

## Test Plan

Existing tests modified to use an unsafe-fixable rule, and new ones added to cover the case with `--unsafe-fixes`


---

_Comment by @github-actions[bot] on 2025-03-14 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-18 07:02_

That makes sense. Thank you

---

_Label `bug` added by @MichaReiser on 2025-03-18 07:03_

---

_Label `cli` added by @MichaReiser on 2025-03-18 07:03_

---

_Merged by @MichaReiser on 2025-03-18 07:03_

---

_Closed by @MichaReiser on 2025-03-18 07:03_

---
