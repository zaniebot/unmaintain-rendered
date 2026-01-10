```yaml
number: 2357
title: "Using the type alias created by `TypeAliasType` which is the base of a type statement, ty doesn't work properly"
type: issue
state: closed
author: hyperkai
labels: []
assignees: []
created_at: 2026-01-06T09:10:06Z
updated_at: 2026-01-06T09:33:49Z
url: https://github.com/astral-sh/ty/issues/2357
synced_at: 2026-01-10T01:56:41Z
```

# Using the type alias created by `TypeAliasType` which is the base of a type statement, ty doesn't work properly

---

_Issue opened by @hyperkai on 2026-01-06 09:10_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

Using the type alias created by a [type statement](https://docs.python.org/3/reference/simple_stmts.html#the-type-statement), ty works properly, giving no error only for `v1` as shown below:

```python
type TA[T] = tuple[T, ...]

v1: TA[int] = (100, 200)         # No error
v2: TA[int] = ('Hello', 'World') # Error
```

But using the type alias created by [TypeAliasType](https://docs.python.org/3/library/typing.html#typing.TypeAliasType) which is the base of a type statement, ty doesn't work properly, giving no error both for `v1` and `v2` as shown below:

```python
from typing import TypeVar, TypeAliasType

T = TypeVar('T')

TA = TypeAliasType('TA', value=tuple[T, ...], type_params=(T,))

v1: TA[int] = (100, 200)         # No error
v2: TA[int] = ('Hello', 'World') # No error
```

### Version

_No response_

---

_Closed by @AlexWaygood on 2026-01-06 09:33_

---
