```yaml
number: 5733
title: Subscript formatting fails when element is parenthesized
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-07-13T10:20:00Z
updated_at: 2023-07-19T15:25:26Z
url: https://github.com/astral-sh/ruff/issues/5733
synced_at: 2026-01-10T11:09:48Z
```

# Subscript formatting fails when element is parenthesized

---

_Issue opened by @konstin on 2023-07-13 10:20_

```python
def f(x):
    x[(1) :: ]
```
fails to format:
```
$ cargo run --bin ruff_python_formatter -- --emit stdout scratch.py
Error: Failed to format node

Caused by:
    syntax error
```

I passes when removing the parentheses around the 1, so this is somehow caused by incorrect parentheses handling.

```python
def f(x):
    x[1 :: ]
```

---

_Label `bug` added by @konstin on 2023-07-13 10:20_

---

_Label `formatter` added by @konstin on 2023-07-13 10:20_

---

_Closed by @konstin on 2023-07-19 15:25_

---
