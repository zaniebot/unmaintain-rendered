```yaml
number: 20508
title: "[`flake8-pyi`] Fix operator precedence by adding parentheses when needed (`PYI061`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-20265
created_at: 2025-09-22T05:22:12Z
updated_at: 2025-10-15T15:14:21Z
url: https://github.com/astral-sh/ruff/pull/20508
synced_at: 2026-01-10T17:34:34Z
```

# [`flake8-pyi`] Fix operator precedence by adding parentheses when needed (`PYI061`)

---

_Pull request opened by @danparizher on 2025-09-22 05:22_

## Summary

Fixes #20265


---

_Review requested from @AlexWaygood by @danparizher on 2025-09-22 05:22_

---

_Comment by @github-actions[bot] on 2025-09-22 05:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @AlexWaygood on 2025-09-22 07:54_

---

_Label `fixes` added by @AlexWaygood on 2025-09-22 07:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:273 on 2025-10-09 21:22_

If this variable isn't used we should remove it from the signature. Or should it have been used?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:282 on 2025-10-09 21:25_

Do we need to match here? It looks like `OperatorPrecedence::CallAttribute` would work fine if we just did `OperatorPrecedence::from(parent_expr)`, but I could be missing something.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI061.py`:87 on 2025-10-09 21:28_

One additional test case would be one where the `Literal` is already parenthesized, to make sure we don't add a duplicate set.

---

_@ntBre reviewed on 2025-10-09 21:28_

Thanks, this looks good overall. Just a couple of potential simplification suggestions and an idea for another test case.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-12 23:29_

---

_@ntBre reviewed on 2025-10-13 20:18_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI061_PYI061.py.snap`:539 on 2025-10-13 20:18_

This added another layer of parentheses, which I was afraid of. If the expression is already parenthesized we should avoid adding another layer. I think you can use the `parenthesized_range` helper to check.

---

_Review requested from @ntBre by @danparizher on 2025-10-15 00:42_

---

_@ntBre approved on 2025-10-15 14:58_

---

_Merged by @ntBre on 2025-10-15 15:06_

---

_Closed by @ntBre on 2025-10-15 15:06_

---

_Branch deleted on 2025-10-15 15:14_

---
