```yaml
number: 22672
title: "[ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/data-decorator
created_at: 2026-01-18T01:17:41Z
updated_at: 2026-01-18T14:10:50Z
url: https://github.com/astral-sh/ruff/pull/22672
synced_at: 2026-01-18T14:22:04Z
```

# [ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2515.

Closes https://github.com/astral-sh/ty/issues/2527.


---

_Label `ty` added by @AlexWaygood on 2026-01-18 01:19_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4351 diagnostics
+ Found 4353 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-18 01:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2710 on 2026-01-18 11:58_

This method is called `is_named_tuple()`, but it doesn't just return `true` if `self` is a namedtuple class -- it also returns `true` if `self` inherits from a namedtuple class. I think this method should be called `has_named_tuple_class_in_mro()` or similar. 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:595 on 2026-01-18 11:59_

(same comment here)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1471 on 2026-01-18 12:01_

I find the parenthetical here slightly confusing, because inheriting from `NamedTuple` just creates a special `tuple` subclass at runtime, and so neither a `NamedTuple` class nor a subclass of a `NamedTuple` class actually has `NamedTuple` in its MRO

```suggestion
Classes that inherit from a `NamedTuple` class also cannot be decorated with `@dataclass`:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1480 on 2026-01-18 12:02_

maybe the error code should actually be `invalid-dataclass`? `Child` here inherits from a `NamedTuple` class, but it isn't a `NamedTuple` class itself.

We could possibly use `invalid-dataclass` for both the `NamedTuple` case and the `TypedDict` case? They both involve invalid applications of the `@dataclass` decorator

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2382 on 2026-01-18 12:07_

I'm not sure it'll lead to an `AttributeError`, but it can definitely lead to other kinds of errors. Whether it does depends on exactly how you try to instantiate it:

```pycon
>>> from typing import TypedDict
>>> from dataclasses import dataclass
>>> @dataclass
... class Foo(TypedDict):
...     x: str
...     
>>> Foo(x='x')
{'x': 'x'}
>>> Foo('x')
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    Foo('x')
    ~~~^^^^^
ValueError: dictionary update sequence element #0 has length 1; 2 is required
```

I think the better rationale when it comes to `TypedDict`s is possibly just that it's conceptually incoherent to apply `@dataclass` to a `TypedDict` class -- `TypedDict`s are abstract structural types that have no effect on the runtime class ("instantiating" a `TypedDict` always gives you a `dict` at runtime), whereas `@dataclass` is a tool for easily customising the creation of new nominal types/classes at runtime

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:701 on 2026-01-18 12:14_

you want to use the `header_range` of the class rather than the full range of the class here -- your diagnostics are currently very verbose if the `TypedDict` or `NamedTuple` class definition spans many lines of code 😄

<img width="2390" height="1426" alt="image" src="https://github.com/user-attachments/assets/0276d72e-6036-4ec3-81d7-4c29b8616f59" />


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:706 on 2026-01-18 12:15_

this is quite a long diagnostic message; I'd split the second half into a subdiagnostic

```suggestion
                    let mut diagnostic = builder.into_diagnostic(format_args!(
                        "Class `{}` inherits from `NamedTuple` and is decorated with `@dataclass`",
                        class.name(self.db()),
                    ));
                    diagnostic.info("An exception will be raised when instantiating the class at runtime");
```

and similar for the `TypedDict` case below

---

_@AlexWaygood reviewed on 2026-01-18 12:18_

Nice. I wonder if we should also flag enums and protocols decorated with `@dataclass`. Those also indicate that the user is probably very confused. And the enum docs explicitly lay out that adding `@dataclass` to an enum class is [not supported](https://docs.python.org/3/howto/enum.html#dataclass-support)

---

_@charliermarsh reviewed on 2026-01-18 13:27_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1511 on 2026-01-18 13:27_

This is turning out to be fairly annoying to fix.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1511 on 2026-01-18 13:42_

Looks like it's a pre-existing issue, though? I think fine to defer it for this PR?

---

_@AlexWaygood reviewed on 2026-01-18 13:42_

---

_@charliermarsh reviewed on 2026-01-18 13:43_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1511 on 2026-01-18 13:43_

👍 Yeah that was my plan.

---

_Converted to draft by @charliermarsh on 2026-01-18 14:09_

---
