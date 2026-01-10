```yaml
number: 2154
title: "Improve `invalid-method-override` diagnostics for overloaded methods"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-22T12:37:19Z
updated_at: 2025-12-22T17:04:18Z
url: https://github.com/astral-sh/ty/issues/2154
synced_at: 2026-01-10T01:56:41Z
```

# Improve `invalid-method-override` diagnostics for overloaded methods

---

_Issue opened by @AlexWaygood on 2025-12-22 12:37_

Currently for this file:

```py
from typing import overload

class A:
    @overload
    def f(self) -> int: ...
    @overload
    def f(self, x: str) -> str: ...
    @overload
    def f(self, x: str, y: str) -> bytes: ...
    def f(self, x="foo", y="bar"):
        raise NotImplementedError

class B(A):
    @overload
    def f(self) -> int: ...
    @overload
    def f(self, y: str) -> str: ...
    @overload
    def f(self, x: str, y: str) -> bytes: ...
    def f(self, x="foo", y="bar"):
        raise NotImplementedError
```

we emit this diagnostic:

<img width="1410" height="746" alt="Image" src="https://github.com/user-attachments/assets/6cc95379-7305-4abd-a2ec-b221184b59aa" />

which is extremely confusing, because from the code frame we show you'd think that the subclass method has exactly the same signature as the superclass method.

The issue here is that the second overload of the subclass method has a different parameter name to the second overload of the superclass method. Ideally we'd say exactly that, but that's very hard without https://github.com/astral-sh/ty/issues/163. In the meantime we should just print the first `N` overloads as part of the diagnostic, the same as mypy does, and the same as we do for other diagnostics such as `no-matching-overload`.

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-22 12:37_

---

_Added to milestone `Stable` by @carljm on 2025-12-22 17:04_

---
