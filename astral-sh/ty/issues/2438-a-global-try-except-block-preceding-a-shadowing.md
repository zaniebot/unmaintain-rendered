```yaml
number: 2438
title: "A global try-except block preceding a shadowing import causes incorrect type inference (Module | Object) in local scope"
type: issue
state: open
author: nemowang2003
labels:
  - bug
assignees: []
created_at: 2026-01-11T02:15:03Z
updated_at: 2026-01-15T05:19:52Z
url: https://github.com/astral-sh/ty/issues/2438
synced_at: 2026-01-15T05:50:18Z
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

_Comment by @AlexWaygood on 2026-01-13 08:25_

> I'm pretty sure it's also related to the fact that this code is in an `__init__.py`. Having a variable named `demo` and a submodule named `demo` in `__init__.py` is pretty error-prone.

X-ref #1676

---

_Comment by @carljm on 2026-01-14 01:35_

Wow, this one's a doozy. Nice find and report!

The root cause here involves several factors:
1. The try/except causes us to set AMBIGUOUS reachability for the remainder of the scope. This in itself is a bug: although some code within the try/except may be ambiguously reachable, the code after the try/except is definitely reachable.
2. The line `from .demo import demo` causes us to insert two definitions of `demo` -- first one is a submodule-import (the `demo` submodule has now been implicitly imported and attached as an attribute on this package), and the second one is the actual import of the `demo` attribute from the `demo` submodule. The former definition is a binding only, the latter we currently consider both a binding and a declaration (though there are other bug reports suggesting we should reconsider this anyway, see #1836).
3. When accessing a name from a nested scope, if there is a definitely-bound declaration, we privilege that and ignore bindings. So without the try/except, when we are considering everything definitely-reachable, we just use the "declaration" of the imported `demo` name, and ignore the "binding" of the submodule attribute.
4. Otherwise, we look at all possibly-reachable declarations and bindings and union them together. This is intended to model the fact that we don't know where in the outer scope's control flow our nested scope could be called from. Effectively in this case we are modeling that `main()` might be called in between the attachment of the `demo` submodule attribute and the binding of the imported `demo` name. In this case that's a silly thing to model, since those two definitions are coming from the same statement.

So if we do #1836, then the "bug" would always show up here, instead of being dependent on the `try/except`.
If we fix (1) above (reachability of code after a try/except), then the bug would not be triggered by a `try/except`, but could still show up if we did something else to cause us to consider the import ambiguously reachable.
Either way, I think we should fix (4) by just skipping the submodule-attachment (`ImportFromSubmodule`) definition altogether if it will be immediately shadowed by another definition of the same name, bound by the same import statement.

---

_Label `bug` added by @dhruvmanila on 2026-01-15 05:19_

---
