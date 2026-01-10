```yaml
number: 17725
title: Expand Semantic Syntax Coverage
type: pull_request
state: merged
author: maxmynter
labels:
  - testing
assignees: []
merged: true
base: main
head: expand-semantic-syntax-cov
created_at: 2025-04-29T22:50:09Z
updated_at: 2025-04-30T14:14:09Z
url: https://github.com/astral-sh/ruff/pull/17725
synced_at: 2026-01-10T19:03:00Z
```

# Expand Semantic Syntax Coverage

---

_Pull request opened by @maxmynter on 2025-04-29 22:50_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Re: #17526 

## Summary
Adds tests to red knot and `linter.rs` for the semantic syntax. 

Specifically add tests for `ReboundComprehensionVariable`, `DuplicateTypeParameter`, and `MultipleCaseAssignment`.

Refactor the `test_async_comprehension_in_sync_comprehension` → `test_semantic_error` to be more general for all semantic syntax test cases. 

## Test Plan
This is a test.

## Question
I'm happy to contribute more tests the coming days. 

Should that happen here or should we merge this PR such that the refactor `test_async_comprehension_in_sync_comprehension` → `test_semantic_error` is available on main and others can chime in, too? 


---

_Review requested from @carljm by @maxmynter on 2025-04-29 22:50_

---

_Review requested from @AlexWaygood by @maxmynter on 2025-04-29 22:50_

---

_Review requested from @sharkdp by @maxmynter on 2025-04-29 22:50_

---

_Review requested from @dcreager by @maxmynter on 2025-04-29 22:50_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-29 22:52_

---

_Review requested from @ntBre by @AlexWaygood on 2025-04-29 22:52_

---

_Label `testing` added by @AlexWaygood on 2025-04-29 22:52_

---

_Comment by @github-actions[bot] on 2025-04-29 22:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-29 22:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2025-04-29 23:22_

The red-knot tests here look good to me!

---

_@ntBre approved on 2025-04-30 14:12_

This looks great, thank you!

> Should that happen here or should we merge this PR such that the refactor test_async_comprehension_in_sync_comprehension → test_semantic_error is available on main and others can chime in, too?

I'll go ahead and merge this, and we can handle others in additional PRs. I think it makes sense to batch a few of them like you did here, though, instead of necessarily one PR per error, at least if the tests are pretty straightforward.

---

_Merged by @ntBre on 2025-04-30 14:14_

---

_Closed by @ntBre on 2025-04-30 14:14_

---
