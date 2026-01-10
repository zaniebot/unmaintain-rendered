```yaml
number: 18479
title: "[`pycodestyle`] Fix `E731` autofix creating a syntax error for expressions spanned across multiple lines"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-E731
created_at: 2025-06-05T14:33:19Z
updated_at: 2025-06-13T06:44:16Z
url: https://github.com/astral-sh/ruff/pull/18479
synced_at: 2026-01-10T18:45:04Z
```

# [`pycodestyle`] Fix `E731` autofix creating a syntax error for expressions spanned across multiple lines

---

_Pull request opened by @LaBatata101 on 2025-06-05 14:33_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18475

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-05 14:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-06-05 16:47_

---

_Label `fixes` added by @ntBre on 2025-06-05 16:47_

---

_Review requested from @ntBre by @ntBre on 2025-06-05 16:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs`:273 on 2025-06-12 06:51_

I don't think this is specific to if expressions. For example, the input could be


```py
foo_tooltip = (
    lambda x, data: f"\nfoo: {data['foo'][int(x)]}" + 
    more

)
```

I think what we should do instead is to test if the expression is parenthesized (see `is_expression_parenthesized`) and if so, preserve the parentheses around the body.

---

_@MichaReiser reviewed on 2025-06-12 06:51_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs`:273 on 2025-06-12 13:35_

I think `is_expression_parenthesized` could be moved to `ruff_python_trivia`, so that others could make use of it outside the formatter code.

---

_@LaBatata101 reviewed on 2025-06-12 13:35_

---

_Renamed from "[`pycodestyle`] Fix `E731` autofix creating a syntax error for `if` expressions spanned across multiple lines" to "[`pycodestyle`] Fix `E731` autofix creating a syntax error for expressions spanned across multiple lines" by @LaBatata101 on 2025-06-12 13:36_

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-12 13:36_

---

_@MichaReiser reviewed on 2025-06-12 14:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs`:319 on 2025-06-12 14:02_

Oh no, I pointed you to the wrong version it seems. There's one that can be reused. See `parenthesized_range`

---

_@LaBatata101 reviewed on 2025-06-12 14:13_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs`:319 on 2025-06-12 14:13_

fixed

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-12 14:13_

---

_@MichaReiser approved on 2025-06-13 06:43_

thank you

---

_Merged by @MichaReiser on 2025-06-13 06:44_

---

_Closed by @MichaReiser on 2025-06-13 06:44_

---
