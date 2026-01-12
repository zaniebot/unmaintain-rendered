```yaml
number: 2334
title: "For extending the type alias created with `TypeAlias` or an assignment statement, ty should also give error for consistency"
type: issue
state: closed
author: hyperkai
labels: []
assignees: []
created_at: 2026-01-05T03:16:58Z
updated_at: 2026-01-05T07:48:42Z
url: https://github.com/astral-sh/ty/issues/2334
synced_at: 2026-01-12T15:54:26Z
```

# For extending the type alias created with `TypeAlias` or an assignment statement, ty should also give error for consistency

---

_@hyperkai_

### Summary

Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

Extending the type alias created with a [type statement](https://docs.python.org/3/reference/simple_stmts.html#type) or [TypeAliasType](https://docs.python.org/3/library/typing.html#typing.TypeAliasType), ty gives the error as shown below:

```python
''' Type statement '''
type TA = int
''' Type statement '''

''' TypeAliasType '''
# from typing import TypeAliasType

# TA = TypeAliasType('TA', value=int)
''' TypeAliasType '''

class Cls(TA): pass # Error
```

> error[invalid-base]: Invalid class base with type `TypeAliasType`

But extending the type alias created with [TypeAlias](https://docs.python.org/3/library/typing.html#typing.TypeAlias) or an [assignment statement](https://docs.python.org/3/reference/simple_stmts.html#assignment-statements), ty doesn't give error as shown below so error should also occur for consistency:

```python
''' TypeAlias '''
from typing import TypeAlias

TA: TypeAlias = int
''' TypeAlias '''

''' Assignment statement '''
# TA = int
''' Assignment statement '''

class Cls(TA): pass # No error
```


### Version

_No response_

---

_Comment by @AlexWaygood on 2026-01-05 07:48_

Thanks for the issue! However, this is deliberate behaviour here. The first of these (the PEP-695 alias) leads to a runtime exception if you try to subclass it. The other two do not.

In an ideal world, maybe we'd say that you should never be allowed to use any kind of type alias in runtime contexts like this. But we've found that real-world actually does do things like this, and it seems needlessly pedantic to disallow it if it won't lead to a runtime error. It's also allowed by other type checkers such as mypy and pyright, and there doesn't seem to be a strong motivation for us to break compatibility with them here

---

_Closed by @AlexWaygood on 2026-01-05 07:48_

---
