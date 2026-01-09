---
number: 7370
title: "Formatter undocumented deviation: long expression wrapped in extra parens and indented"
type: issue
state: closed
author: andersk
labels:
  - formatter
assignees: []
created_at: 2023-09-13T22:07:09Z
updated_at: 2023-09-15T09:52:09Z
url: https://github.com/astral-sh/ruff/issues/7370
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter undocumented deviation: long expression wrapped in extra parens and indented

---

_Issue opened by @andersk on 2023-09-13 22:07_

Input, unchanged by Black:

```python
result = (
    f(111111111111111111111111111111111111111111111111111111111111111111111111111111111)
    + 1
).bit_length()
```

Ruff:

```python
result = (
    (
        f(
            111111111111111111111111111111111111111111111111111111111111111111111111111111111
        )
        + 1
    ).bit_length()
)
```

---

_Label `formatter` added by @charliermarsh on 2023-09-13 22:31_

---

_Comment by @charliermarsh on 2023-09-13 23:08_

Has to do with this being an assignment -- i.e., we don't have this formatting when it's a standalone expression:

```python
(
    f(111111111111111111111111111111111111111111111111111111111111111111111111111111111)
    + 1
).bit_length()
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-13 23:13_

---

_Comment by @charliermarsh on 2023-09-13 23:56_

The same problem arises with:

```python
result = (
    f(111111111111111111111111111111111111111111111111111111111111111111111111111111111)
    + 1
)()
```

Which gets formatted as:

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

The attribute and call expressions in these two cases (respectively) are returning `IfBreaks`, because the "head" (`value` or `func`) of the expression needs to be parenthesized:

```rust
impl NeedsParentheses for ExprAttribute {
    fn needs_parentheses(
        &self,
        _parent: AnyNodeRef,
        context: &PyFormatContext,
    ) -> OptionalParentheses {
        // Checks if there are any own line comments in an attribute chain (a.b.c).
        if CallChainLayout::from_expression(self.into(), context.source())
            == CallChainLayout::Fluent
        {
            OptionalParentheses::Multiline
        } else if context.comments().has_dangling(self) {
            OptionalParentheses::Always
        } else if self.value.is_name_expr() {
            OptionalParentheses::BestFit
        } else {
            self.value.needs_parentheses(self.into(), context)
        }
    }
}
```

---

_Referenced in [astral-sh/ruff#7373](../../astral-sh/ruff/pulls/7373.md) on 2023-09-14 00:21_

---

_Closed by @charliermarsh on 2023-09-14 09:05_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-15 09:52_

---
