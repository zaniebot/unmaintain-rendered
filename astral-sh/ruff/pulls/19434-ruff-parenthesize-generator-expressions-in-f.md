```yaml
number: 19434
title: "[`ruff`] Parenthesize generator expressions in f-strings (`RUF010`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19433
created_at: 2025-07-20T02:17:50Z
updated_at: 2025-07-30T15:14:20Z
url: https://github.com/astral-sh/ruff/pull/19434
synced_at: 2026-01-12T15:56:39Z
```

# [`ruff`] Parenthesize generator expressions in f-strings (`RUF010`)

---

_@danparizher_

## Summary

Fixes #19433

---

_Comment by @github-actions[bot] on 2025-07-20 02:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-07-20 02:28_

---

_Label `fixes` added by @ntBre on 2025-07-20 02:28_

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:184 on 2025-07-28 19:17_

We should also add a test for an already-parenthesized generator to make sure we don't add a duplicate set of parentheses. We can check the `parenthesized` field on the generator expression in that case:

https://github.com/astral-sh/ruff/blob/e867830848f6cc996186ecaf6570c43a56c0cf2c/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs#L409-L412

That's an example from a recent PR.

---

_@ntBre reviewed on 2025-07-28 19:17_

---

_Review requested from @ntBre by @danparizher on 2025-07-30 00:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:186 on 2025-07-30 14:50_

```suggestion
        return !generator.parenthesized;
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:187 on 2025-07-30 14:57_

This is fine for now, but I wonder if a better fix would be to handle unparenthesized generator expressions differently in `OperatorPrecedence::from_expr`. It doesn't really seem to fit into the rest of the precedence hierarchy, though.

---

_@ntBre approved on 2025-07-30 14:58_

Thanks!

---

_Renamed from "[`ruff`] Fix RUF010 to parenthesize generator expressions in f-strings" to "[`ruff`] Parenthesize generator expressions in f-strings (`RUF010`)" by @ntBre on 2025-07-30 14:59_

---

_Merged by @ntBre on 2025-07-30 15:02_

---

_Closed by @ntBre on 2025-07-30 15:02_

---

_Branch deleted on 2025-07-30 15:12_

---
