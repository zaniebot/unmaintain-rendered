```yaml
number: 11010
title: "Consider `if` expression for parenthesized with items parsing"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/with-items-if-expr
created_at: 2024-04-18T14:22:36Z
updated_at: 2024-04-18T14:40:07Z
url: https://github.com/astral-sh/ruff/pull/11010
synced_at: 2026-01-12T15:55:34Z
```

# Consider `if` expression for parenthesized with items parsing

---

_@dhruvmanila_

## Summary

This PR fixes the bug in parenthesized with items parsing where the `if` expression would result into a syntax error.

The reason being that once we identify that the ambiguous left parenthesis belongs to the context expression, the parser converts the parsed with item into an equivalent expression. Then, the parser continuous to parse any postfix expressions. Now, attribute, subscript, and call are taken into account as they're grouped in `parse_postfix_expression` but `if` expression has it's own parsing function.

Use `parse_if_expression` once all postfix expressions have been parsed. Ideally, I think that `if` could be included in postfix expression parsing as they can be chained as well (`x if True else y if True else z`).

## Test Plan

Add test cases and verified the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-04-18 14:22_

---

_Label `parser` added by @dhruvmanila on 2024-04-18 14:22_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-18 14:22_

---

_@MichaReiser approved on 2024-04-18 14:24_

---

_Merged by @dhruvmanila on 2024-04-18 14:30_

---

_Closed by @dhruvmanila on 2024-04-18 14:30_

---

_Branch deleted on 2024-04-18 14:30_

---

_Comment by @github-actions[bot] on 2024-04-18 14:40_

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
