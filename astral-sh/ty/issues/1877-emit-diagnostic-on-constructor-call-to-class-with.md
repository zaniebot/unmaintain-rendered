```yaml
number: 1877
title: Emit diagnostic on constructor call to class with abstract methods
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-12-13T13:00:59Z
updated_at: 2025-12-22T14:08:12Z
url: https://github.com/astral-sh/ty/issues/1877
synced_at: 2026-01-10T01:56:41Z
```

# Emit diagnostic on constructor call to class with abstract methods

---

_Issue opened by @AlexWaygood on 2025-12-13 13:00_

```py
from abc import abstractmethod

class Base:
    @abstractmethod
    def f(self): ...

class BadChild(Base): ...

class GoodChild(Base):
    def f(self): ...  # concrete override

Base()  # ❌ has abstract methods
BadChild()  # ❌ has abstract methods
GoodChild()  # ✅ abstract methods were overridden

class OtherBase:
    @property
    @abstractmethod
    def prop(self) -> int:
        return 42

OtherBase()  # ❌ has abstract methods

class ThirdBase:
    @property
    def prop(self) -> int:
        return 42

    @prop.setter
    @abstractmethod
    def prop(self, x: int) -> None: ...

ThirdBase()  # ❌ has abstract methods
```

The runtime only enforces this rule if the class has `abc.ABCMeta` (or a subclass) as its metaclass. But other type checkers also enforce it even if the decorator is used on a method in a class that does not have that metaclass. That's useful in stubs: there are situations where you want to indicate that you're meant to override a method (so you add the decorator), but the runtime version of the class doesn't have `ABCMeta` as the metaclass and you don't want to lie about the class's metaclass in the stub file.

Mypy's tests for this feature are at https://github.com/python/mypy/blob/master/test-data/unit/check-abstract.test

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-13 13:01_

---

_Comment by @AlexWaygood on 2025-12-15 20:34_

This check should be implemented here, where we already have a similar check for attempted `Protocol` instantiations: https://github.com/astral-sh/ruff/blob/7d3b7c57545282cebfe63215d3fba98fca34db4d/crates/ty_python_semantic/src/types/infer/builder.rs#L8505-L8521

---

_Label `typing semantics` added by @carljm on 2025-12-15 21:39_

---

_Comment by @carljm on 2025-12-15 21:39_

@AlexWaygood Is this a good candidate for "help wanted" tag?

---

_Comment by @AlexWaygood on 2025-12-15 21:45_

Not sure. Calculating exactly which methods are still abstract across the whole MRO feels like it could be finnicky. (Any superclass could add an abstract method, but any superclass could also concretize an abstract method from higher up in the hierarchy).

---

_Comment by @AlexWaygood on 2025-12-16 12:19_

interestingly, mypy specifically tests that `NewType`s of abstract classes _should_ be instantiable. I think that makes sense: <https://github.com/python/mypy/blob/d06d3d9cfd6611c0e64c0df59fc0449754e47ed8/test-data/unit/check-abstract.test#L1041-L1051>.

---

_Comment by @AlexWaygood on 2025-12-22 14:08_

https://github.com/astral-sh/ty/issues/2157 notes that as well as considering methods decorated with `@abstractmethod` as abstract, we also need to consider all methods in `Protocol` classes as implicitly abstract:

```py
from typing import Protocol

class SomeProto(Protocol):
    def method(self) -> str: ...

class ExplicitSubclass(SomeProto): ...

ExplicitSubclass()  # ❌ Unsound
```

Unlike methods explicitly decorated with `@abstractmethod`, this is never enforced at runtime, but it's necessary for us to enforce this in order to maintain soundness. Because it is a method on a `Protocol` class (which cannot be instantiated), `SomeProto.method` is explicitly excluded from the check where we enforce that methods return what they say they return. But this exemption means that we _must_ enforce that the method is overridden on concrete (non-`Protocol`) subclasses if the user wants those classes to be instantiable.

---
