```yaml
number: 21187
title: "[`flake8-simplify`] Fix SIM222 false positive for `tuple(generator) or None` (`SIM222`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-21136
created_at: 2025-11-01T18:51:27Z
updated_at: 2025-11-10T15:52:14Z
url: https://github.com/astral-sh/ruff/pull/21187
synced_at: 2026-01-10T16:53:55Z
```

# [`flake8-simplify`] Fix SIM222 false positive for `tuple(generator) or None` (`SIM222`)

---

_Pull request opened by @danparizher on 2025-11-01 18:51_

## Summary

Fixes #21136. The rule was incorrectly treating `tuple(generator)` as always truthy, but `tuple(empty_generator)` is falsy.

## Problem Analysis

SIM222 was incorrectly suggesting to remove `or None` from `tuple(item for item in items) or None` because it was treating the tuple call as always truthy. However, when a generator expression is empty, `tuple(empty_generator)` produces an empty tuple `()`, which is falsy. Removing `or None` would change the runtime behavior.

## Approach

Updated `Truthiness::from_expr` to return `Unknown` for `tuple(generator)`, `tuple(list_comp)`, and `tuple(set_comp)` calls, since we cannot statically determine if the resulting tuple will be empty or not. This prevents SIM222 from flagging these cases incorrectly.


---

_Comment by @github-actions[bot] on 2025-11-01 19:08_

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

_Label `bug` added by @ntBre on 2025-11-03 14:25_

---

_Label `rule` added by @ntBre on 2025-11-03 14:25_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1329 on 2025-11-03 14:31_

I think this only needs to include `Generator`, not the comprehension types. Recursing into the `Self::from_expr` arm below should already return `Self::Unknown` for other comprehensions. Generators are the only special case on 1223, if I'm following correctly.

The last two new tests already suppress the rule, for example: https://play.ruff.rs/a211c51a-b6f3-4dae-b259-431a75788464

---

_@ntBre reviewed on 2025-11-03 14:43_

---

_Review requested from @ntBre by @danparizher on 2025-11-07 23:22_

---

_@MichaReiser approved on 2025-11-10 13:03_

---

_Merged by @MichaReiser on 2025-11-10 13:27_

---

_Closed by @MichaReiser on 2025-11-10 13:27_

---

_Branch deleted on 2025-11-10 15:52_

---
