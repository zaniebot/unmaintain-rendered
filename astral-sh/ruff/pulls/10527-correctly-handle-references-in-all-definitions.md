```yaml
number: 10527
title: "Correctly handle references in `__all__` definitions when  renaming symbols in autofixes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
  - linter
assignees: []
merged: true
base: main
head: pyi025-incorrect-fix
created_at: 2024-03-22T17:54:47Z
updated_at: 2024-03-22T20:11:18Z
url: https://github.com/astral-sh/ruff/pull/10527
synced_at: 2026-01-12T15:55:32Z
```

# Correctly handle references in `__all__` definitions when  renaming symbols in autofixes

---

_@AlexWaygood_

## Summary

This PR is stacked on top of #10525 -- review that one first!

This PR changes the logic in `ruff_linter/src/renamer.rs` to correctly rename references to symbols if those references are string references inside `__all__` definitions. In order for the renamer to be able to distinguish between `__all__` references (which need to have their quotes stripped before they can be renamed) and other references, a new flag is added to the `SemanticModelFlags` bitflag, `DUNDER_ALL_DEFINITION`. This flag is inserted into the semantic model when the model is visiting the r.h.s. of an `__all__` definition.

Fixes #10508

## Test Plan

Several new test cases have been added for PYI025. These include `__all__` definitions with triple-quoted strings, raw strings and implicitly concatenated strings.


---

_Label `bug` added by @AlexWaygood on 2024-03-22 17:54_

---

_Label `fixes` added by @AlexWaygood on 2024-03-22 17:54_

---

_Label `linter` added by @AlexWaygood on 2024-03-22 17:54_

---

_@charliermarsh reviewed on 2024-03-22 18:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/renamer.rs`:216 on 2024-03-22 18:02_

What happens here if you have something like:

```
__all__ = ["Se" 't'"]
```

I.e., mismatching quotes?

---

_@charliermarsh reviewed on 2024-03-22 18:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:2113 on 2024-03-22 18:04_

I think we should set and unset this around the `self.semantic.add_global_reference` calls, just to be safe.

---

_Comment by @github-actions[bot] on 2024-03-22 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-03-22 18:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/renamer.rs`:216 on 2024-03-22 18:15_

Aha, we produce invalid syntax! Good catch...

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-03-22 18:44_

---

_@charliermarsh approved on 2024-03-22 20:03_

Thanks!

---

_Merged by @AlexWaygood on 2024-03-22 20:06_

---

_Closed by @AlexWaygood on 2024-03-22 20:06_

---

_Branch deleted on 2024-03-22 20:06_

---
