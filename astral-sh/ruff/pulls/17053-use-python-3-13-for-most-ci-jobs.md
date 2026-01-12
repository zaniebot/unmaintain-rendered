```yaml
number: 17053
title: Use Python 3.13 for most CI jobs
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/newer-python
created_at: 2025-03-28T21:27:32Z
updated_at: 2025-03-29T00:35:46Z
url: https://github.com/astral-sh/ruff/pull/17053
synced_at: 2026-01-12T15:56:00Z
```

# Use Python 3.13 for most CI jobs

---

_@AlexWaygood_

_No description provided._

---

_Label `ci` added by @AlexWaygood on 2025-03-28 21:27_

---

_Renamed from "Use Python 3.13 for CI jobs" to "Use Python 3.13 for most CI jobs" by @AlexWaygood on 2025-03-28 21:35_

---

_Comment by @github-actions[bot] on 2025-03-28 21:44_

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

_Comment by @AlexWaygood on 2025-03-28 21:54_

It's a tiny bit unfortunate to have to use a different Python version for ruff-lsp tests than anything else, but Python 3.13 has been out for a while now (I'd like to upgrade), and ruff-lsp is deprecated anyway (hopefully we should remove that whole CI job before too long)

---

_Marked ready for review by @AlexWaygood on 2025-03-28 21:54_

---

_@MichaReiser approved on 2025-03-28 23:35_

---

_Merged by @AlexWaygood on 2025-03-29 00:35_

---

_Closed by @AlexWaygood on 2025-03-29 00:35_

---

_Branch deleted on 2025-03-29 00:35_

---
