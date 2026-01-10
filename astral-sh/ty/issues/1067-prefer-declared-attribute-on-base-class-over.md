```yaml
number: 1067
title: Prefer declared attribute on base class over inferred attribute on subclass
type: issue
state: closed
author: jankatins
labels: []
assignees: []
created_at: 2025-08-20T20:37:24Z
updated_at: 2025-10-13T07:28:59Z
url: https://github.com/astral-sh/ty/issues/1067
synced_at: 2026-01-10T02:06:24Z
```

# Prefer declared attribute on base class over inferred attribute on subclass

---

_Issue opened by @jankatins on 2025-08-20 20:37_

### Summary

We use some descriptor (like) classes and basically have an attribute set by `__set_name__`  (https://docs.python.org/3/reference/datamodel.html#object.__set_name__)

This is a boiled down example:

```py
from typing import Any


class Mixin:
    name: str


class Concrete(Mixin):
    def __init__(self) -> None:
        self.name = None  # type: ignore[assignment]

    def __set_name__(self, owner: Any, name: str) -> None:
        self.name = name

class X:
    c = Concrete()

x = X()
# mypy is ok here, but ty chokes on it
print(x.c.name.upper()) # prints "C"

```

mypy trusts the declaration in the mixin and allows accessing the attribute unconditionally, but ty seems does not see the descriptor protocol (and does not trust the explicit typing info) and assumes the attribute is not set:

```shell
λ  python test.py      
C

λ  mypy test.py        
Success: no issues found in 1 source file

λ  uvx ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                                                                                     warning[possibly-unbound-attribute]: Attribute `upper` on type `Unknown | str | None` is possibly unbound                                                                                                                                                                                                           
  --> test.py:20:7
   |
18 | x = X()
19 | # mypy is ok here, but ty chokes on it
20 | print(x.c.name.upper()) # prints "C"
   |       ^^^^^^^^^^^^^^
   |
info: rule `possibly-unbound-attribute` is enabled by default

Found 1 diagnostic
```

Given that I do quite a lot of `c.name` like usages, I would really avoid having to check every place for None... :-)

### Version

ty 0.0.1-alpha.19

---

_Comment by @carljm on 2025-08-20 21:52_

Thanks for the report! This is definitely a bug. I think the descriptor protocol and `__set_name__` are unrelated, though. We look at all assignments to the name in all methods, we won't handle `__set_name__` any different from any other method in that regard.

Here's an [even more simplified example](https://play.ty.dev/88cd3d79-9c3f-4058-bd6b-f5d797080ea4) (edit by @sharkdp: modified the example to let `B` actually derive from `A`) of the bug. There are really two bugs there: one is that we should error on `self.name = None` (that's already tracked in #166), and the other is that if you ignore that error (as your example does), we should still prefer the declared type from the base class: we should not include the type from the bad assignment, and we should also not union with `Unknown`, because the attribute is declared on the base class.


---

_Added to milestone `Beta` by @carljm on 2025-08-20 21:52_

---

_Renamed from "Attribute set by __set_name__()  is not seen and typing not trusted" to "Prefer declared attribute on base class over inferred attribute on subclass" by @carljm on 2025-08-20 21:52_

---

_Assigned to @sharkdp by @sharkdp on 2025-08-22 13:59_

---

_Closed by @sharkdp on 2025-10-13 07:28_

---
