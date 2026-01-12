```yaml
number: 17052
title: Run pre-commit via uv in CI
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/faster-precommit
created_at: 2025-03-28T21:06:46Z
updated_at: 2025-03-28T21:21:49Z
url: https://github.com/astral-sh/ruff/pull/17052
synced_at: 2026-01-12T15:56:00Z
```

# Run pre-commit via uv in CI

---

_@AlexWaygood_

Installing pre-commit via pip can take up to 6s sometimes in CI (e.g. https://github.com/astral-sh/ruff/actions/runs/14137246278/job/39611795976). Installing pre-commit via uv should be faster. It also simplifies the workflow and improves the amount we're dogfooding our own tools


---

_Label `ci` added by @AlexWaygood on 2025-03-28 21:06_

---

_Marked ready for review by @AlexWaygood on 2025-03-28 21:08_

---

_@sharkdp approved on 2025-03-28 21:08_

---

_Merged by @AlexWaygood on 2025-03-28 21:16_

---

_Closed by @AlexWaygood on 2025-03-28 21:16_

---

_Branch deleted on 2025-03-28 21:16_

---

_Comment by @github-actions[bot] on 2025-03-28 21:21_

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
