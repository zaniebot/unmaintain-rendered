```yaml
number: 633
title: "Do not fall back to behaving as if there is no `__get__` if call to `__get__` fails to type check"
type: issue
state: closed
author: n-gao
labels:
  - generics
  - attribute access
assignees: []
created_at: 2025-06-11T10:03:04Z
updated_at: 2025-11-13T12:05:12Z
url: https://github.com/astral-sh/ty/issues/633
synced_at: 2026-01-10T02:06:24Z
```

# Do not fall back to behaving as if there is no `__get__` if call to `__get__` fails to type check

---

_Issue opened by @n-gao on 2025-06-11 10:03_

### Summary

The following code gives false positives in `ty` but works in PyLance:
```py
from typing import Protocol, ClassVar, Self, reveal_type

class Descriptor[S, T](Protocol):
    def __get__(self, instance: S, owner: type[S]) -> T:
        ...

class HasItem(Protocol):
    item: ClassVar[Descriptor[Self, str]]

def f(x: HasItem) -> str:
    result = x.item
    reveal_type(result) # PyLance: str, ty: Descriptor[HasItem, str]
    return result
```

I would assume that `x.item` will be inferred to be `str`.

### Version

0.0.1a8

---

_Label `attribute access` added by @AlexWaygood on 2025-06-11 10:23_

---

_Label `generics` added by @carljm on 2025-06-11 18:20_

---

_Comment by @carljm on 2025-06-11 18:24_

We do support descriptors in general. I think one thing that's potentially confusing is that if a call to `__get__` fails with a type error, we don't surface that error, we just skip the descriptor protocol. That may not be ideal.

The specific problem here appears to be related to the use of `typing.Self`. [This works](https://play.ty.dev/e03af05c-c805-4e9c-a503-59eedfba4905), but [this does not](https://play.ty.dev/0bdee25a-249f-4dcd-9e22-ce3fc84181fc) -- the only difference is the use of `Self` vs `"HasItem"`. (I also removed the use of protocols from the OP, just to eliminate another possible factor.)

---

_Renamed from "Descriptors `__get__` are not resolved" to "Do not fall back to behaving as if there is no `__get__` if call to `__get__` fails to type check" by @carljm on 2025-11-13 07:27_

---

_Comment by @carljm on 2025-11-13 07:29_

This came up again today.

We need #194 to get good diagnostics from these cases, but even in the absence of that, we should still not fall back to not-a-descriptor if the call to `__get__` fails.

(The OP example here also requires #1124 in order for the call to `__get__` to not fail; that's a separate issue.)

---

_Added to milestone `Stable` by @carljm on 2025-11-13 07:29_

---

_Closed by @AlexWaygood on 2025-11-13 12:05_

---
