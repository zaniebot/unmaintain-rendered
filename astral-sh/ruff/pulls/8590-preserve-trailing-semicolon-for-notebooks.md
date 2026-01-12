```yaml
number: 8590
title: Preserve trailing semicolon for Notebooks
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
assignees: []
merged: true
base: main
head: dhruv/notebook-semicolon
created_at: 2023-11-09T21:05:17Z
updated_at: 2023-11-29T17:52:18Z
url: https://github.com/astral-sh/ruff/pull/8590
synced_at: 2026-01-12T15:55:26Z
```

# Preserve trailing semicolon for Notebooks

---

_@dhruvmanila_

## Summary

This PR updates the formatter to preserve trailing semicolon for Jupyter Notebooks.

The motivation behind the change is that semicolons in notebooks are typically used to hide the output, for example when plotting. This is highlighted in the linked issue.

The conditions required as to when the trailing semicolon should be preserved are:
1. It should be a top-level statement which is last in the module.
2. For statement, it can be either assignment, annotated assignment, or augmented assignment. Here, the target should only be a single identifier i.e., multiple assignments or tuple unpacking isn't considered.
3. For expression, it can be any.

## Test Plan

Add a new integration test in `ruff_cli`. The test notebook basically acts as a document as to which trailing semicolons are to be preserved.

fixes: #8254 


---

_Comment by @dhruvmanila on 2023-11-09 21:05_

Current dependencies on/for this PR:
* main
  * **PR #8590** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8590?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8590?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-11-09 21:22_

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

_Label `formatter` added by @dhruvmanila on 2023-11-10 09:23_

---

_Review requested from @konstin by @dhruvmanila on 2023-11-10 09:23_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-11-10 09:23_

---

_Marked ready for review by @dhruvmanila on 2023-11-10 09:23_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/tests/format.rs`:821 on 2023-11-10 09:24_

I'm not sure if this is the correct place for this test case. We would need to refactor the formatter test suite to include notebook files.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:64 on 2023-11-10 09:26_

We could avoid cloning the iterator by making it peekable but that involves updating the function signature wherever this iterator is being passed. This is because it expects `std::slice::Iter`.

---

_@dhruvmanila reviewed on 2023-11-10 09:27_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:64 on 2023-11-10 11:09_

This clone is effectively free (we just clone the slice reference), so this is fine. The peakable things with the function signatures is annoying though, i had also already hit that

---

_@konstin reviewed on 2023-11-10 11:12_

Would it simplify the implementation to insert the semicolon at

https://github.com/astral-sh/ruff/blob/3076d76b0ab7a4b221c3ec1f34a8b68ed07059b1/crates/ruff_python_formatter/src/statement/suite.rs#L399

instead? Then we shouldn't need `TopLevelStatementPosition`

---

_Comment by @dhruvmanila on 2023-11-10 12:12_

> Would it simplify the implementation to insert the semicolon at
> 
> https://github.com/astral-sh/ruff/blob/3076d76b0ab7a4b221c3ec1f34a8b68ed07059b1/crates/ruff_python_formatter/src/statement/suite.rs#L399
> 
> instead? Then we shouldn't need `TopLevelStatementPosition`

I'm assuming what you're recommending is to check at the linked position if this is the last statement and then preserve the semicolon, if any. Is this correct?

I think that'll involve matching against the relevant nodes where the semicolon needs to be preserved as it's only required for a subset of nodes - `StmtAssign`, `StmtAugAssign`, `StmtAnnAssign`, and `StmtExpr`. Additionally, certain conditions needs to be checked like is this a multiple assignment or not? I'd prefer to keep the logic in the respective node.

---

_@konstin approved on 2023-11-10 13:05_

---

_Merged by @dhruvmanila on 2023-11-10 16:23_

---

_Closed by @dhruvmanila on 2023-11-10 16:23_

---

_Branch deleted on 2023-11-10 16:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:64 on 2023-11-29 08:08_

I think this can be simplified to 	

```rust
SuiteKind::TopLevel => NodeLevel::TopLevel(if statements.len() < 2 {
    TopLevelStatementPosition::Last
} else {
    TopLevelStatementPosition::Other
}),
```

---

_@MichaReiser reviewed on 2023-11-29 08:10_

I prefer to avoid using the `context` whenever possible because it is harder to reason about where the context value is set/changed. 

An alternative to the context would have been to parameterized `FormatStmt`, `FormatStmtAssign` etc (by implementing FormatWithOptions) and explicitly pass a value whether it is the last statement. 

---

_@dhruvmanila reviewed on 2023-11-29 17:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:64 on 2023-11-29 17:51_

Yes, that can be done.

---

_Comment by @dhruvmanila on 2023-11-29 17:52_

> An alternative to the context would have been to parameterized `FormatStmt`, `FormatStmtAssign` etc (by implementing FormatWithOptions) and explicitly pass a value whether it is the last statement.

I thought about this but I felt like this kind of information is suited well for the context. And, it already provided the node level information.

---
