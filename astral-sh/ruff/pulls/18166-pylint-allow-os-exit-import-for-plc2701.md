```yaml
number: 18166
title: "[`Pylint`] Allow os._exit import for (`PLC2701`)"
type: pull_request
state: closed
author: danparizher
labels: []
assignees: []
draft: true
base: main
head: fix/PLC2701-os-exit-exception
created_at: 2025-05-18T20:52:43Z
updated_at: 2025-05-25T06:36:10Z
url: https://github.com/astral-sh/ruff/pull/18166
synced_at: 2026-01-10T18:51:01Z
```

# [`Pylint`] Allow os._exit import for (`PLC2701`)

---

_Pull request opened by @danparizher on 2025-05-18 20:52_

This change updates the PLC2701 rule to correctly identify os._exit as a non-private member, aligning its behavior with the SLF001 rule.

Fixes astral-sh/ruff#18143

---

_Comment by @github-actions[bot] on 2025-05-18 20:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Closed by @danparizher on 2025-05-25 06:36_

---

_Branch deleted on 2025-05-25 06:36_

---
