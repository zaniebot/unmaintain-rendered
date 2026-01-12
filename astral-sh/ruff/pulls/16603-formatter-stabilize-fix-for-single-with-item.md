```yaml
number: 16603
title: "[formatter] Stabilize fix for single-with-item formatting with trailing comment"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - formatter
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/stabilizie-with-items-bug-fix
created_at: 2025-03-10T14:37:45Z
updated_at: 2025-03-10T18:31:37Z
url: https://github.com/astral-sh/ruff/pull/16603
synced_at: 2026-01-12T15:55:55Z
```

# [formatter] Stabilize fix for single-with-item formatting with trailing comment

---

_@MichaReiser_

## Summary

This PR stabilizies the fix for https://github.com/astral-sh/ruff/issues/14001

We try to only make breaking formatting changes once a year. However,
the plan was to release this fix as part of Ruff 0.9 but I somehow missed it when promoting all other formatter changes. 
I think it's worth making an exception here considering that this is a bug fix, it improves readability, and it should be rare
(very few files in a single project). Our version policy explicitly allows breaking formatter changes in any minor release and the idea of only making breaking formatter changes once a year is mainly to avoid multiple releases throughout the year that introduce large formatter changes

Closes https://github.com/astral-sh/ruff/issues/14001

## Test Plan

Updated snapshot


---

_Label `formatter` added by @MichaReiser on 2025-03-10 14:37_

---

_Label `breaking` added by @MichaReiser on 2025-03-10 14:38_

---

_Marked ready for review by @MichaReiser on 2025-03-10 14:38_

---

_Comment by @github-actions[bot] on 2025-03-10 14:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-03-10 14:49_

---

_@ntBre approved on 2025-03-10 14:54_

---

_Merged by @MichaReiser on 2025-03-10 18:31_

---

_Closed by @MichaReiser on 2025-03-10 18:31_

---

_Branch deleted on 2025-03-10 18:31_

---
