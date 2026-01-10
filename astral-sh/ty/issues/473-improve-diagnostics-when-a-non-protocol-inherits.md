```yaml
number: 473
title: Improve diagnostics when a non-Protocol inherits a Protocol and tries to define stub methods
type: issue
state: closed
author: ilius
labels:
  - diagnostics
assignees: []
created_at: 2025-05-21T15:07:45Z
updated_at: 2025-05-21T18:12:52Z
url: https://github.com/astral-sh/ty/issues/473
synced_at: 2026-01-10T02:34:10Z
```

# Improve diagnostics when a non-Protocol inherits a Protocol and tries to define stub methods

---

_Issue opened by @ilius on 2025-05-21 15:07_

### Summary

When you create a type class from `typing.Protocol`, you don't generally fill in the body of methods, just the signature.

Here is an example:
```py
from typing import Protocol

class A(Protocol):
    def astr(self) -> str: ...

class B(A):
    def bstr(self) -> str: ...

```

Error:
```
error[invalid-return-type]: Function can implicitly return `None`, which is not assignable to return type `str`
 --> ty-protocol.py:7:23
  |
6 | class B(A):
7 |     def bstr(self) -> str: ...
  |                       ^^^
  |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic

```

### Version

`ty 0.0.1-alpha.6`


---

_Comment by @AlexWaygood on 2025-05-21 15:28_

Hi! This looks like a true positive to me. Class `A` is a protocol class, so all methods on the class are implicitly abstract. But class `B` is not a protocol class -- it has `Protocol` in its MRO, but it must directly inherit from `Protocol` in order for it to be considered a `Protocol` class by type checkers or Python at runtime, e.g.

```diff
- class B(A):
+ class B(A, Protocol):
      def bstr(self) -> str: ...
```

 Therefore a method will only be considered abstract on class `B` if it is explicitly decorated with `@abstractmethod`.

I think we could improve our diagnostics here -- we could add a note to `invalid-return-type` diagnostics if we see that `Protocol` is present in the MRO but the class is not actually a `Protocol` class!

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-21 15:28_

---

_Renamed from "Protocol: Function can implicitly return `None`" to "Improve diagnostics when a non-Protocol inherits a Protocol and tries to define stub methods" by @carljm on 2025-05-21 15:34_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-21 15:48_

---

_Closed by @AlexWaygood on 2025-05-21 17:38_

---

_Comment by @ilius on 2025-05-21 17:54_

Thanks.

---

_Comment by @AlexWaygood on 2025-05-21 18:12_

you're welcome!

---
