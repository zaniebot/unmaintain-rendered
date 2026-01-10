```yaml
number: 17201
title: "Don't skip visiting non-tuple slice in `typing.Annotated` subscripts"
type: pull_request
state: merged
author: Daverball
labels:
  - bug
assignees: []
merged: true
base: main
head: bugfix/esoteric-use-of-annotated
created_at: 2025-04-04T12:12:00Z
updated_at: 2025-04-04T14:32:15Z
url: https://github.com/astral-sh/ruff/pull/17201
synced_at: 2026-01-10T19:40:37Z
```

# Don't skip visiting non-tuple slice in `typing.Annotated` subscripts

---

_Pull request opened by @Daverball on 2025-04-04 12:12_

Fixes: #17196

## Summary

Skipping these nodes for malformed type expressions would lead to incorrect semantic state, which can in turn mean we emit false positives for rules like `unused-variable`(`F841`)

## Test Plan

`cargo nextest run`

---

_Comment by @github-actions[bot] on 2025-04-04 12:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-04-04 12:50_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:1680 on 2025-04-04 12:50_

Should we use `visit_type_definition` (when in type definition context) / `visit_non_type_definition` here instead?

---

_Label `bug` added by @dhruvmanila on 2025-04-04 12:55_

---

_@Daverball reviewed on 2025-04-04 13:05_

---

_Review comment by @Daverball on `crates/ruff_linter/src/checkers/ast/mod.rs`:1680 on 2025-04-04 13:05_

`visit_expr` preserves the current flags, so the only reason to call either of those functions would be to either always or never treat that slice as a type definition, neither of which I'm convinced is appropriate here.

It makes most sense to me to treat it as a type definition if the outside is a type definition and as a regular expression otherwise.

---

_@dhruvmanila reviewed on 2025-04-04 13:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:1680 on 2025-04-04 13:17_

That's fair enough.

---

_@dhruvmanila approved on 2025-04-04 13:17_

---

_Merged by @dhruvmanila on 2025-04-04 13:17_

---

_Closed by @dhruvmanila on 2025-04-04 13:17_

---

_Branch deleted on 2025-04-04 14:32_

---
