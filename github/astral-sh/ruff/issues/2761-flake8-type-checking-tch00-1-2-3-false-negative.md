---
number: 2761
title: " `flake8-type-checking` TCH00{1,2,3}: False negative "
type: issue
state: closed
author: MaksimZayats
labels:
  - bug
assignees: []
created_at: 2023-02-11T10:57:53Z
updated_at: 2023-02-11T19:53:01Z
url: https://github.com/astral-sh/ruff/issues/2761
synced_at: 2026-01-07T13:12:14-06:00
---

#  `flake8-type-checking` TCH00{1,2,3}: False negative 

---

_Issue opened by @MaksimZayats on 2023-02-11 10:57_

Hi! First of all, thank you so much for the amazing project!

I think I found a bug with `flake8-type-checking (TCH)`:
If you try to create any variables with uninitialized objects at right-hand-side, TCH will raise an error.
But if you move these imports into `if TYPE_CHECKING` block, you will get a `NameErorr`: `NameError: name 'MyClass' is not defined`, because `from __future__ import annotations` doesn't cover this use case.


```
├── main
│   ├── __init__.py
│   ├── aliases.py
│   └── somefile.py
└── pyproject.toml
```

```python
# aliases.py
from __future__ import annotations

from collections.abc import Callable
from typing import Any, TypeAlias

from main.somefile import MyClass

AnyCallable: TypeAlias = Callable[..., Any]
MyClassAlias: TypeAlias = MyClass
```

```python
# somefile.py
from __future__ import annotations


class MyClass:
    pass
```

```toml
[tool.ruff]
select = ["ALL"]
ignore = ["D10"]

[tool.ruff.isort]
required-imports = ["from __future__ import annotations"]

```


Ruff output:
```bash
main\aliases.py:3:29: TCH003 Move standard library import `collections.abc.Callable` into a type-checking block
main\aliases.py:6:27: TCH001 Move application import `main.somefile.MyClass` into a type-checking block
```


Version: `ruff 0.0.245`

UPD: one more example:
```python
from __future__ import annotations

from typing import TYPE_CHECKING, Any, TypeVar

if TYPE_CHECKING:
    from collections.abc import Callable

F = TypeVar("F", bound=Callable[..., Any])

"""
    F = TypeVar("F", bound=Callable[..., Any])
                           ^^^^^^^^
NameError: name 'Callable' is not defined. Did you mean: 'callable'?  
"""

```


---

_Label `bug` added by @charliermarsh on 2023-02-11 17:02_

---

_Comment by @charliermarsh on 2023-02-11 17:02_

Definitely a bug, thank you for the clear issue!

---

_Referenced in [astral-sh/ruff#2774](../../astral-sh/ruff/pulls/2774.md) on 2023-02-11 17:43_

---

_Comment by @charliermarsh on 2023-02-11 17:43_

Fixed in the next release (probably tomorrow?).

---

_Closed by @charliermarsh on 2023-02-11 17:44_

---

_Comment by @MaksimZayats on 2023-02-11 19:25_

@charliermarsh, thanks for the quick fix!

But there is one more specific use case:
```python
if TYPE_CHECKING:
    from blahblah import MyClass1, MyClass2

MyDefaultDict: TypeAlias = DefaultDict["MyClass1", "list[MyClass2]"]
```

Ruff output:
```
Move import `...` out of type-checking block. Import is used for more than type hinting.Ruff(TCH004)
```

I think in cases where annotations are used in quotes, `TCH004` should not be raised.


---

_Comment by @charliermarsh on 2023-02-11 19:53_

Right right, my bad.

---

_Referenced in [astral-sh/ruff#2779](../../astral-sh/ruff/pulls/2779.md) on 2023-02-11 19:56_

---
