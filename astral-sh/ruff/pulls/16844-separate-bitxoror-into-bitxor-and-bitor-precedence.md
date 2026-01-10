```yaml
number: 16844
title: "Separate `BitXorOr` into `BitXor` and `BitOr` precedence"
type: pull_request
state: merged
author: junhsonjb
labels:
  - internal
assignees: []
merged: true
base: main
head: jjb-split-bitxoror-precedence
created_at: 2025-03-19T13:19:18Z
updated_at: 2025-03-20T10:43:47Z
url: https://github.com/astral-sh/ruff/pull/16844
synced_at: 2026-01-10T19:40:36Z
```

# Separate `BitXorOr` into `BitXor` and `BitOr` precedence

---

_Pull request opened by @junhsonjb on 2025-03-19 13:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This change follows up on the bug-fix requested in #16747 -- `ruff_python_ast::OperatorPrecedence` had an enum variant, `BitXorOr`, which which gave the same precedence to the `|` and `^` operators. This goes against [Python's documentation for operator precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence), so this PR changes the code so that it's correct.

This is part of the overall effort to unify redundant definitions of `OperatorPrecedence` throughout the codebase (#16071)

## Test Plan

<!-- How was it tested? -->

Because this is an internal change, I only ran existing tests to ensure nothing was broken.

---

_@junhsonjb reviewed on 2025-03-19 13:24_

---

_Review comment by @junhsonjb on `crates/ruff_python_ast/src/operator_precedence.rs`:28 on 2025-03-19 13:24_

(This comment is really related to the code on lines 11-18, but GitHub won't let me comment on that block of unchanged code)

The `None`, `Yield`, and `Starred` variants don't show up in Python's official list of Operator Precedences. `None` mostly makes sense to me, as a virtual precedence to represent the absolute lowest value. But I'm curious about why `Yield` and `Starred` were placed in their respective spots. Is there any documentation about this? Or context that can be shared?

[This question is more about my understanding than about the logic in this PR, I just thought this was a good place to ask -- please let me know if there's somewhere else I should raise the question!]

---

_Comment by @github-actions[bot] on 2025-03-19 13:28_

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

_Review requested from @dhruvmanila by @MichaReiser on 2025-03-19 17:09_

---

_Label `internal` added by @MichaReiser on 2025-03-19 17:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/operator_precedence.rs`:28 on 2025-03-20 10:41_

I think it's just an easier way to model the grammar of those expressions in terms of binding power (precedence levels). The `Yield` and `Starred` variant don't show up in the precedence table but they're part of the grammar spec (https://docs.python.org/3/reference/grammar.html) because they're implementation details of the parser (refer to `yield_expr` and `star_named_expression` grammar rule) and the rules for them would be dependent on the surrounding context.

---

_@dhruvmanila reviewed on 2025-03-20 10:41_

---

_@dhruvmanila approved on 2025-03-20 10:42_

Thanks!

---

_Renamed from "[internal] separate `OperatorPrecedence::BitXorOr` into `BitXor` (higher) and BitOr (`ruff_python_ast`)" to "Separate `BitXorOr` into `BitXor` and `BitOr` precedence" by @dhruvmanila on 2025-03-20 10:43_

---

_Merged by @dhruvmanila on 2025-03-20 10:43_

---

_Closed by @dhruvmanila on 2025-03-20 10:43_

---
