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
draft: true
base: main
head: charlie/final
created_at: 2026-01-20T02:42:20Z
updated_at: 2026-01-20T15:28:15Z
url: https://github.com/astral-sh/ruff/pull/22753
synced_at: 2026-01-20T15:44:04Z
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
django-test-migrations (https://github.com/wemake-services/django-test-migrations)
+ django_test_migrations/db/backends/postgresql/configuration.py:12:7: error[unimplemented-abstract-method] Final class `PostgreSQLDatabaseConfiguration` does not implement abstract method `statement_timeout`
- Found 2 diagnostics
+ Found 3 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/read_only_dict.py:13:7: error[unimplemented-abstract-method] Final class `ReadOnlyDict` does not implement abstract method `__delitem__`
+ homeassistant/util/read_only_dict.py:13:7: error[unimplemented-abstract-method] Final class `ReadOnlyDict` does not implement abstract method `__setitem__`
- Found 14492 diagnostics
+ Found 14494 diagnostics


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
| `invalid-return-type` | 0 | 5 | 2 |
| `invalid-await` | 0 | 0 | 6 |
| `unimplemented-abstract-method` | 3 | 0 | 0 |
| `invalid-argument-type` | 0 | 0 | 2 |
| **Total** | **3** | **5** | **17** |


**[Full report with detailed diff](https://87e42dfb.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://87e42dfb.ty-ecosystem-ext.pages.dev/timing))



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
