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
updated_at: 2026-01-20T14:08:21Z
url: https://github.com/astral-sh/ruff/pull/22753
synced_at: 2026-01-20T14:40:45Z
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
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4452 diagnostics
+ Found 4450 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

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
| `invalid-argument-type` | 2 | 1 | 4 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 0 | 0 | 5 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **2** | **3** | **14** |


**[Full report with detailed diff](https://570e1361.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://570e1361.ty-ecosystem-ext.pages.dev/timing))



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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1084 on 2026-01-20 14:01_

we should add a branch (and tests) for `Type::PropertyInstance` too -- if the getter is abstract then the class can't be instantiated. (I can't remember what happens if the setter is decorated with `@abstractmethod` but the getter is not.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1107 on 2026-01-20 14:04_

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
