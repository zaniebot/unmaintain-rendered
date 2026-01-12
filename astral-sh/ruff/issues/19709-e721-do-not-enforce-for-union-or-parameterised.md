```yaml
number: 19709
title: "E721: do not enforce for union or parameterised generics"
type: issue
state: closed
author: jakewilliami
labels: []
assignees: []
created_at: 2025-08-03T04:37:18Z
updated_at: 2025-08-03T04:49:21Z
url: https://github.com/astral-sh/ruff/issues/19709
synced_at: 2026-01-12T15:54:57Z
```

# E721: do not enforce for union or parameterised generics

---

_@jakewilliami_

### Summary

[E721](https://docs.astral.sh/ruff/rules/type-comparison/) is good for checking equality of simple types:
```python
>>> int is int
True
```

But struggles with order of union types (even though these are the same):
```python
>>> str | int == int | str
True
>>> str | int is int | str
False
>>> int | float | str == ((int | float) | str)
True
>>> int | float | str is ((int | float) | str)
False
```

And with parameterised generics:
```python
>>> list[int] == list[int]
True
>>> list[int] is list[int]
False
```

I wonder if Ruff should know that it can't enforce E721 for all cases, or whether that is up to the user to determine.

---

_Comment by @jakewilliami on 2025-08-03 04:49_

Closing as this seems to be accounted for and I didn't realise.  Apologies for that!

---

_Closed by @jakewilliami on 2025-08-03 04:49_

---
