```yaml
number: 22008
title: "[ty] Fixed benchmark markdown auto-linenumbers"
type: pull_request
state: merged
author: sinon
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: fix-markdown-list-numbering
created_at: 2025-12-16T15:05:46Z
updated_at: 2025-12-16T15:38:11Z
url: https://github.com/astral-sh/ruff/pull/22008
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Fixed benchmark markdown auto-linenumbers

---

_@sinon_

## Summary

Small nitpick I noticed when looking to leverage the benchmark script on a codebase we are considering what next-gen typechecker to adopt.

The auto linenumbering of the markdown list is broken due to sublist not being indented enough

<img width="666" height="495" alt="image" src="https://github.com/user-attachments/assets/7f54a9f6-5075-44cc-8f57-21d6bacebad9" />


## Test Plan

Rendered Github preview: https://github.com/sinon/ruff/blob/893556f460cd88623aa19cafb9680f324e598a5e/scripts/ty_benchmark/README.md

<!-- How was it tested? -->


---

_Review requested from @carljm by @sinon on 2025-12-16 15:05_

---

_Review requested from @MichaReiser by @sinon on 2025-12-16 15:05_

---

_Review requested from @AlexWaygood by @sinon on 2025-12-16 15:05_

---

_Review requested from @sharkdp by @sinon on 2025-12-16 15:05_

---

_Review requested from @dcreager by @sinon on 2025-12-16 15:05_

---

_Label `documentation` added by @ntBre on 2025-12-16 15:21_

---

_Label `ty` added by @ntBre on 2025-12-16 15:21_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 15:32_


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

_@MichaReiser approved on 2025-12-16 15:37_

Thank you

---

_Merged by @MichaReiser on 2025-12-16 15:38_

---

_Closed by @MichaReiser on 2025-12-16 15:38_

---
