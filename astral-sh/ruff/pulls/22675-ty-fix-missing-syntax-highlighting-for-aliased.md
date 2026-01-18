```yaml
number: 22675
title: "[ty] Fix missing syntax highlighting for aliased import names"
type: pull_request
state: open
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
base: main
head: claude/fix-issue-2547-FX1de
created_at: 2026-01-18T11:45:34Z
updated_at: 2026-01-18T12:04:08Z
url: https://github.com/astral-sh/ruff/pull/22675
synced_at: 2026-01-18T12:19:40Z
```

# [ty] Fix missing syntax highlighting for aliased import names

---

_@MichaReiser_

## Summary

When using `from X import Y as Z`, only the alias (Z) was receiving syntax highlighting, while the original import name (Y) was not being highlighted at all. This fix ensures both the imported name and its alias receive consistent syntax highlighting with the same token type.

Fixes: https://github.com/astral-sh/ty/issues/2547


## Test Plan

Added test


---

_Review requested from @carljm by @MichaReiser on 2026-01-18 11:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-18 11:45_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-18 11:45_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-18 11:45_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-18 11:45_

---

_Label `server` added by @MichaReiser on 2026-01-18 11:45_

---

_Label `ty` added by @MichaReiser on 2026-01-18 11:45_

---

_Label `bug` added by @MichaReiser on 2026-01-18 11:45_

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-18 11:48_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-18 11:48_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-18 11:48_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2026-01-18 11:48_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/semantic_tokens.rs`:3299 on 2026-01-18 11:51_

nittiest of nitpicks: I think you meant

```suggestion
from collections.abc import Set as AbstractSet
```

assuming that this is a reference to the convention enforced by https://docs.astral.sh/ruff/rules/unaliased-collections-abc-set-import/ 😄

---

_@AlexWaygood approved on 2026-01-18 11:51_

---
