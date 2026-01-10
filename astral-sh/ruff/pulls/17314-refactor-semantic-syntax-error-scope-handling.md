```yaml
number: 17314
title: Refactor semantic syntax error scope handling
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/syn-scope-context
created_at: 2025-04-09T14:01:49Z
updated_at: 2025-04-09T18:23:31Z
url: https://github.com/astral-sh/ruff/pull/17314
synced_at: 2026-01-10T19:40:37Z
```

# Refactor semantic syntax error scope handling

---

_Pull request opened by @ntBre on 2025-04-09 14:01_

## Summary

Based on the discussion in https://github.com/astral-sh/ruff/pull/17298#discussion_r2033975460, we decided to move the scope handling out of the `SemanticSyntaxChecker` and into the `SemanticSyntaxContext` trait. This PR implements that refactor by:

- Reverting all of the `Checkpoint` and `in_async_context` code in the `SemanticSyntaxChecker`
- Adding four new methods to the `SemanticSyntaxContext` trait
  - `in_async_context`: matches `SemanticModel::in_async_context` and only detects the nearest enclosing function
  - `in_sync_comprehension`: uses the new `is_async` tracking on `Generator` scopes to detect any enclosing sync comprehension
  - `in_module_scope`: reports whether we're at the top-level scope
  - `in_notebook`: reports whether we're in a Jupyter notebook
- In-lining the `TestContext` directly into the `SemanticSyntaxCheckerVisitor`
  - This allows modifying the context as the visitor traverses the AST, which wasn't possible before

One potential question here is "why not add a single method returning a `Scope` or `Scopes` to the context?" The main reason is that the `Scope` type is defined in the `ruff_python_semantic` crate, which is not currently a dependency of the parser. It also doesn't appear to be used in red-knot. So it seemed best to use these more granular methods instead of trying to access `Scope` in `ruff_python_parser` (and red-knot).

## Test Plan

Existing parser and linter tests.


---

_Label `internal` added by @ntBre on 2025-04-09 14:01_

---

_Comment by @github-actions[bot] on 2025-04-09 14:24_

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

_Marked ready for review by @ntBre on 2025-04-09 14:25_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-09 14:25_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-09 14:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1335 on 2025-04-09 16:47_

Should we add a docblock stating that this visitor is only for testing?

Or should we move it to the tests module, considering that it isn't used anywhere else?

---

_@MichaReiser approved on 2025-04-09 16:49_

I only skimmed through the changes. I like it, it simplifies the visitor a good deal and I think we have all the necessary information in red knot too

---

_@ntBre reviewed on 2025-04-09 16:55_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1335 on 2025-04-09 16:55_

Yeah I think moving it to the tests module makes sense. I can't remember why we separated it from the context and kept it here originally, but we haven't used it anywhere else yet.

---

_Merged by @ntBre on 2025-04-09 18:23_

---

_Closed by @ntBre on 2025-04-09 18:23_

---

_Branch deleted on 2025-04-09 18:23_

---
