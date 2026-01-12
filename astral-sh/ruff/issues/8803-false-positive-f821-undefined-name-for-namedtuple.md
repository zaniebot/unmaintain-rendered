```yaml
number: 8803
title: False positive F821 (Undefined name) for NamedTuple
type: issue
state: closed
author: cbeytas
labels:
  - bug
assignees: []
created_at: 2023-11-21T07:46:29Z
updated_at: 2023-11-21T19:30:49Z
url: https://github.com/astral-sh/ruff/issues/8803
synced_at: 2026-01-12T15:54:48Z
```

# False positive F821 (Undefined name) for NamedTuple

---

_@cbeytas_

ruff 0.1.6, Python 3.11

```python
import typing
User = typing.NamedTuple('User', **{'name': str, 'password': bytes})
User = typing.NamedTuple('User', name=str, password=bytes)
User = typing.NamedTuple('User', [('name', str), ('password', bytes)])
```
Those namedtuples should all be functionally equivalent. Why is F821 flagged on the first one?

```
ruff test.py --isolated
test.py:2:38: F821 Undefined name `name`
test.py:2:51: F821 Undefined name `password`
Found 2 errors.
```


---

_Label `bug` added by @charliermarsh on 2023-11-21 11:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-21 12:00_

---

_Comment by @tmke8 on 2023-11-21 12:55_

Worth noting maybe that the syntax used in the first two lines has been deprecated in Python 3.13:
> *Deprecated since version 3.13, will be removed in version 3.15:* The undocumented keyword argument syntax for creating NamedTuple classes (`NT = NamedTuple("NT", x=int)`) is deprecated, and will be disallowed in 3.15. Use the class-based syntax or the functional syntax instead.

https://docs.python.org/3.13/library/typing.html#typing.NamedTuple

---

_Closed by @charliermarsh on 2023-11-21 19:30_

---
