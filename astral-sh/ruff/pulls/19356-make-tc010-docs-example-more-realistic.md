```yaml
number: 19356
title: Make TC010 docs example more realistic
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: alex/tc010-docs
created_at: 2025-07-15T12:28:42Z
updated_at: 2025-07-15T12:58:23Z
url: https://github.com/astral-sh/ruff/pull/19356
synced_at: 2026-01-12T15:56:37Z
```

# Make TC010 docs example more realistic

---

_@AlexWaygood_

## Summary

I noticed this while reviewing https://github.com/astral-sh/ruff/pull/19100. The current example here is pretty confusing IMO: you'd never need to quote `int` in a type annotation, because it's always defined (it's a builtin!). Usually you quote annotations if they represent a forward reference to something defined later on. Also, the first "use instead" suggestion will only be valid if you also add `from __future__ import annotations` to the top of the file; this makes that explicit


---

_Label `documentation` added by @AlexWaygood on 2025-07-15 12:28_

---

_Comment by @github-actions[bot] on 2025-07-15 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-07-15 12:51_

Thank you

---

_Merged by @AlexWaygood on 2025-07-15 12:52_

---

_Closed by @AlexWaygood on 2025-07-15 12:52_

---

_Branch deleted on 2025-07-15 12:52_

---
