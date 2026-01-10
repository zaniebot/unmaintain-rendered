---
number: 15456
title: "[red-knot] Out-of-body (re-)definitions of class variables"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-01-13T13:52:33Z
updated_at: 2025-01-22T09:42:49Z
url: https://github.com/astral-sh/ruff/issues/15456
synced_at: 2026-01-10T01:22:56Z
---

# [red-knot] Out-of-body (re-)definitions of class variables

---

_Issue opened by @sharkdp on 2025-01-13 13:52_

(Re-)definitions of class variables outside the body of the class are currently not checked:

```py
class C:
    c: str = "foo"

C.c = 1  # <-- no error here
```

---

_Label `bug` added by @sharkdp on 2025-01-13 13:52_

---

_Label `red-knot` added by @sharkdp on 2025-01-13 13:52_

---

_Comment by @AlexWaygood on 2025-01-13 15:10_

The bug applies to redefinitions of any attributes from outside their defining scopes. E.g. here we see the same bug manifested for redefinition of a module attribute:

```py
import builtins
from typing_extensions import reveal_type

reveal_type(builtins.copyright)  # revealed: _Printer
builtins.copyright = "foo"       # no diagnostic
reveal_type(builtins.copyright)  # revealed: _Printer
```

---

_Comment by @carljm on 2025-01-13 18:26_

There should be an error if the assigned type is not assignable to the attribute type.

There should not be an updated type if the assignment was an error. We can add narrowing of attribute types in the local scope in case an assignable-but-more-precise type is assigned to an attribute, but this isn't an immediate priority.

I would probably split "checking of attribute assignments" and "narrowing on attribute assignments" as two separate issues. The lack of the former is a bug, the lack of the latter is not really a bug, just a nice-to-have feature we haven't implemented yet. 

---

_Comment by @sharkdp on 2025-01-13 19:20_

Okay, I have updated the description.

---

_Referenced in [astral-sh/ruff#15613](../../astral-sh/ruff/pulls/15613.md) on 2025-01-20 10:46_

---

_Closed by @sharkdp on 2025-01-22 09:42_

---

_Closed by @sharkdp on 2025-01-22 09:42_

---

_Referenced in [astral-sh/ruff#14164](../../astral-sh/ruff/issues/14164.md) on 2025-01-27 14:38_

---
