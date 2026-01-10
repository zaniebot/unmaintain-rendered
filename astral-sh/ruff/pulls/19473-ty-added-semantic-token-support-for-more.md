```yaml
number: 19473
title: "[ty] Added semantic token support for more identifiers"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: semantic_tokens
created_at: 2025-07-21T22:28:47Z
updated_at: 2025-07-22T11:05:40Z
url: https://github.com/astral-sh/ruff/pull/19473
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Added semantic token support for more identifiers

---

_Pull request opened by @UnboundVariable on 2025-07-21 22:28_

I noticed that the semantic token implementation was not handling identifiers in a few cases. This adds support for identifiers that appear in `except`, `case`, `nonlocal`, and `global` statements.


---

_Review requested from @carljm by @UnboundVariable on 2025-07-21 22:28_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-21 22:28_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-21 22:28_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-21 22:28_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-21 22:28_

---

_Comment by @github-actions[bot] on 2025-07-21 22:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-07-21 22:34_

---

_Merged by @UnboundVariable on 2025-07-21 22:39_

---

_Closed by @UnboundVariable on 2025-07-21 22:39_

---

_Branch deleted on 2025-07-21 22:39_

---

_Label `server` added by @AlexWaygood on 2025-07-22 11:04_

---

_Label `ty` added by @AlexWaygood on 2025-07-22 11:04_

---

_Comment by @AlexWaygood on 2025-07-22 11:05_

(reminder to add the `ty` label to all ty-related PRs -- otherwise the PR will be included in the generated changelog for a Ruff release, and will be excluded from the generated changelog for a ty release)

---
