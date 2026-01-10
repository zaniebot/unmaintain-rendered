```yaml
number: 725
title: "What is the right way to annotating `self` with other types without losing its original type?"
type: issue
state: closed
author: observerw
labels:
  - question
assignees: []
created_at: 2025-06-29T08:02:17Z
updated_at: 2025-06-30T03:59:27Z
url: https://github.com/astral-sh/ty/issues/725
synced_at: 2026-01-10T02:07:36Z
```

# What is the right way to annotating `self` with other types without losing its original type?

---

_Issue opened by @observerw on 2025-06-29 08:02_

### Question

When marking `self` with another type (e.g a Protocol), we can't use it to access other methods in the same class:

```python
class WithHello(Protocol):
    def hello(self) -> str: ...


class SomeClass:
    def world(self) -> str:
        return "world"

    def some_method(self: WithHello):
        self.hello()
        self.world()  # Type `WithHello` has no attribute `world` ty(unresolved-attribute)
```

Maybe we can declare a new type that is both a `WithHello` and a `SomeClass`, then use it to annotate self. That works fine by ty, but upsets Pylance, so there is a behavior inconsistent here:

```python
from __future__ import annotations

from typing import Protocol


class WithHello(Protocol):
    def hello(self) -> str: ...


class SomeClass:
    def world(self) -> str:
        return "world"

    def some_method(self: WithHelloAndSomeClass): # the type of `self` must be of super type of `SomeClass` Pylance(reportGeneralTypeIssues)
        self.hello()
        self.world()


class WithHelloAndSomeClass(WithHello, SomeClass): ...
```

So I'm not sure if this is the right way (or the only way) to achieve the desired behavior, and I can only find some limited discussions (https://github.com/python/mypy/issues/7191) about self-types and they are not helping.


### Version

_No response_

---

_Label `question` added by @observerw on 2025-06-29 08:02_

---

_Comment by @iyakushev on 2025-06-29 18:24_

Hello! This seems intended, as you've explicitly marked `self: WithHello` which only implements `def hello() -> str`. Instead, try inheriting your protocol `class SomeClass(WithHello): ...` if you want to be explicit, or just implement required functions and pass class instances to some functions/methods which require said interface `WithHello`.

```python
class WithHello(Protocol):
    def hello(self) -> str: ...


class SomeClass1:
    def hello(self) -> str: 
         return "hello"

    def world(self) -> str:
        return "world"

    def some_method(self): 
        self.hello()  # OK
        self.world()  # OK

class SomeClass2(WithHello):
    def world(self) -> str:
        return "world"

    def some_method(self): 
        self.hello()  # OK (but relies on default impl from WithHello)
        self.world()  # OK



def handler(obj: WithHello): ...

handler(SomeClass1())  # OK
handler(SomeClass2())  # OK
```

---

_Comment by @observerw on 2025-06-30 03:59_

> Hello! This seems intended, as you've explicitly marked `self: WithHello` which only implements `def hello() -> str`. Instead, try inheriting your protocol `class SomeClass(WithHello): ...` if you want to be explicit, or just implement required functions and pass class instances to some functions/methods which require said interface `WithHello`.
> 
> class WithHello(Protocol):
>     def hello(self) -> str: ...
> 
> 
> class SomeClass1:
>     def hello(self) -> str: 
>          return "hello"
> 
>     def world(self) -> str:
>         return "world"
> 
>     def some_method(self): 
>         self.hello()  # OK
>         self.world()  # OK
> 
> class SomeClass2(WithHello):
>     def world(self) -> str:
>         return "world"
> 
>     def some_method(self): 
>         self.hello()  # OK (but relies on default impl from WithHello)
>         self.world()  # OK
> 
> 
> 
> def handler(obj: WithHello): ...
> 
> handler(SomeClass1())  # OK
> handler(SomeClass2())  # OK

Oh! I forgot I can just inherit a Protocol! That solves my problem. Thank you for your kind and timely reply!

---

_Closed by @observerw on 2025-06-30 03:59_

---
