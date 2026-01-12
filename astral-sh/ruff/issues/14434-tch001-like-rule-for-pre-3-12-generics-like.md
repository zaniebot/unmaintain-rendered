```yaml
number: 14434
title: TCH001 like rule for pre 3.12 generics like TypVar, ParamSpec and the likes
type: issue
state: closed
author: bvolkmer
labels:
  - question
assignees: []
created_at: 2024-11-18T13:20:28Z
updated_at: 2024-11-28T07:22:53Z
url: https://github.com/astral-sh/ruff/issues/14434
synced_at: 2026-01-12T15:54:53Z
```

# TCH001 like rule for pre 3.12 generics like TypVar, ParamSpec and the likes

---

_@bvolkmer_


It would be nice to have a rule, that checks if a TypeVar, ParamSpec and the likes is defined outside `if TYPE_CHECKING` eventhough it is not needed. Example:
```python
from __future__ import annotations
from typing import TypeVar
_T = TypeVar("_T") # Warning here: Move generics TypeVar into a type-checking block

def foobar(arg: _T):
    return arg
```

could be:

```python
from __future__ import annotations
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from typing import TypeVar

    _T = TypeVar("_T")

def foobar(arg: _T):
    return arg
```

---

_Comment by @Daverball on 2024-11-18 13:51_

There is a `TCH004` equivalent rule in the upstream `flake8-type-checking` plugin (namely `TC009`). I considered adding a `TCH001-003` equivalent as well, but I deemed its potential for false positives too high. The`TCH004` case is a lot more useful, since it will prevent runtime errors and I will definitely port `TCH009` to ruff.

A more narrow equivalent to `TCH001-003` just for type-vars might make some sense (although I would only do it for type vars that are marked as private with a `_` prefix or by virtue of not being included in `__all__` and then it becomes more questionable how useful this rule is, since you still have to go in with the intent of making that type var private to the module).

Here's a more general discussion about the future of `flake8-type-checking` within ruff: #13208

---

_Label `question` added by @MichaReiser on 2024-11-19 07:41_

---

_Closed by @MichaReiser on 2024-11-28 07:22_

---
