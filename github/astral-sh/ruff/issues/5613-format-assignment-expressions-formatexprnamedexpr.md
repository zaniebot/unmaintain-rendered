---
number: 5613
title: "Format assignment expressions (`FormatExprNamedExpr`)"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-08T14:41:02Z
updated_at: 2023-07-10T12:32:17Z
url: https://github.com/astral-sh/ruff/issues/5613
synced_at: 2026-01-07T13:12:15-06:00
---

# Format assignment expressions (`FormatExprNamedExpr`)

---

_Issue opened by @konstin on 2023-07-08 14:41_

Assignment expressions are those using the "walrus operator" `:=`:
```python
if x := f():
    g(x)
```
In general, they are easy to format, they are a sequence of target, space, `:=`, space, value with optional parentheses.

The difficulty is that there are a list of [exceptional cases in PEP 572](https://peps.python.org/pep-0572/#exceptional-cases) where the parentheses are mandatory based on the surrounding node, e.g. keyword argument values need parentheses. So for implementing this we either need to use the [tuple way of checking if there are parentheses in range](https://github.com/astral-sh/ruff/blob/40ddc1604cf53d209d55c7fd9ec40a399fc2a53f/crates/ruff_python_formatter/src/expression/expr_tuple.rs#L128) or each node in the list in PEP 572 needs to have a special case where it checks whether the expression it is about to format is a `ExprNamedExpr` and in that cases sets the parentheses to always.

---

_Label `formatter` added by @konstin on 2023-07-08 14:41_

---

_Referenced in [astral-sh/ruff#4798](../../astral-sh/ruff/issues/4798.md) on 2023-07-08 14:41_

---

_Comment by @charliermarsh on 2023-07-08 14:46_

If the named expression needs to be parenthesized, won’t it already have been parenthesized in the parsed source? (Otherwise it would’ve been invalid syntax in the first place.) So would those “exceptions” be solved by preserving original parentheses? May be misunderstanding.

---

_Comment by @konstin on 2023-07-08 14:50_

Good point, i filed this because i [tried the naive solution and failed](https://github.com/astral-sh/ruff/compare/main...named_expr_stash), but you're right, the [tuple way of checking if there are parentheses in range](https://github.com/astral-sh/ruff/blob/40ddc1604cf53d209d55c7fd9ec40a399fc2a53f/crates/ruff_python_formatter/src/expression/expr_tuple.rs#L128) is likely better

---

_Referenced in [astral-sh/ruff#5642](../../astral-sh/ruff/pulls/5642.md) on 2023-07-10 11:52_

---

_Comment by @konstin on 2023-07-10 11:52_

Implemented this differently because unlike tuples even mandatory parentheses are not part of the range

---

_Closed by @konstin on 2023-07-10 12:32_

---
