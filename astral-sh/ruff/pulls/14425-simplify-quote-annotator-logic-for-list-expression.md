```yaml
number: 14425
title: Simplify quote annotator logic for list expression
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/simplify-quote-annotation
created_at: 2024-11-18T06:50:12Z
updated_at: 2024-11-18T07:04:17Z
url: https://github.com/astral-sh/ruff/pull/14425
synced_at: 2026-01-12T15:55:47Z
```

# Simplify quote annotator logic for list expression

---

_@dhruvmanila_

## Summary

Follow-up to #14371, this PR simplifies the visitor logic for list expressions to remove the state management. We just need to make sure that we visit the nested expressions using the `QuoteAnnotator` and not the `Generator`. This is similar to what's being done for binary expressions.

As per the [grammar](https://typing.readthedocs.io/en/latest/spec/annotations.html#grammar-token-expression-grammar-annotation_expression), list expressions can be present which can contain other type expressions (`Callable`):
```
       | <Callable> '[' <Concatenate> '[' (type_expression ',')+
                    (name | '...') ']' ',' type_expression ']'
             (where name must be a valid in-scope ParamSpec)
       | <Callable> '[' '[' maybe_unpacked (',' maybe_unpacked)*
                    ']' ',' type_expression ']'
```

## Test Plan

`cargo insta test`

---

_Label `internal` added by @dhruvmanila on 2024-11-18 06:50_

---

_Comment by @dhruvmanila on 2024-11-18 06:59_

I think even [starred expressions](https://typing.readthedocs.io/en/latest/spec/annotations.html#grammar-token-expression-grammar-unpacked) should be considered here:

```
unpacked              ::=  '*' unpackable
                           | <Unpack> '[' unpackable ']'
unpackable            ::=  tuple_type_expression`
                           | name
                                 (where name must refer to an in-scope TypeVarTuple)
tuple_type_expression ::=  <tuple> '[' '(' ')' ']'
                                (representing an empty tuple)
                           | <tuple> '[' type_expression ',' '...' ']'
                                 (representing an arbitrary-length tuple)
                           | <tuple> '[' maybe_unpacked (',' maybe_unpacked)* ']'
```

---

_@MichaReiser approved on 2024-11-18 07:00_

---

_Merged by @dhruvmanila on 2024-11-18 07:03_

---

_Closed by @dhruvmanila on 2024-11-18 07:03_

---

_Branch deleted on 2024-11-18 07:03_

---

_Comment by @github-actions[bot] on 2024-11-18 07:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
