```yaml
number: 14364
title: Fix file watcher panic when event has no paths
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-watcher-panic-no-paths
created_at: 2024-11-15T16:51:05Z
updated_at: 2024-11-16T07:36:59Z
url: https://github.com/astral-sh/ruff/pull/14364
synced_at: 2026-01-12T15:55:47Z
```

# Fix file watcher panic when event has no paths

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/14222

I don't know how to test this. There's also no documentation saying whether this is expected for any event other than a rescan, which is explicitly handled above.

Either way, we should not panic, so let's remove the unwrap.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-15 16:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-15 16:51_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-15 16:51_

---

_Label `red-knot` added by @MichaReiser on 2024-11-15 17:01_

---

_Comment by @github-actions[bot] on 2024-11-15 17:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-11-15 23:49_

---

_Merged by @MichaReiser on 2024-11-16 07:36_

---

_Closed by @MichaReiser on 2024-11-16 07:36_

---

_Branch deleted on 2024-11-16 07:36_

---
