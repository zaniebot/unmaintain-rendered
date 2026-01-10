```yaml
number: 13867
title: "Fix `D204`'s documentation to correctly mention the conventions when it is enabled"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/blank-after-class-doc
created_at: 2024-10-21T19:51:08Z
updated_at: 2024-10-22T14:52:00Z
url: https://github.com/astral-sh/ruff/pull/13867
synced_at: 2026-01-10T20:59:37Z
```

# Fix `D204`'s documentation to correctly mention the conventions when it is enabled

---

_Pull request opened by @MichaReiser on 2024-10-21 19:51_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13864

## Test Plan

I verified in the playground that `D204` reports violations when using the `numpy` and `pep257` convention but doesn't when using the google convention. 


---

_Label `documentation` added by @MichaReiser on 2024-10-21 19:51_

---

_Comment by @github-actions[bot] on 2024-10-21 20:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-10-22 14:51_

---

_Closed by @MichaReiser on 2024-10-22 14:51_

---

_Branch deleted on 2024-10-22 14:52_

---
