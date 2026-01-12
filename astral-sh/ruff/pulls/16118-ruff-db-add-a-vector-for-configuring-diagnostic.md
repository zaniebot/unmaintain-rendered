```yaml
number: 16118
title: "ruff_db: add a vector for configuring diagnostic output"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/fix-coloring
created_at: 2025-02-12T14:26:57Z
updated_at: 2025-02-12T14:43:09Z
url: https://github.com/astral-sh/ruff/pull/16118
synced_at: 2026-01-12T15:55:53Z
```

# ruff_db: add a vector for configuring diagnostic output

---

_@BurntSushi_

For now, the only thing one can configure is whether color is enabled or
not. This avoids needing to ask the `colored` crate whether colors have
been globally enabled or disabled. And, more crucially, avoids the need
to _set_ this global flag for testing diagnostic output. Doing so can
have unintended consequences, as outlined in #16115.

Fixes #16115


---

_Review requested from @carljm by @BurntSushi on 2025-02-12 14:26_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-02-12 14:26_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-12 14:26_

---

_Review requested from @sharkdp by @BurntSushi on 2025-02-12 14:26_

---

_@MichaReiser approved on 2025-02-12 14:29_

---

_Label `red-knot` added by @MichaReiser on 2025-02-12 14:29_

---

_@AlexWaygood approved on 2025-02-12 14:32_

Thank you!

---

_Merged by @BurntSushi on 2025-02-12 14:38_

---

_Closed by @BurntSushi on 2025-02-12 14:38_

---

_Branch deleted on 2025-02-12 14:38_

---

_Comment by @github-actions[bot] on 2025-02-12 14:43_

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
