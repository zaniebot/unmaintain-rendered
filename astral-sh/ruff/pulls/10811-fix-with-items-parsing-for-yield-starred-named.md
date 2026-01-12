```yaml
number: 10811
title: Fix with items parsing for yield, starred, named expr
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/fix-with-item-parsing
created_at: 2024-04-07T10:08:05Z
updated_at: 2024-04-09T09:08:57Z
url: https://github.com/astral-sh/ruff/pull/10811
synced_at: 2026-01-12T15:55:33Z
```

# Fix with items parsing for yield, starred, named expr

---

_@dhruvmanila_

## Summary

This PR fixes with item parsing considering the yield, starred and named expression.

### But, what's the bug?

The yield and starred expressions weren't being considered when determining whether the with items are parenthesized or if it's a parenthesized expression. Taking a simple example:

```py
with (item1, *item2): ...
```

This is parsed as two individual with items currently but this is actually a tuple expression with two elements. This is easier to understand with the grammar:

```ebnf
with_stmt:
    | 'with' '(' ','.with_item+ ','? ')' ':' block 
    | 'with' ','.with_item+ ':' [TYPE_COMMENT] block 

with_item:
    | expression 'as' star_target &(',' | ')' | ':') 
    | expression 
```
(Omitted the `async` variant for brevity.)

Here, the PEG parser will _try_ the first variant in `with_stmt` which is a parenthesized with items and if that fails then it'll try the second variant. The grammar for a with item is `expression` which doesn't allow yield, starred, and named expression. This means that if they're present then the first variant would fail and it'll be the second variant which can possibly parse them.

### Solution

I've updated the logic so that it resembles what the PEG parser does.

Our algorithm starts in a similar manner by assuming that it's a parenthesized with items. This means that the `parse_with_item` should parse the context expression with a higher grammar rule between parenthesized with items and parenthesized expression. Here's an explanation mentioned in the code:

```rs
// If it's in an ambiguous state, the parenthesis (`(`) could be part of any
// of the following expression:
//
// Tuple expression          -  star_named_expression
// Generator expression      -  named_expression
// Parenthesized expression  -  (yield_expr | named_expression)
// Parenthesized with items  -  expression
//
// Here, the right side specifies the grammar for an element corresponding
// to the expression mentioned in the left side.
//
// So, the grammar used should be able to parse an element belonging to any
// of the above expression. At a later point, once the parser understands
// where the parenthesis belongs to, it'll validate and report errors for
// any invalid expression usage.
//
// Thus, we can conclude that the grammar used should be:
//      (yield_expr | star_named_expression)
```

Now, there are two important things to determine the with item kind:
1. If any of the with item has optional variables, it's a parenthesized with items
2. If any of the context expression is yield, starred or named, it's a parenthesized expression

Whatever the with item kind turns out to be, the parser needs to limit the expression because, as mentioned above, it parses using the highest common grammar.

This is basically what the change is.

## Test Plan

Added new valid and invalid test cases and verified the snapshots.


---

_Label `parser` added by @dhruvmanila on 2024-04-07 10:08_

---

_Comment by @github-actions[bot] on 2024-04-07 10:26_

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

_@dhruvmanila reviewed on 2024-04-07 11:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1457 on 2024-04-07 11:58_

One or more with item have an optional variable, so it's a parenthesized with items. Limit the parsed expression by reporting error for invalid expressions in this context.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1479 on 2024-04-07 11:59_

None of the with items have an optional variable, if these expressions are present, it's a parenthesized expression.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1497 on 2024-04-07 11:59_

Now, there's a dedicated branch if `has_optional_vars` is true.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1522 on 2024-04-07 11:59_

Limiting the expression because we know it's a parenthesized expression.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1541 on 2024-04-07 11:59_

Limiting the expression because we know it's a tuple expression.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1598 on 2024-04-07 12:00_

This check is now automatically done when limiting the expression in `parse_parenthesized_with_items`

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/tests/snapshots/ruff_python_parser__parser__tests__parser__tests__parse_with_stmt.snap`:477 on 2024-04-07 12:01_

This range update is similar to the one for named expression:

```py
with (x := 1): ...
#    ^^^^^^^^ with item range
#     ^^^^^^ expression range

with (yield x): ...
#    ^^^^^^^^^ with item range
#     ^^^^^^^ expression range
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/valid_syntax@statement__ambiguous_lpar_with_items.py.snap`:1 on 2024-04-07 12:04_

The new test cases are added in their respective sections in the test file. But, I verified that the range updates are correct by temporarily moving all of the new test cases at the bottom of the file.

---

_@dhruvmanila reviewed on 2024-04-07 12:04_

---

_Marked ready for review by @dhruvmanila on 2024-04-07 12:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 12:05_

---

_Label `bug` added by @dhruvmanila on 2024-04-07 12:53_

---

_@MichaReiser approved on 2024-04-08 12:12_

---

_Merged by @dhruvmanila on 2024-04-09 08:50_

---

_Closed by @dhruvmanila on 2024-04-09 08:50_

---

_Branch deleted on 2024-04-09 08:50_

---
