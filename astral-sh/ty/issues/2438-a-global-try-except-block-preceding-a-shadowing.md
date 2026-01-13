```yaml
number: 2438
title: "A global try-except block preceding a shadowing import causes incorrect type inference (Module | Object) in local scope"
type: issue
state: open
author: nemowang2003
labels: []
assignees: []
created_at: 2026-01-11T02:15:03Z
updated_at: 2026-01-13T04:11:23Z
url: https://github.com/astral-sh/ty/issues/2438
synced_at: 2026-01-13T04:30:28Z
```

# A global try-except block preceding a shadowing import causes incorrect type inference (Module | Object) in local scope

---

_@nemowang2003_

### Summary

# Description

I have found a minimal reproduction where a `try-except` block placed **before** an import causes type inference ambiguity inside a function scope.

The issue specifically occurs when the imported object shares the same name as the module it is imported from (shadowing).

# Minimal Reproducible Example

### File Structure:
```text
repro/src/
├── __init__.py
└── demo.py
```

```python
# __init__.py

try:
    pass
except Exception:
    pass

import typing

from .demo import demo

typing.reveal_type(demo)  # Literal[42]


def main() -> None:
    typing.reveal_type(demo)  # <module 'repro.demo'> | Literal[42]

# demo.py

demo = 42
```

# Investigation Notes

Order is critial. If the `try-except` block is moved **after** the import, the issue disappears.



### Version

ty 0.0.11 (Homebrew 2026-01-09)

---

_Comment by @nemowang2003 on 2026-01-11 02:30_

Using `from .demo import demo as var` and `typing.reveal_type(var)` in `main` would also fix this. So I think it's somehow related to shadowing.

---

_Comment by @carljm on 2026-01-13 03:36_

I'm pretty sure it's also related to the fact that this code is in an `__init__.py`. Having a variable named `demo` and a submodule named `demo` in `__init__.py` is pretty error-prone.

The relevance of the empty try/except is mysterious to me, though.

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-13 03:36_

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-13 03:36_

---

_Added to milestone `Stable` by @carljm on 2026-01-13 03:36_

---

_Comment by @nemowang2003 on 2026-01-13 04:11_

> The relevance of the empty try/except is mysterious to me, though.

To clarify regarding the empty try-except: the fact that it is empty is incidental. The issue persists even with a populated/functional try-except block.

For example, this standard import check also triggers the error:

```python
try:
    import click
except ImportError:
    import sys
    print('click is not installed')
    sys.exit(1)

from .demo import demo

# the reset remains same ...
```

---
