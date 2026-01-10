```yaml
number: 20378
title: bump mypy_primer pin
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: AlexWaygood-patch-1
created_at: 2025-09-14T17:48:45Z
updated_at: 2025-09-15T08:14:56Z
url: https://github.com/astral-sh/ruff/pull/20378
synced_at: 2026-01-10T17:40:28Z
```

# bump mypy_primer pin

---

_Pull request opened by @AlexWaygood on 2025-09-14 17:48_

Pulls in https://github.com/hauntsaninja/mypy_primer/commit/06849fda409cbdc3233c8125a2cf9881bb9447a6

---

_Comment by @github-actions[bot] on 2025-09-14 17:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-14 18:01_

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

_Merged by @AlexWaygood on 2025-09-14 18:16_

---

_Closed by @AlexWaygood on 2025-09-14 18:16_

---

_Branch deleted on 2025-09-14 18:16_

---

_Comment by @sharkdp on 2025-09-15 08:14_

We should also remove `materialize` from `good.txt`. mypy_primer doesn't care if there are projects in the filter that are not available, but ecosystem-analyzer complains. I'll fix that.

---
