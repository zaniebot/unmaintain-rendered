```yaml
number: 883
title: "isinstance(arg, A | B) does not narrow"
type: issue
state: closed
author: elbaro
labels: []
assignees: []
created_at: 2025-07-24T11:58:41Z
updated_at: 2025-07-24T12:04:37Z
url: https://github.com/astral-sh/ty/issues/883
synced_at: 2026-01-10T02:06:24Z
```

# isinstance(arg, A | B) does not narrow

---

_Issue opened by @elbaro on 2025-07-24 11:58_

### Summary

https://play.ty.dev/69be450c-2bcc-48b8-b682-bc140b3ba5e3

```py
from typing import Any


class Parent: ...


class A(Parent):
    field: int


class B(Parent):
    field: int


def f(arg: Parent):
    assert isinstance(arg, A | B)
    return arg.field
```

```
    Type `Parent` has no attribute `field` (unresolved-attribute) [Ln 17, Col 12]
```

However this passes:

https://play.ty.dev/de915d9a-c523-460e-9f6d-7c4989517981

```py
from typing import Any


class Parent: ...


class A(Parent):
    field: int


def f(arg: Parent):
    assert isinstance(arg, A)
    return arg.field

```




### Version

ty 0.0.1-alpha.15 (326db79d6 2025-07-21)

---

_Comment by @InSyncWithFoo on 2025-07-24 12:00_

This seems to be a duplicate of #122.

---

_Closed by @AlexWaygood on 2025-07-24 12:04_

---
