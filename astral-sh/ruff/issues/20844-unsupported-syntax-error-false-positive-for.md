```yaml
number: 20844
title: Unsupported syntax error false positive for escapes in f-string format specifier
type: issue
state: closed
author: ntBre
labels:
  - bug
  - parser
assignees: []
created_at: 2025-10-13T13:59:11Z
updated_at: 2025-10-15T13:23:18Z
url: https://github.com/astral-sh/ruff/issues/20844
synced_at: 2026-01-10T11:09:59Z
```

# Unsupported syntax error false positive for escapes in f-string format specifier

---

_Issue opened by @ntBre on 2025-10-13 13:59_

This string from our formatter test suite:

https://github.com/astral-sh/ruff/blob/e26dce5543687e84e40f0d57c8ba4949275b18a2/crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py#L709-L711

gets [formatted](https://play.ruff.rs/5520ae38-9395-48ed-87cd-67e7002255d9) to

```py
f"{x:a{z:hy \"user\"}} '''"
```

which is reported as [invalid](https://play.ruff.rs/154cf36b-de49-45a1-9d27-c3bf5032c591) by Ruff. However, this is allowed even before Python 3.12:

```pycon
Python 3.7.9 (default, Aug 23 2020, 00:57:53)
[Clang 10.0.1 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> f"{x:a{z:hy \"user\"}} '''"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
```

even if GitHub's syntax highlighting doesn't like it. The `NameError` means this parsed successfully, unlike escapes in the expression part:

```pycon
>>> f"{'\x123'}"
  File "<stdin>", line 1
SyntaxError: f-string expression part cannot include a backslash
```

It seems that my logic for detecting these was too naive after all:

https://github.com/astral-sh/ruff/blob/e26dce5543687e84e40f0d57c8ba4949275b18a2/crates/ruff_python_parser/src/parser/expression.rs#L1595-L1612

This example from the formatter emits both the backslash and reused quote character diagnostics. We should double-check the comment semantics too, but I think that part may still be correct.

---

_Label `bug` added by @ntBre on 2025-10-13 13:59_

---

_Label `parser` added by @ntBre on 2025-10-13 13:59_

---

_Assigned to @ntBre by @ntBre on 2025-10-13 13:59_

---

_Closed by @ntBre on 2025-10-15 13:23_

---
