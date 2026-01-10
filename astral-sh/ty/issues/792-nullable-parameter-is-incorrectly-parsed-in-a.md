```yaml
number: 792
title: Nullable parameter is incorrectly parsed in a lambda
type: issue
state: closed
author: CorentinDeBoisset
labels: []
assignees: []
created_at: 2025-07-09T15:09:39Z
updated_at: 2025-07-09T15:22:31Z
url: https://github.com/astral-sh/ty/issues/792
synced_at: 2026-01-10T02:07:36Z
```

# Nullable parameter is incorrectly parsed in a lambda

---

_Issue opened by @CorentinDeBoisset on 2025-07-09 15:09_

### Summary

```py
from typing import Iterable

def sort(iterable: Iterable, attribute: str | None = None) -> Iterable:
    if not attribute:
        return sorted(iterable)
    else:
        return sorted(iterable, key=lambda item: item.get(attribute) if type(item) is dict else getattr(item, attribute))
```

This error is raised on the final line:

```
Argument to function `getattr` is incorrect: Expected `str`, found `str | None` (invalid-argument-type)
```

Playground: https://play.ty.dev/caa55b6f-6506-405b-acaf-a71fdbe7d3a7

### Version

ty 0.0.1-alpha.13 (6a408d5f9 2025-07-02)

---

_Comment by @AlexWaygood on 2025-07-09 15:17_

A smaller repro is:

```py
def f(x: str): ...

def g(x: str | None):
    if not x:
        return

    def h():
        f(x)

    h()  # Argument to function `f` is incorrect: Expected `str`, found `str | None` (invalid-argument-type)
```

I think we already have an issue open for this somewhere

---

_Comment by @AlexWaygood on 2025-07-09 15:20_

Yeah, I think this is a duplicate of #559. Thanks for the report, though!

---

_Closed by @AlexWaygood on 2025-07-09 15:20_

---

_Comment by @CorentinDeBoisset on 2025-07-09 15:22_

Actually, after tinkering with it a bit, it seems that python lambda are absolutely not proper closures. See [this playground](https://play.ty.dev/53cc3faf-781c-4aeb-b189-7494c1528dd9). No idea what should be the proper course of action.

---
