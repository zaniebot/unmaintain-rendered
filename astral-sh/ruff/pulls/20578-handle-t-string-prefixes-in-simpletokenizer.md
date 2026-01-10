```yaml
number: 20578
title: "Handle t-string prefixes in `SimpleTokenizer`"
type: pull_request
state: merged
author: dylwil3
labels: []
assignees: []
merged: true
base: main
head: t-string-simple-tokenizer
created_at: 2025-09-25T18:21:55Z
updated_at: 2025-09-25T19:33:37Z
url: https://github.com/astral-sh/ruff/pull/20578
synced_at: 2026-01-10T17:40:28Z
```

# Handle t-string prefixes in `SimpleTokenizer`

---

_Pull request opened by @dylwil3 on 2025-09-25 18:21_

The simple tokenizer is meant to skip strings, but it was recording a `Name` token for t-strings (from the `t`). This PR fixes that.

---

_Comment by @github-actions[bot] on 2025-09-25 18:34_

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

_@MichaReiser approved on 2025-09-25 19:32_

---

_Merged by @dylwil3 on 2025-09-25 19:33_

---

_Closed by @dylwil3 on 2025-09-25 19:33_

---
