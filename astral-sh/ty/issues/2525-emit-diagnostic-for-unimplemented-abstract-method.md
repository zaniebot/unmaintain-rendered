```yaml
number: 2525
title: "Emit diagnostic for unimplemented abstract method on @final class"
type: issue
state: open
author: vepain
labels: []
assignees: []
created_at: 2026-01-15T21:43:37Z
updated_at: 2026-01-16T00:48:08Z
url: https://github.com/astral-sh/ty/issues/2525
synced_at: 2026-01-16T01:03:50Z
```

# Emit diagnostic for unimplemented abstract method on @final class

---

_@vepain_

### Summary

ty does not return any error when a class decorated with `@final` does not implement a inherited abstract method.

The same false negative is observed when class `A` is a `Protocol`.

```py
from abc import ABC, abstractmethod
from typing import final

class A(ABC):
    @abstractmethod
    def foo(self) -> int:
        raise NotImplementedError

@final 
class B(A):  # should error: abstract method foo not implemented
    pass

if __name__ == "__main__":
    b = B()  # should also error 
    b.foo()  # also?
```

### Version

* Python 3.13
* ruff 0.14.13
* ty 0.0.12

---

_Renamed from "False negative for unimplemened abstract method when final" to "False negative for unimplemented abstract method for class decorated with @final" by @vepain on 2026-01-15 21:46_

---

_Renamed from "False negative for unimplemented abstract method for class decorated with @final" to "False negative for unimplemented abstract method for classes decorated with @final" by @vepain on 2026-01-15 21:46_

---

_Comment by @vepain on 2026-01-15 21:55_

This issue is potentially implied by this one: https://github.com/astral-sh/ty/issues/1877

---

_Comment by @carljm on 2026-01-16 00:10_

Thanks! This is a known missing feature, but it doesn't look like we have an issue open for it yet, so we'll use this one.

(#1877 is a related but different feature.)

---

_Added to milestone `Stable` by @carljm on 2026-01-16 00:10_

---

_Renamed from "False negative for unimplemented abstract method for classes decorated with @final" to "Emit diagnostic for unimplemented abstract method on @final class" by @carljm on 2026-01-16 00:10_

---
