```yaml
number: 6866
title: "`handle_parenthesized_comment` can't deal with certain kinds of match comments "
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-25T07:52:46Z
updated_at: 2023-08-26T14:45:46Z
url: https://github.com/astral-sh/ruff/issues/6866
synced_at: 2026-01-10T11:09:49Z
```

# `handle_parenthesized_comment` can't deal with certain kinds of match comments 

---

_Issue opened by @konstin on 2023-08-25 07:52_

The following code is valid python, but crashes `handle_parenthesized_comment`:

```python
match a:
    case A(
        # a
        b # b
        = # c
        2 # d
        # e
    ):
        pass
```

```
thread 'main' panicked at 'Unexpected token between nodes: `"\n        b # b\n        = # c\n        "`', crates/ruff_python_formatter/src/comments/placement.rs:147:17
```

The assert in question:

https://github.com/astral-sh/ruff/blob/1044d66c1cdfdfae45af74abeda07ed550131b51/crates/ruff_python_formatter/src/comments/placement.rs#L138-L153

---

_Label `bug` added by @konstin on 2023-08-25 07:52_

---

_Label `formatter` added by @konstin on 2023-08-25 07:52_

---

_Renamed from "`handle_parenthesized_comment` can't deal with certain kinds" to "`handle_parenthesized_comment` can't deal with certain kinds of match comments " by @konstin on 2023-08-25 08:48_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-25 09:25_

---

_Comment by @charliermarsh on 2023-08-25 11:47_

Probably because the b in that example isn’t a node but rather an identifier? I can own.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-25 11:47_

---

_Closed by @charliermarsh on 2023-08-26 14:45_

---
