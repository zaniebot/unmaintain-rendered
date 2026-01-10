```yaml
number: 15492
title: "[red-knot] Typeshed sync and `sys.platform` fixes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/sync-typeshed
created_at: 2025-01-15T10:09:16Z
updated_at: 2025-01-15T10:21:03Z
url: https://github.com/astral-sh/ruff/pull/15492
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Typeshed sync and `sys.platform` fixes

---

_Pull request opened by @sharkdp on 2025-01-15 10:09_

## Summary

The next sync of typeshed would have failed without manual changes anyway, so I'm doing one manual sync + the required changes in our `sys.platform` tests (which are necessary because of my tiny typeshed PR here: https://github.com/python/typeshed/pull/13378).

closes #15485 (the next run of the pipeline in two weeks should be fine as the bug has been fixed upstream)

---

_Label `red-knot` added by @sharkdp on 2025-01-15 10:09_

---

_Review requested from @carljm by @sharkdp on 2025-01-15 10:09_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-15 10:09_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-15 10:09_

---

_@AlexWaygood approved on 2025-01-15 10:12_

---

_Comment by @github-actions[bot] on 2025-01-15 10:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2025-01-15 10:21_

---

_Closed by @sharkdp on 2025-01-15 10:21_

---

_Branch deleted on 2025-01-15 10:21_

---
