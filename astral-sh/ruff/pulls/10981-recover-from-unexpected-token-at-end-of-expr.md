```yaml
number: 10981
title: Recover from unexpected token at end of expr
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/expr-end-recovery
created_at: 2024-04-16T18:39:37Z
updated_at: 2024-04-16T19:01:35Z
url: https://github.com/astral-sh/ruff/pull/10981
synced_at: 2026-01-10T22:37:01Z
```

# Recover from unexpected token at end of expr

---

_Pull request opened by @dhruvmanila on 2024-04-16 18:39_

## Summary

This fixes a bug in the parser when `Mode::Expression` is used where the parser didn't recover from unexpected token after the expression.

The current solution isn't ideal as the parser will drop all the remaining tokens after the initial expression has been parsed. This is to avoid panicking especially in the context of forward annotation like:

```python
x: Literal["foo", "bar baz"]
```

Here, the parser is being given the `bar baz` as the source to be parsed using `Mode::Expression`. The parser will parse `bar` as a name expression and then stop because the expression ends there. But, it seems like there's more tokens remaining. This specific case could potentially be recovered by using a `ExprTuple` and raising a "missing comma" error but I've avoided that complexity for now.

## Test Plan

Add new test cases for `Mode::Expression` and verified the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-04-16 18:39_

---

_Label `parser` added by @dhruvmanila on 2024-04-16 18:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-16 18:39_

---

_@MichaReiser approved on 2024-04-16 18:57_

The way I solved this in RomeJS is by what you describe. It parses an array expression. You could also look at how TypeScript's parseExpression recovers. But I'm okay doing this in a follow up

---

_Comment by @github-actions[bot] on 2024-04-16 18:57_

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

_Merged by @dhruvmanila on 2024-04-16 19:01_

---

_Closed by @dhruvmanila on 2024-04-16 19:01_

---

_Branch deleted on 2024-04-16 19:01_

---
