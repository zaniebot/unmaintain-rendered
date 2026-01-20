```yaml
number: 22753
title: "[ty] Emit diagnostic for unimplemented abstract method on @final class"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/final
created_at: 2026-01-20T02:42:20Z
updated_at: 2026-01-20T19:27:06Z
url: https://github.com/astral-sh/ruff/pull/22753
synced_at: 2026-01-20T20:43:31Z
```

# [ty] Emit diagnostic for unimplemented abstract method on @final class

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2525.


---

_Comment by @astral-sh-bot[bot] on 2026-01-20 02:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 02:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14492 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @charliermarsh on 2026-01-20 02:47_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-20 02:47_

---

_Marked ready for review by @charliermarsh on 2026-01-20 02:50_

---

_Review requested from @carljm by @charliermarsh on 2026-01-20 02:51_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 02:51_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-20 02:51_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-20 02:51_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-20 02:51_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 02:53_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-argument-type` | 1 | 2 | 2 |
| `type-assertion-failure` | 2 | 0 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| `invalid-return-type` | 0 | 0 | 1 |
| **Total** | **5** | **2** | **10** |


**[Full report with detailed diff](https://d5ad750a.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://d5ad750a.ty-ecosystem-ext.pages.dev/timing))



---

_@MichaReiser reviewed on 2026-01-20 07:03_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/final.md_-_Tests_for_the_`@typi…_-_A_`@final`_class_mus…_-_Multiple_abstract_me…_(feafee9a4abbe8d1).snap`:54 on 2026-01-20 07:03_

Should we add a secondary annotation instead that points to the definition of `foo`? The info message is helpful, but it still requires me to lookup `Base`, find `foo`, to understand the method signature

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1317 on 2026-01-20 12:50_

There are quite a few as-yet unimplemented rules where we need to know the full set of unimplemented abstract methods a class might have (https://github.com/astral-sh/ty/issues/1927, https://github.com/astral-sh/ty/issues/1923, and https://github.com/astral-sh/ty/issues/1877). I think it would best to extract this into a standalone method on `ClassType` (probably Salsa-cached, so we don't repeatedly reconstruct the `FxHashMap`, which seems expensive).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1319 on 2026-01-20 12:52_

What's the reason for calling `.skip(1)` here? This means that your branch doesn't emit an error on this, but it seems just as invalid as having an unimplemented method on a parent or grandparent class:

```py
from typing import final
from abc import abstractmethod, ABC

@final
class F(ABC):
    @abstractmethod
    def g(self): ...
```



---

_@AlexWaygood reviewed on 2026-01-20 12:52_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 13:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1094 on 2026-01-20 14:01_

we should add a branch (and tests) for `Type::PropertyInstance` too -- if the getter is abstract then the class can't be instantiated. (I can't remember what happens if the setter is decorated with `@abstractmethod` but the getter is not.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1132 on 2026-01-20 14:04_

It looks like this logic means you don't emit a diagnostic on this:

```py
from abc import ABC, abstractmethod
from typing import final

class Foo(ABC):
    @abstractmethod
    def method(self): ...

@final
class Bar(Foo):
    method: int
```

But the annotation in the subclass there doesn't actually override the abstract method in the superclass; attempting to instantiate `Bar` will still cause an error at runtime

---

_@AlexWaygood reviewed on 2026-01-20 14:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1326 on 2026-01-20 14:08_

missing a let-chain opportunity :P

```suggestion
            if !is_implemented && let Some(builder) = self
                .context
                .report_lint(&UNIMPLEMENTED_ABSTRACT_METHOD, &class_node.name)
            {
```

---

_@AlexWaygood reviewed on 2026-01-20 14:08_

---

_Converted to draft by @charliermarsh on 2026-01-20 15:03_

---

_Marked ready for review by @charliermarsh on 2026-01-20 15:09_

---

_Comment by @AlexWaygood on 2026-01-20 15:12_

now you're emitting false-positive errors on classes like this, which _can_ be instantiated at runtime 😄

```py
from abc import abstractmethod, ABC
from typing import final

class Base(ABC):
    @property
    @abstractmethod
    def f(self) -> int: ...

@final
class Child(Base):
    f = 42
```

The difference between `Child` and a class like this:

```py
@final
class BadChild(Base):
    f: int
```

is that `Child.f` has a _definition_ whereas `BadChild.f` only has a _declaration_

---

_Comment by @charliermarsh on 2026-01-20 15:15_

Oh gosh.

---

_Converted to draft by @charliermarsh on 2026-01-20 15:28_

---

_@charliermarsh reviewed on 2026-01-20 15:44_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/place.rs`:301 on 2026-01-20 15:44_

I think we were losing the origin here (unintentionally?).

---

_Marked ready for review by @charliermarsh on 2026-01-20 15:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:783 on 2026-01-20 15:57_

(but we should emit an error when such a class is _instantiated_, of course -- which is https://github.com/astral-sh/ty/issues/1877; it doesn't need to be implemented here)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:760 on 2026-01-20 16:11_

`Protocol`s also actually have a custom metaclass, we just haven't implemented that yet (which is https://github.com/astral-sh/ty/issues/1204 -- see https://github.com/astral-sh/ty/issues/1204#issuecomment-3308633844). `MyEnum` fails at runtime:

```pycon
>>> from abc import abstractmethod
... from enum import Enum
... from typing import Protocol
... 
... class Stringable(Protocol):
...     @abstractmethod
...     def stringify(self) -> str: ...
... 
... class MyEnum(Stringable, Enum):  # error: [unimplemented-abstract-method]
...     A = 1
...     B = 2
...     
Traceback (most recent call last):
  File "<python-input-0>", line 9, in <module>
    class MyEnum(Stringable, Enum):  # error: [unimplemented-abstract-method]
        A = 1
        B = 2
TypeError: metaclass conflict: the metaclass of a derived class must be a (non-strict) subclass of the metaclasses of all its bases
```

but you can get it to work if you create a custom metaclass that mutually subclasses `EnumMeta` and `ABCMeta`:

```pycon
>>> from enum import EnumMeta
>>> from abc import ABCMeta, ABC
>>> class Stringable(ABC):
...     @abstractmethod
...     def stringify(self) -> str: ...
...     
>>> class AbstractEnumMeta(ABCMeta, EnumMeta): ...
... 
>>> class MyEnum(Stringable, Enum, metaclass=AbstractEnumMeta):
...     A = 1
...     B = 2
...
>>>
```

But combining the metaclasses like that appears to break the checking that `ABCMeta` does to prohibit instantiation of classes with namespace packages:

```pycon     
>>> MyEnum(1)
<MyEnum.A: 1>
```

But anyway, it doesn't look like you're looking at the metaclasses when deciding whether a method should be considered as an abstract method? Which I think is correct -- it matches mypy's behaviour, and I don't think a type checker should care that this class doesn't have `ABCMeta` as its metaclass; the intention is clear that the method should be implemented if a subclass wants to be considered a concrete, instantiable method:

```py
from abc import abstractmethod

class Foo:
    @abstractmethod
    def method(self): ...
```

So TL;DR, I'd get rid of the `Protocol` base class and the prose describing why we have `Protocol` as a base class here; I think it's unnecessary :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:1 on 2026-01-20 16:13_

Can you also add these tests (no errors should be emitted):

```py
from abc import abstractmethod, ABC

class Base(ABC):
    @property
    @abstractmethod
    def foo(self) -> int: ...

class Child1(Base):
    foo = 42

class Child2(Base):
    foo: int = 42
```

---

_@AlexWaygood reviewed on 2026-01-20 16:13_

---

_@AlexWaygood reviewed on 2026-01-20 17:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:842 on 2026-01-20 17:04_

We could also add a failing test for properties with abstract deleters (we don't support property deleters at all yet, which is why it would not currently pass):

```pycon
>>> class F(ABC):
...     @property
...     def g(self): ...
...     @g.setter
...     def g(self, value): ...
...     @g.deleter
...     @abstractmethod
...     def g(self): ...
...     
>>> F()
Traceback (most recent call last):
  File "<python-input-16>", line 1, in <module>
    F()
    ~^^
TypeError: Can't instantiate abstract class F without an implementation for abstract method 'g'
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1074 on 2026-01-20 17:09_

I find it a bit confusing that this method returns a map of "abstract methods", but then it's up to the caller to figure out if they're still abstract (because they might only be abstract on a grandparent class; they could have then been overridden with a concrete method on a parent class). My instinct would be to have the returned map only return methods that are _still_ abstract on `self`.

---

_@AlexWaygood reviewed on 2026-01-20 17:09_

---

_@AlexWaygood reviewed on 2026-01-20 17:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:1 on 2026-01-20 17:12_

Can you also add a test like this, where an abstract method is overridden as concrete but then overridden again as abstract:

```py
from abc import abstractmethod, ABC
from typing import final

class GreatGrandparent(ABC):
    @abstractmethod
    def f(self): ...

class GrandParent(GreatGrandparent):
    def f(self): ...

class Parent(GrandParent):
    @abstractmethod
    def f(self): ...

@final
class Child(Parent): ...  # error

---

_@AlexWaygood reviewed on 2026-01-20 17:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:301 on 2026-01-20 17:14_

yes, this looks correct! good catch

---

_@AlexWaygood reviewed on 2026-01-20 17:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1889 on 2026-01-20 17:21_

Your docs here mention classes that inherit from `ABC` or `Protocol`. But at runtime, any class with `ABCMeta` (or a subclass thereof) as its metaclass cannot be instantiated if it has unimplemented abstract methods; it doesn't have to have `ABC` or `Protocol` in its MRO. And at type-checking time, you're not currently checking the MRO or the metaclass at all.

As I said in https://github.com/astral-sh/ruff/pull/22753/files#r2709058042, I think that's correct behaviour for a type checker:
- It matches mypy
- The intent for the class to be abstract is clear even if it won't be enforced at runtime
- It's useful for stub authors if they want to indicate that you really _should_ override a certain method on a particular class when you subclass it -- authors of stubs generally don't want to pretend that a class has `ABCMeta` as its metaclass if it doesn't at runtime, but they might want to nonetheless mark a method as abstract in the stubs to help out users of the stubs

But if we don't want to change our behaviour here, we should change the docs to make it clear that even though abstractness at runtime is only enforced for classes with `ABCMeta` (or a subclass thereof) as their metaclass, we enforce it at type-checking for all classes regardless of their metaclass

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1406 on 2026-01-20 17:21_

```suggestion
                            .message(format_args!(
                                "`{method_name}` defined as abstract on superclass `{defining_class}`",
                                defining_class = defining_class.name(db)
                            )),
```

---

_@AlexWaygood reviewed on 2026-01-20 17:22_

---

_Converted to draft by @charliermarsh on 2026-01-20 18:46_

---

_@charliermarsh reviewed on 2026-01-20 19:11_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:1074 on 2026-01-20 19:11_

Moved that logic into `abstract_methods`.

---

_Marked ready for review by @charliermarsh on 2026-01-20 19:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1075 on 2026-01-20 19:23_

I think there are probably marginally more efficient ways of doing this with a single MRO iteration (if we iterated through the MRO in reverse order), but this also seems fine for a first pass

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1306 on 2026-01-20 19:26_

if there are multiple unimplemented abstract methods, do we really want to emit a separate diagnostic for each one? Wouldn't it be better to collect a list of all the unimplemented abstract methods and then emit a single diagnostic saying "Methods `foo`, `bar`, `baz` are all abstract"? (But I think it would be fine to only add a subdiagnostic pointing to the definition of the first one; too many subdiagnostics would be noisy.)

---

_@AlexWaygood reviewed on 2026-01-20 19:27_

---
