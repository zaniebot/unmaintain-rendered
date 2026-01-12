```yaml
number: 1177
title: "Changes to an enum's `__ne__` still affects the `__eq__` for type narrowing"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - narrowing
assignees: []
created_at: 2025-09-13T01:59:09Z
updated_at: 2025-09-30T18:43:41Z
url: https://github.com/astral-sh/ty/issues/1177
synced_at: 2026-01-12T15:54:24Z
```

# Changes to an enum's `__ne__` still affects the `__eq__` for type narrowing

---

_@MeGaGiGaGon_

### Summary

When an enum has it's `__ne__` changed, it is still safe to narrow based on the `__eq__`, which is unaffected by the change. This behaves correctly for the direct `==` case, but not for type narrowing:
https://play.ty.dev/b261649f-6f26-468c-84c7-e70633407931
```py
from enum import Enum
from typing import cast

class Foo(Enum):
    A = 1
    B = 2

    def __ne__(self, other): return False

reveal_type(Foo.A == Foo.A)  # Revealed type: `Literal[True]` (correct)

foo = cast(Foo, Foo.A)

if foo == Foo.A:
    reveal_type(foo)  # Revealed type: `Foo`, could be narrowed to Literal[Foo.A]
```
Showing the unchanged `__eq__`:
```pycon
>>> from enum import Enum
...
... class Foo(Enum):
...     A = 1
...     B = 2
...
...     def __ne__(self, other): return False
...
>>> Foo.A == Foo.A
True
>>> Foo.A == Foo.B
False
```

### Version

playground (174555480)

---

_Label `narrowing` added by @sharkdp on 2025-09-15 07:02_

---

_Comment by @sharkdp on 2025-09-15 07:05_

Thank you for reporting this.

Looks like this could be fixed, even though it seems like a rare case that you would implement `__ne__` without also implementing `__eq__` (in a consistent way).

Note that the default `__ne__` may call a custom `__eq__`, so the converse of this is not true, i.e. when we see a custom `__eq__`, then we must assume custom behavior for `__ne__` as well, even if it's not explicitly implemented:

> For [`__ne__()`](https://docs.python.org/3/reference/datamodel.html#object.__ne__), by default it delegates to [`__eq__()`](https://docs.python.org/3/reference/datamodel.html#object.__eq__) and inverts the result unless it is `NotImplemented`.

<https://docs.python.org/3/reference/datamodel.html#object.__ne__>

---

_Label `good first issue` added by @sharkdp on 2025-09-15 07:05_

---

_Label `good first issue` removed by @sharkdp on 2025-09-30 17:33_

---

_Comment by @sharkdp on 2025-09-30 18:43_

Due to the reasons given in https://github.com/astral-sh/ruff/pull/20516#pullrequestreview-3266474280, and after speaking to @MeGaGiGaGon, we decided to close this issue for now. If someone observes this in a real program, please comment here and we can reopen it.

---

_Closed by @sharkdp on 2025-09-30 18:43_

---
