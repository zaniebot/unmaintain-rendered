```yaml
number: 11171
title: "[`pyflakes`] Improve `invalid-print-syntax` documentation"
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - documentation
assignees: []
merged: true
base: main
head: invalid-print
created_at: 2024-04-27T04:17:04Z
updated_at: 2024-04-27T14:31:27Z
url: https://github.com/astral-sh/ruff/pull/11171
synced_at: 2026-01-12T15:55:36Z
```

# [`pyflakes`] Improve `invalid-print-syntax` documentation

---

_@JelleZijlstra_

This syntax wasn't "deprecated" in Python 3; it was removed.

I started looking at this rule because I was curious how Ruff could even
detect this without a Python 2 parser. Then I realized that
"print >> f, x" is actually valid Python 3 syntax: it creates a tuple
containing a right-shifted version of the print function.

---

_Comment by @github-actions[bot] on 2024-04-27 04:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-27 11:53_

Thanks!

---

_Renamed from "[pyflakes] Improve invalid-print-syntax documentation" to "[`pyflakes`] Improve `invalid-print-syntax` documentation" by @charliermarsh on 2024-04-27 11:53_

---

_Label `documentation` added by @charliermarsh on 2024-04-27 11:54_

---

_Merged by @charliermarsh on 2024-04-27 11:54_

---

_Closed by @charliermarsh on 2024-04-27 11:54_

---

_Branch deleted on 2024-04-27 14:31_

---
