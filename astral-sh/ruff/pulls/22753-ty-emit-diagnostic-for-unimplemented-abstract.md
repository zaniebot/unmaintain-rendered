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
updated_at: 2026-01-21T11:19:19Z
url: https://github.com/astral-sh/ruff/pull/22753
synced_at: 2026-01-21T11:58:43Z
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
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5411 diagnostics
+ Found 5406 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4454 diagnostics
+ Found 4456 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




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
| `invalid-return-type` | 4 | 0 | 6 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 1 | 3 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **5** | **6** | **17** |


**[Full report with detailed diff](https://f3fa6d9f.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://f3fa6d9f.ty-ecosystem-ext.pages.dev/timing))



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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:781 on 2026-01-20 15:57_

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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:840 on 2026-01-20 17:04_

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

_@charliermarsh reviewed on 2026-01-20 20:13_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:1306 on 2026-01-20 20:13_

I personally find this more useful + consistent with our other patterns, but I don't feel strongly.

---

_@AlexWaygood reviewed on 2026-01-21 11:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1306 on 2026-01-21 11:08_

I know there's still a bunch of rules where we emit a separate diagnostic for each element in cases like this, but in general I think we aspire to group them together where possible -- e.g. here we only emit a single diagnostic, even though multiple elements of the union have the incorrect signature:

```
% uvx ty check foo.py
error[unknown-argument]: Argument `x` does not match any known parameter of function `f`
 --> foo.py:7:10
  |
5 | def i(coinflip: bool):
6 |     func = f if coinflip else g if coinflip else h
7 |     func(x=42)
  |          ^^^^
  |
info: Union variant `def f() -> Unknown` is incompatible with this call site
info: Attempted to call union type `(def f() -> Unknown) | (def h(x) -> Unknown)`
info: rule `unknown-argument` is enabled by default

Found 1 diagnostic
% cat foo.py
def f(): ...
def g(): ...
def h(x): ...

def i(coinflip: bool):
    func = f if coinflip else g if coinflip else h
    func(x=42)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:681 on 2026-01-21 11:10_

(this can be a followup): we also need to implement the rule where all methods on `Protocol` classes that have "trivial bodies" (either `...`, `pass`, a docstring, or a combination of those three elements) and have an explicit non-`None` return type are treated as implicitly abstract even if they are not decorated with `@abstractmethod`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/final.md`:816 on 2026-01-21 11:15_

I don't think we need to prioritise supporting them since they're deprecated, but you could add a note here about `abc.abstractproperty`, `abc.abstractclassmethod` and `abc.abstractstaticmethod`.

Hmm, and it doesn't look like you have any tests for `@classmethod` or `@staticmethod` stacked on top of `@abstractmethod`. We should _definitely_ add those tests!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1909 on 2026-01-21 11:17_

I'm not totally sure this is the best name:
- (minor) there might be multiple abstract methods that are unimplemented
- (more important) this could equally well be a good name for the rule https://github.com/astral-sh/ty/issues/1877 discusses, but I think that should probably be a separate error code, so we should probably choose a name here that more clearly differentiates the two rules

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1315 on 2026-01-21 11:18_

you might be able to use this nice helper we have: https://github.com/astral-sh/ruff/blob/4e9de95cd83e6ffd4b1af7f4fd36af959c4cac07/crates/ty_python_semantic/src/diagnostic.rs#L127-L149

---

_@AlexWaygood approved on 2026-01-21 11:19_

This looks great, thank you

---
