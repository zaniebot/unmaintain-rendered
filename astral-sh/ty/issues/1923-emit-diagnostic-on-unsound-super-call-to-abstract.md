```yaml
number: 1923
title: "Emit diagnostic on unsound `super()` call to abstract method with trivial body"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-12-16T12:47:25Z
updated_at: 2025-12-16T16:16:33Z
url: https://github.com/astral-sh/ty/issues/1923
synced_at: 2026-01-12T15:54:26Z
```

# Emit diagnostic on unsound `super()` call to abstract method with trivial body

---

_@AlexWaygood_

[Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=2cc45538a0bb162f1f4ecd996515d985), [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBAhgRgYygSwgBzCALrOBnLEGBLCAUywAswATAKDoQBsY88oAxALjqj6gAC8AkRLkqtXvxplgUcdRoAKPGSbAAlFAC0APhQA7LFygA6cw2at2AcSUcNPflBlyFtFWs079yI0%2Bd%2BEAoAVxADKDwQtDIQJQ1Td2UNOiA) and [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEq2AxnRDcbpQC4PZxeVUTLjRhcAFrkwAddLKZRUcOHQBiiWXS10AAo36DhoiVM3bMMMHWOTMACjgwoYAJR0AtAD5W6LojqEgbLyisp0AOJ2qi4a6Np0FlY2Ug5Orh7eEL6x8fGUYgCulHFwBcQwlHYuhMn2LiAANCAFXNBwJOSIIADEdACqrVAQXKR0YAXowhC46HDBWJZjnDSoXAD66AU02BV2%2BP5ZXG5edAY52vlcRXFg0iAAcls7lP7A%2BAC%2Bd7KNIGT5YFBSIQuLQoBRegAFUj-QGnDA4Ah0JgzSAAcyKq2m6EIsl6AGUYDA6OIuFxiHBEAB6Sl-SyAwicVGUmDoSmYXBMOCU5HoNEY1ozSlLSgMABuqGgjFgSJREHRggFcVwxEV7VkZBM6HcooqcCxdAAvHQ7gBmQgARgATF90CB3k0hK0dapoDAKGgsHgiGQ7UA) all detect the unsoundness here. We should too:

```py
from abc import abstractmethod

class F:
    @abstractmethod
    def method(self) -> int: ...

class G(F):
    def method(self) -> int:
        # mypy: error: Call to abstract method "method" of "F" with trivial body via super() is unsafe  [safe-super]
        return super().method()
```

This is similar to, but distinct from, https://github.com/astral-sh/ty/issues/1877.

We should ensure that abstract properties are also covered, e.g.

```py
from abc import abstractmethod

class F:
    @property
    @abstractmethod
    def prop(self) -> int: ...

class G(F):
    @property
    def prop(self) -> int:
        # mypy: error: Call to abstract method "method" of "F" with trivial body via super() is unsafe  [safe-super]
        return super().prop
```

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-16 12:47_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-16 12:47_

---

_Comment by @AlexWaygood on 2025-12-16 14:17_

Note that the "with trivial body" part of this is important. For example, this is fine, because the abstract method has a default implementation:

```py
from abc import abstractmethod

class F:
    @abstractmethod
    def method(self) -> int:
        return 42

class G(F):
    def method(self) -> int:
        return super().method()
```

As a result of the above being fine, however, this means that we must also reject `super()` calls to abstract methods even if the `super()` call occurs in the body of an overriding method that is also abstract. To see why, consider this example below:

```py
from abc import abstractmethod

class F:
    @abstractmethod
    def method(self) -> int: ...

class G(F):
    @abstractmethod
    def method(self) -> int:
        return super().method()

class H(G):
    def method(self) -> int:
        return super().method()
```

`H.method()` says it will return `int`, but it actually returns `None`. But `H.method` doesn't break the rule outlined above: it calls `super()` on `G.method`, which is an abstract method that has a default implementation. Therefore the only way to prevent this unsoundness is to forbid the `super()` call in `G.method`, despite the fact that `G.method` is also an abstract method.

---

_Comment by @AlexWaygood on 2025-12-16 16:16_

Mypy and pyrefly also emit a diagnostic on `G.method` in this snippet, where `F.method` is not explicitly abstract, but _is_ a protocol method with a trivial body. The same soundness issues occur with this, so it makes sense that we should also emit a diagnostic here:

```py
from typing import Protocol

class F(Protocol):
    def method(self) -> int: ...

class G(F):
    def method(self) -> int:
        return super().method()
```

Pyright does not emit a diagnostic on this variation.

---
