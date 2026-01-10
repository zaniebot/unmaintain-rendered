```yaml
number: 14977
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
created_at: 2024-12-15T00:31:53Z
updated_at: 2024-12-15T01:04:08Z
url: https://github.com/astral-sh/ruff/pull/14977
synced_at: 2026-01-10T20:42:27Z
```

# Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2024-12-15 00:31_

Close and reopen this PR to trigger CI

---

_Label `internal` added by @github-actions[bot] on 2024-12-15 00:31_

---

_Review requested from @carljm by @github-actions[bot] on 2024-12-15 00:31_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2024-12-15 00:31_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2024-12-15 00:31_

---

_Review requested from @sharkdp by @github-actions[bot] on 2024-12-15 00:31_

---

_Closed by @AlexWaygood on 2024-12-15 00:40_

---

_Reopened by @AlexWaygood on 2024-12-15 00:40_

---

_Comment by @AlexWaygood on 2024-12-15 01:00_

We used `builtins.copyright` for various tests that tested absolute core functionality such as the ability to resolve a symbol from `builtins.pyi` in typeshed. This is because it was, at the time, the simplest function definition we could find in typeshed. But it has recently become more complicated. (I'm partly to blame here... I merged the typeshed PR that made it more complicated 🙈)

I switched our tests to use `builtins.chr` instead of `builtins.copyright`. For these tests that are checking the absolute fundamentals, it feels best to use a function that's as simple as possible, and `builtins.copyright` no longer fits that bill.

---

_Merged by @AlexWaygood on 2024-12-15 01:02_

---

_Closed by @AlexWaygood on 2024-12-15 01:02_

---

_Branch deleted on 2024-12-15 01:02_

---

_Comment by @github-actions[bot] on 2024-12-15 01:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
