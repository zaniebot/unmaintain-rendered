```yaml
number: 14030
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2024-11-01T00:29:38Z
updated_at: 2024-11-01T21:39:04Z
url: https://github.com/astral-sh/ruff/pull/14030
synced_at: 2026-01-12T15:55:46Z
```

# Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2024-11-01 00:29_

---

_Label `internal` added by @github-actions[bot] on 2024-11-01 00:29_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2024-11-01 00:29_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2024-11-01 00:29_

---

_Review requested from @sharkdp by @github-actions[bot] on 2024-11-01 00:29_

---

_Closed by @AlexWaygood on 2024-11-01 00:48_

---

_Reopened by @AlexWaygood on 2024-11-01 00:48_

---

_Comment by @AlexWaygood on 2024-11-01 10:50_

Two changes to our tests were required:
- https://github.com/python/typeshed/pull/12918 means that we now understand that `__doc__` is present in the global namespace of all modules (good!)
- https://github.com/python/typeshed/pull/12775 means that we no longer understand what the type of `types.ModuleType.__spec__` is (bad!).

  This is because typeshed annotates `types.ModuleType.__spec__` as `ModuleSpec | None` in `types.pyi`, and `ModuleSpec` is imported from `importlib.machinery` in `types.pyi`, and `importlib/machinery.pyi` now re-exports `ModuleSpec` from `importlib._bootstrap`, and `importlib/_bootstrap.pyi` re-exports `ModuleSpec` from `_frozen_importlib` via a `*` import, and we don't understand `*` imports yet.
  
  I'm not sure it's _absolutely_ necessary for the stubs to have _quite_ so much indirection here... but that's a conversation we'd have to have over at typeshed. Whatever the case, even if there are no changes at typeshed, we should fix this regression in type inference naturally when we add support for `*` imports.

---

_Merged by @AlexWaygood on 2024-11-01 10:51_

---

_Closed by @AlexWaygood on 2024-11-01 10:51_

---

_Branch deleted on 2024-11-01 10:51_

---

_Comment by @github-actions[bot] on 2024-11-01 10:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
