```yaml
number: 1658
title: Check if number of variables in unpacking assignment matches slice notation
type: issue
state: open
author: not-my-profile
labels:
  - rule
assignees: []
created_at: 2023-01-05T10:41:14Z
updated_at: 2023-01-05T17:31:48Z
url: https://github.com/astral-sh/ruff/issues/1658
synced_at: 2026-01-12T15:54:41Z
```

# Check if number of variables in unpacking assignment matches slice notation

---

_@not-my-profile_

```python
foo, bar = values[:1] # not enough values to unpack
foo, bar = values[:3] # maybe too many values to unpack
```

I think this check would be a nice addition because static type checkers only perform this check if values has the type `tuple` but they don't perform it when values has e.g. type `list` (since lists of different lengths have the same type).

Sidenote: This check would only work for conventional containers because custom `__getitem__` implementations could return an arbitrary number of values independent of the passed slice object.

---

_Label `rule` added by @charliermarsh on 2023-01-05 14:44_

---
