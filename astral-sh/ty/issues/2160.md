---
number: 2160
title: "Detect invalid attempts to override `@property`s with mutable attributes"
type: issue
state: open
author: AlexWaygood
labels:
  - runtime semantics
assignees: []
created_at: 2025-12-22T14:25:45Z
updated_at: 2025-12-23T11:35:24Z
url: https://github.com/astral-sh/ty/issues/2160
synced_at: 2026-01-10T01:52:52Z
---

# Detect invalid attempts to override `@property`s with mutable attributes

---

_Issue opened by @AlexWaygood on 2025-12-22 14:25_

From the perspective of the Liskov Substitution Principle, this is fine[^1], because the attribute on the subclass can do strictly more than the attribute on the superclass (it's read-only on the superclass, but it's writable on the subclass!).

However, overriding a property like this doesn't actually make the attribute writable at runtime -- so we should detect the unsoundness here:

```pycon
>>> class A:
...     @property
...     def f(self): ...
...     
>>> class B(A):
...     f: int
...     
>>> B().f = 42
Traceback (most recent call last):
  File "<python-input-11>", line 1, in <module>
    B().f = 42
    ^^^^^
AttributeError: property 'f' of 'B' object has no setter
```

In order to make a read-only property in the superclass writable, you _must_ providing an overriding _definition_ in the class body, not just an overriding _declaration_. This works:

```py
class A:
    @property
    def f(self): ...

class B(A):
    @property
    def f(self): ...

    @f.setter
    def f(self, value): ...
```

As does this:

```py
class A:
    @property
    def f(self): ...

class B(A):
    @A.f.setter
    def f(self, value): ...
```

And this too (but see footnote 1 for some caveats):

```py
class A:
    @property
    def f(self) -> int:
        return 42

class B(A):
    f: int = 42
```

but this also doesn't work:

```py
class A:
    @property
    def f(self) -> int:
        return 42

class B(A):
    def __init__(self):
        self.x: int = 42
```

This should also be disallowed, since all fields on `NamedTuple`s are implicitly `@property`s:

```py
from typing import NamedTuple

class A(NamedTuple):
    x: int

class B(A):
    x: int
```

but I think this should be fine (though pyright and pyrefly seem to disallow it, for some reason). If we do want to disallow it, I think it should be because of the caveat outlined in footnote (1) rather than because of the runtime error described above, because there's no runtime error here:

```py
from typing import NamedTuple

class A(NamedTuple):
    x: int

class B(A):
    x: int = 42
```

[^1]: Well, it's fine if you only consider the type when accessed from instances. It's not fine if you also consider the type as accessed from the class object. But anyway, I don't think that's the most important thing that's wrong about this override, so I don't think it's the thing we should actually complain about.

---

_Label `runtime semantics` added by @AlexWaygood on 2025-12-22 14:25_

---
