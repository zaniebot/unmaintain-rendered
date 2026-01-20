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
updated_at: 2026-01-20T16:13:59Z
url: https://github.com/astral-sh/ruff/pull/22753
synced_at: 2026-01-20T16:46:59Z
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
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1821 diagnostics
+ Found 1825 diagnostics

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
| `invalid-argument-type` | 61 | 0 | 3 |
| `possibly-missing-attribute` | 6 | 0 | 2 |
| `invalid-await` | 7 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 5 |
| `invalid-assignment` | 0 | 0 | 5 |
| **Total** | **75** | **0** | **15** |


**[Full report with detailed diff](https://3b7a3276.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://3b7a3276.ty-ecosystem-ext.pages.dev/timing))



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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1093 on 2026-01-20 14:01_

we should add a branch (and tests) for `Type::PropertyInstance` too -- if the getter is abstract then the class can't be instantiated. (I can't remember what happens if the setter is decorated with `@abstractmethod` but the getter is not.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1116 on 2026-01-20 14:04_

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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:753 on 2026-01-20 15:57_

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
