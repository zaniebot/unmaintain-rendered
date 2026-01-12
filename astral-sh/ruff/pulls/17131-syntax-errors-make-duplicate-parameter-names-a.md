```yaml
number: 17131
title: "[syntax-errors] Make duplicate parameter names a semantic error"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-duplicate-parameter-names
created_at: 2025-04-01T19:09:12Z
updated_at: 2025-04-23T19:45:53Z
url: https://github.com/astral-sh/ruff/pull/17131
synced_at: 2026-01-12T15:56:00Z
```

# [syntax-errors] Make duplicate parameter names a semantic error

---

_@ntBre_

Status
--

This is a pretty minor change, but it was breaking a red-knot mdtest until #17463 landed. Now this should close #11934 as the last syntax error being tracked there!

Summary
--

Moves `Parser::validate_parameters` to `SemanticSyntaxChecker::duplicate_parameter_name`.

Test Plan
--

Existing tests, with `## Errors` replaced with `## Semantic Syntax Errors`.

---

_Label `rule` added by @ntBre on 2025-04-01 19:09_

---

_Label `preview` added by @ntBre on 2025-04-01 19:09_

---

_Comment by @github-actions[bot] on 2025-04-23 14:16_

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

_Marked ready for review by @ntBre on 2025-04-23 14:20_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-23 14:20_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-23 14:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3647 on 2025-04-23 15:21_

Can we remove the `ParseErrorType::DuplicateParameter` variant or is it being used elsewhere as well?

---

_@dhruvmanila approved on 2025-04-23 15:21_

Congrats on finishing all the syntax errors!

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:3647 on 2025-04-23 15:56_

Yep, looks like that's safe to delete!

I also noticed the `DuplicateKeywordArgumentError` variant right next to it. That looks like another good addition to https://github.com/astral-sh/ruff/issues/17412.

---

_@ntBre reviewed on 2025-04-23 15:56_

---

_@MichaReiser approved on 2025-04-23 19:44_

---

_Merged by @ntBre on 2025-04-23 19:45_

---

_Closed by @ntBre on 2025-04-23 19:45_

---

_Branch deleted on 2025-04-23 19:45_

---
