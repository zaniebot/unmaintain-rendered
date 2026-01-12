```yaml
number: 11065
title: Make associativity a property of operator precedence
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/precedence-parsing-refactor
created_at: 2024-04-21T05:52:54Z
updated_at: 2024-04-23T04:39:43Z
url: https://github.com/astral-sh/ruff/pull/11065
synced_at: 2026-01-12T15:55:34Z
```

# Make associativity a property of operator precedence

---

_@dhruvmanila_

## Summary

This PR does a few things but the main change is that is makes associativity a property of operator precedence.

1. Rename `Precedence` -> `OperatorPrecedence`
2. Rename `parse_expression_with_precedence` -> `parse_binary_expression_or_higher`
3. Move `current_binding_power` to `OperatorPrecedence::try_from_tokens` [^1]
4. Add a `OperatorPrecedence::is_right_associative` method
5. Move from `increment_precedence` to using `<=` / `<` to check if the parsing loop needs to stop [^2]

[^1]: Another alternative would be to have two separate methods to avoid lookahead as it's required only for once case (`not in`). So, `try_from_current_token(current).or_else(|| try_from_next_token(current, peek))`
[^2]: This will allow us to easily make the refactors mentioned in #10752 

## Test Plan

Make sure the precedence parsing algorithm is still correct by running the test suite, fuzz testing it and running it against a dozen or so open-source repositories.


---

_Label `internal` added by @dhruvmanila on 2024-04-21 05:52_

---

_Label `parser` added by @dhruvmanila on 2024-04-21 05:52_

---

_Comment by @github-actions[bot] on 2024-04-21 06:10_

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

_Marked ready for review by @dhruvmanila on 2024-04-22 06:33_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-22 06:33_

---

_@MichaReiser approved on 2024-04-22 08:05_

---

_Merged by @dhruvmanila on 2024-04-23 04:28_

---

_Closed by @dhruvmanila on 2024-04-23 04:28_

---

_Branch deleted on 2024-04-23 04:28_

---
