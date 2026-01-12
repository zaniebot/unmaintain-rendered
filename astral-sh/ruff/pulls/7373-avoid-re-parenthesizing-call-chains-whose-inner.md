```yaml
number: 7373
title: Avoid re-parenthesizing call chains whose inner values are parenthesized
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/parens
created_at: 2023-09-14T00:21:06Z
updated_at: 2023-09-14T09:05:39Z
url: https://github.com/astral-sh/ruff/pull/7373
synced_at: 2026-01-12T15:55:23Z
```

# Avoid re-parenthesizing call chains whose inner values are parenthesized

---

_@charliermarsh_

## Summary

Given a statement like:

```python
result = (
    f(111111111111111111111111111111111111111111111111111111111111111111111111111111111)
    + 1
)()
```

When we go to parenthesize the target of the assignment, we use `maybe_parenthesize_expression` with `Parenthesize::IfBreaks`. This then checks if the call on the right-hand side needs to be parenthesized, the implementation of which looks like:

```rust
impl NeedsParentheses for ExprCall {
    fn needs_parentheses(
        &self,
        _parent: AnyNodeRef,
        context: &PyFormatContext,
    ) -> OptionalParentheses {
        if CallChainLayout::from_expression(self.into(), context.source())
            == CallChainLayout::Fluent
        {
            OptionalParentheses::Multiline
        } else if context.comments().has_dangling(self) {
            OptionalParentheses::Always
        } else {
            self.func.needs_parentheses(self.into(), context)
        }
    }
}
```

Checking for `self.func.needs_parentheses(self.into(), context)` is problematic, since, as in the example above, `self.func` may _already_ be parenthesized -- in which case, we _don't_ want to parenthesize the entire expression. If we do, we end up with this non-ideal formatting:

```python
result = (
    (
        f(
            111111111111111111111111111111111111111111111111111111111111111111111111111111111
        )
        + 1
    )()
)
```

This PR modifies the `NeedsParentheses` implementations for call chain expressions to return `Never` if the inner expression has its own parentheses, in which case, the formatting implementations for those expressions will preserve them anyway.

Closes https://github.com/astral-sh/ruff/issues/7370.

## Test Plan

Zulip improves a bit, everything else is unchanged.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99981 |              2760 |                40 |
| transformers |           0.99944 |              2587 |               413 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99834 |               648 |                20 |
| zulip        |           0.99956 |              1437 |                23 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99981 |              2760 |                40 |
| transformers |           0.99944 |              2587 |               413 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99834 |               648 |                20 |
| **zulip**        |           **0.99962** |              **1437** |                **22** |

---

_Label `formatter` added by @charliermarsh on 2023-09-14 00:21_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-14 00:21_

---

_@MichaReiser approved on 2023-09-14 06:33_

---

_@MichaReiser approved on 2023-09-14 06:33_

:guitar: 

---

_@konstin approved on 2023-09-14 07:45_

---

_Merged by @charliermarsh on 2023-09-14 09:05_

---

_Closed by @charliermarsh on 2023-09-14 09:05_

---

_Branch deleted on 2023-09-14 09:05_

---
