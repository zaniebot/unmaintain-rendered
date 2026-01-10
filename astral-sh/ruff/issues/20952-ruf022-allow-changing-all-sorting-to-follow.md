```yaml
number: 20952
title: "RUF022: allow changing `__all__` sorting to follow definition order"
type: issue
state: open
author: Jackenmen
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-17T22:24:22Z
updated_at: 2025-10-20T14:16:01Z
url: https://github.com/astral-sh/ruff/issues/20952
synced_at: 2026-01-10T11:10:00Z
```

# RUF022: allow changing `__all__` sorting to follow definition order

---

_Issue opened by @Jackenmen on 2025-10-17 22:24_

### Summary

I have a suggestion to extend the `__all__` sorting a bit and allow sorting by the *definition* order of names (functions/classes/variables) that are put in `__all__`.

So, for example:
```py
from .submodule import reexported_name
from .submodule_b import different_reexported_name

__all__ = (
    "reexported_name",
    "different_reexported_name",
    "function_b",
    "X",
    "function_a",
    "function_c",
    "Y",
)


def function_b():
    ...


class X:
   ...


def function_a():
    ...


def function_c():
    ...


class Y:
   ...
```

The reason for such a suggestion is that I think it makes it easier to find out what names of public APIs are missing in `__all__` and what names shouldn't be in it (especially the latter one) since it allows one to just go through the file and look through the names in `__all__` in the same order.

---

I've based this enhancement request on my original request for this rule: https://github.com/astral-sh/ruff/issues/1198#issuecomment-1509734136. I was configuring Ruff for a new project today and realised that this part never became a thing.


---

_Label `rule` added by @ntBre on 2025-10-20 14:16_

---

_Label `needs-decision` added by @ntBre on 2025-10-20 14:16_

---
