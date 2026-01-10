```yaml
number: 14660
title: "[`flake8-pyi`] Avoid rewriting invalid type expressions in `unnecessary-type-union` (`PYI055`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - bug
assignees: []
merged: true
base: main
head: pyi055-exclude-invalid-type-expression
created_at: 2024-11-28T18:17:43Z
updated_at: 2024-11-29T03:09:31Z
url: https://github.com/astral-sh/ruff/pull/14660
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Avoid rewriting invalid type expressions in `unnecessary-type-union` (`PYI055`)

---

_Pull request opened by @sbrugman on 2024-11-28 18:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

See this discussion for details https://github.com/astral-sh/ruff/pull/14641#discussion_r1861140449
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Updated the test fixtures to have valid `type` annotations, and appended the invalid `type` annotations as check.


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-28 18:17_

---

_@sbrugman reviewed on 2024-11-28 18:19_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:96 on 2024-11-28 18:19_

The user should be notified, but we can leave that for the type checker.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:96 on 2024-11-28 18:20_

The `semantic.match_builtin_expr` call here can be surprisingly expensive. As a micro-optimisation, we can do the `Expr::Tuple` check first here:

```suggestion
                if !matches!(**slice, Expr::Tuple(_)) && semantic.match_builtin_expr(value, "type")
```

---

_@AlexWaygood approved on 2024-11-28 18:20_

Fab, thank you!

---

_Label `bug` added by @AlexWaygood on 2024-11-28 18:23_

---

_Comment by @github-actions[bot] on 2024-11-28 18:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-11-28 18:30_

---

_Closed by @AlexWaygood on 2024-11-28 18:30_

---

_Branch deleted on 2024-11-28 18:44_

---

_Renamed from "[`flake8-pyi`] Avoid rewriting invalid type expressions in `unnecessary-type-union` (PYI055)" to "[`flake8-pyi`] Avoid rewriting invalid type expressions in `unnecessary-type-union` (`PYI055`)" by @dhruvmanila on 2024-11-29 03:09_

---
