```yaml
number: 20321
title: Bump LibCST to 1.8.4
type: pull_request
state: merged
author: ntBre
labels:
  - dependencies
assignees: []
merged: true
base: main
head: brent/bump-libcst
created_at: 2025-09-09T21:09:23Z
updated_at: 2025-09-09T21:34:34Z
url: https://github.com/astral-sh/ruff/pull/20321
synced_at: 2026-01-10T17:46:22Z
```

# Bump LibCST to 1.8.4

---

_Pull request opened by @ntBre on 2025-09-09 21:09_

This should fix the fuzz build on `main`. They added support for t-strings, which made one of our matches non-exhaustive.

https://github.com/Instagram/LibCST/releases/tag/v1.8.4


---

_Label `dependencies` added by @ntBre on 2025-09-09 21:09_

---

_Comment by @github-actions[bot] on 2025-09-09 21:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-09 21:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @ntBre on 2025-09-09 21:23_

---

_Comment by @github-actions[bot] on 2025-09-09 21:27_

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

_@zanieb approved on 2025-09-09 21:30_

---

_Merged by @ntBre on 2025-09-09 21:34_

---

_Closed by @ntBre on 2025-09-09 21:34_

---

_Branch deleted on 2025-09-09 21:34_

---
