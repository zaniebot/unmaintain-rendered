```yaml
number: 10513
title: "'Revert \"F821: Fix false negatives in .py files when `from __future__ import annotations` is active (#10362)\"'"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: pydantic-f821-revert
created_at: 2024-03-21T16:32:53Z
updated_at: 2024-03-21T16:46:20Z
url: https://github.com/astral-sh/ruff/pull/10513
synced_at: 2026-01-10T22:47:02Z
```

# 'Revert "F821: Fix false negatives in .py files when `from __future__ import annotations` is active (#10362)"'

---

_Pull request opened by @AlexWaygood on 2024-03-21 16:32_

Fixes https://github.com/astral-sh/ruff/issues/10451.

The first commit adds a regression test for #10451. The second commit is a clean revert of https://github.com/astral-sh/ruff/commit/704fefc7ab9c84c7c10b9dfaaf68a4d071ae62d3, the commit that caused the regression.

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-03-21 16:32_

---

_@charliermarsh approved on 2024-03-21 16:36_

---

_Merged by @AlexWaygood on 2024-03-21 16:41_

---

_Closed by @AlexWaygood on 2024-03-21 16:41_

---

_Branch deleted on 2024-03-21 16:41_

---

_Comment by @github-actions[bot] on 2024-03-21 16:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
