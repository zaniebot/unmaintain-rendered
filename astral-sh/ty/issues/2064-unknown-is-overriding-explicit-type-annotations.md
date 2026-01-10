```yaml
number: 2064
title: "`Unknown` is overriding explicit type annotations"
type: issue
state: closed
author: MarkusPielmeier
labels: []
assignees: []
created_at: 2025-12-18T12:14:39Z
updated_at: 2025-12-18T12:15:47Z
url: https://github.com/astral-sh/ty/issues/2064
synced_at: 2026-01-10T01:54:00Z
```

# `Unknown` is overriding explicit type annotations

---

_Issue opened by @MarkusPielmeier on 2025-12-18 12:14_

### Summary

A variable with explicit type annotation loses its type information when a value of type `Unknown` is assigned:

```py
from typing import reveal_type


# no type annotation to force ty inferring `Unknown`
def foo(): ...


def int_acceptor(value: int) -> None:
    print(value)


def str_acceptor(value: str) -> None:
    print(value)


def bar() -> None:
    x: int = foo()

    reveal_type(x)

    int_acceptor(x)
    str_acceptor(x)

```

In this example, i would expect the revealed type of `x` to be `int` due to its explicit annotation.
In consequence, i would expect ty to detect the type mismatch of calling `str_acceptor(x)`.

But actually, ty is revealing `x` being of type `Unknown` and no further diagnostics being issued.

``` txt
$ uvx ty check sandbox.py
info[revealed-type]: Revealed type
  --> sandbox.py:18:17
   |
17 |     x: int = foo()
18 |
19 |     reveal_type(x)
   |                 ^ `Unknown`
20 |
21 |     int_acceptor(x)
   |

Found 1 diagnostic
```

### Version

ty 0.0.3 (fadfe0966 2025-12-17)

---

_Comment by @AlexWaygood on 2025-12-18 12:15_

Thanks for the report! Please see #136

---

_Closed by @AlexWaygood on 2025-12-18 12:15_

---
