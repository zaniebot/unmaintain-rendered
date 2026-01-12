```yaml
number: 10269
title: Assert the parser is at augmented assign token
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/parser-augmented-assign
created_at: 2024-03-07T08:50:51Z
updated_at: 2024-03-07T13:07:10Z
url: https://github.com/astral-sh/ruff/pull/10269
synced_at: 2026-01-12T15:55:31Z
```

# Assert the parser is at augmented assign token

---

_@dhruvmanila_

## Summary

This PR updates fixes one of the `FIXME` comment to assert that the parser is at one of the possible augmented assignment token when parsing an augmented assignment statement.

## Test Plan

1. Add valid test cases for all the possible augmented assignment tokens
2. Add invalid test cases similar to assignment statement


---

_Label `parser` added by @dhruvmanila on 2024-03-07 08:50_

---

_@dhruvmanila reviewed on 2024-03-07 10:48_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/statements/invalid_assignment_targets.py`:30 on 2024-03-07 10:48_

I talked with @BurntSushi about this comment and it seems to be outdated. Our parser allows a standalone f-string to be present at the top-level.

---

_Marked ready for review by @dhruvmanila on 2024-03-07 10:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-07 10:56_

---

_@MichaReiser reviewed on 2024-03-07 11:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/invalid/statements/invalid_assignment_targets.py`:30 on 2024-03-07 11:58_

I see the definition in the grammar but I don't know how you would make the parser parse a standalone formatted value

```
    expr = BoolOp(boolop op, expr* values)
         | NamedExpr(expr target, expr value)
         | BinOp(expr left, operator op, expr right)
         | UnaryOp(unaryop op, expr operand)
         | Lambda(arguments args, expr body)
         | IfExp(expr test, expr body, expr orelse)
         | Dict(expr* keys, expr* values)
         | Set(expr* elts)
         | ListComp(expr elt, comprehension* generators)
         | SetComp(expr elt, comprehension* generators)
         | DictComp(expr key, expr value, comprehension* generators)
         | GeneratorExp(expr elt, comprehension* generators)
         -- the grammar constrains where yield expressions can occur
         | Await(expr value)
         | Yield(expr? value)
         | YieldFrom(expr value)
         -- need sequences for compare to distinguish between
         -- x < 4 < 3 and (x < 4) < 3
         | Compare(expr left, cmpop* ops, expr* comparators)
         | Call(expr func, expr* args, keyword* keywords)
         | FormattedValue(expr value, int conversion, expr? format_spec) <------------here
         | JoinedStr(expr* values)
         | Constant(constant value, string? kind)
```

I think this is just an internal implementation detail of how Python represents their AST. 

---

_@MichaReiser approved on 2024-03-07 12:00_

Nice

---

_@dhruvmanila reviewed on 2024-03-07 13:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/statements/invalid_assignment_targets.py`:30 on 2024-03-07 13:05_

Yeah, I don't see any way that we can get a `FormattedValue` even in expression mode.

---

_Merged by @dhruvmanila on 2024-03-07 13:07_

---

_Closed by @dhruvmanila on 2024-03-07 13:07_

---

_Branch deleted on 2024-03-07 13:07_

---
