```yaml
number: 18404
title: "[ty] Support type annotation for legacy typing aliases for generic classes"
type: pull_request
state: merged
author: lipefree
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-annot-legacy
created_at: 2025-05-31T13:36:20Z
updated_at: 2025-06-06T01:24:27Z
url: https://github.com/astral-sh/ruff/pull/18404
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Support type annotation for legacy typing aliases for generic classes

---

_Pull request opened by @lipefree on 2025-05-31 13:36_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ty/issues/548.

This PR implements type annotations for legacy typing aliases for generic classes. 

Namely:
* `typing.List[T]`
* `typing.Dict[T, U]`
* `typing.Set[T]`
* `typing.FrozenSet[T]`
* `typing.ChainMap[T, U]`
* `typing.Counter[T]`
* `typing.DefaultDict[T, U]`
* `typing.Deque[T]`
* `typing.OrderedDict[T, U]`

Here is an example for `Dict` : 

```py
from typing import Dict

def f(x: Dict[str, int]):
    reveal_type(x)  # revealed: dict[str, int]
```

Thank you @dcreager and @carljm for helping me out!
## Test Plan

<!-- How was it tested? -->
I modified existing mdtest. I can add more cases if necessary!


---

_Review requested from @carljm by @lipefree on 2025-05-31 13:36_

---

_Review requested from @AlexWaygood by @lipefree on 2025-05-31 13:36_

---

_Review requested from @sharkdp by @lipefree on 2025-05-31 13:36_

---

_Review requested from @dcreager by @lipefree on 2025-05-31 13:36_

---

_Label `ty` added by @AlexWaygood on 2025-05-31 13:43_

---

_Comment by @AlexWaygood on 2025-05-31 18:05_

Thanks! Could you resolve the merge conflicts? It's caused by https://github.com/astral-sh/ruff/pull/18350 -- you'll just need to change `KnownInstanceType::ChainMap` to `SpecialFormType::ChainMap`, and the same for all the other branches :-)

---

_Comment by @lipefree on 2025-05-31 22:03_

I did not see it, I will do it now !

---

_Comment by @github-actions[bot] on 2025-05-31 22:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-chess (https://github.com/niklasf/python-chess)
- error[call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ error[call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int, (s: slice[Any, Any, Any], /) -> list[int]]` is not callable on object of type `list[int]`
- error[call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ error[call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int, (s: slice[Any, Any, Any], /) -> list[int]]` is not callable on object of type `list[int]`
- error[invalid-argument-type] chess/svg.py:170:19: Argument to function `_color` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] chess/svg.py:170:19: Argument to function `_color` is incorrect: Expected `str`, found `str | None`

parso (https://github.com/davidhalter/parso)
+ error[call-non-callable] parso/python/tokenize.py:107:16: Method `__getitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__getitem__(key: PythonVersionInfo, /) -> TokenCollection` is not callable on object of type `dict[PythonVersionInfo, TokenCollection]`
+ error[call-non-callable] parso/python/tokenize.py:109:9: Method `__getitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__getitem__(key: PythonVersionInfo, /) -> TokenCollection` is not callable on object of type `dict[PythonVersionInfo, TokenCollection]`
- error[invalid-parameter-default] parso/python/tokenize.py:367:5: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[invalid-parameter-default] parso/python/tokenize.py:367:5: Default value of type `None` is not assignable to annotated parameter type `list[int]`
- Found 81 diagnostics
+ Found 83 diagnostics

beartype (https://github.com/beartype/beartype)
+ error[call-non-callable] beartype/_check/error/_pep/pep484585/errpep484585container.py:62:9: Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` is not callable on object of type `dict[HintSign, range]`
+ error[call-non-callable] beartype/_check/error/_pep/pep484585/errpep484585mapping.py:56:9: Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` is not callable on object of type `dict[HintSign, range]`
- error[invalid-assignment] beartype/_check/error/errcause.py:574:13: Object of type `Unknown | None` is not assignable to `(ViolationCause, /) -> ViolationCause`
+ error[invalid-assignment] beartype/_check/error/errcause.py:574:13: Object of type `((ViolationCause, /) -> ViolationCause) | None` is not assignable to `(ViolationCause, /) -> ViolationCause`
- warning[unused-ignore-comment] beartype/_util/hint/pep/proposal/pep649.py:191:79: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] beartype/_util/hint/pep/proposal/pep649.py:204:74: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] beartype/door/_cls/util/doorclsmap.py:95:67: Unused blanket `type: ignore` directive
- Found 574 diagnostics
+ Found 573 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- warning[unused-ignore-comment] pyinstrument/__main__.py:275:35: Unused blanket `type: ignore` directive
- error[invalid-return-type] pyinstrument/stack_sampler.py:344:12: Return type does not match returned value: expected `dict[Unknown, int | float]`, found `dict[Unknown, Unknown] | dict[Unknown, int | float] | None`
+ error[invalid-return-type] pyinstrument/stack_sampler.py:344:12: Return type does not match returned value: expected `dict[Unknown, int | float]`, found `dict[Unknown, int | float] | None`
- Found 88 diagnostics
+ Found 87 diagnostics

git-revise (https://github.com/mystor/git-revise)
+ error[invalid-return-type] gitrevise/odb.py:491:20: Return type does not match returned value: expected `Self`, found `GitObj`
- Found 9 diagnostics
+ Found 10 diagnostics

kornia (https://github.com/kornia/kornia)
- error[not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict[Unknown, Unknown] | list[Unknown] | None` may not be iterable
+ error[not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict[str, Unknown] | list[ParamItem] | None` may not be iterable
+ error[invalid-argument-type] kornia/augmentation/container/image.py:381:48: Argument to function `_get_new_batch_shape` is incorrect: Expected `ParamItem`, found `str | Unknown | ParamItem`
- error[unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict[Unknown, Unknown] | list[Unknown] | None`
+ error[unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict[str, Unknown] | list[ParamItem] | None`
- error[call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `dict[Unknown, Unknown] | list[Unknown] | None`
+ error[call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]])` is not callable on object of type `dict[str, Unknown] | list[ParamItem] | None`
- error[call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `dict[Unknown, Unknown] | list[Unknown] | None`
+ error[call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]])` is not callable on object of type `dict[str, Unknown] | list[ParamItem] | None`
- error[invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: expected `dict[Unknown, Unknown]`, found `dict[Unknown, Unknown] | list[Unknown] | None`
+ error[invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: expected `dict[str, Unknown]`, found `dict[str, Unknown] | list[ParamItem] | None`
- error[invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: expected `list[Unknown]`, found `dict[Unknown, Unknown] | list[Unknown] | None`
+ error[invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: expected `list[ParamItem]`, found `dict[str, Unknown] | list[ParamItem] | None`
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:51:8: Attribute `dim` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:51:8: Attribute `dim` on type `Unknown | int | float | tuple[int | float, int | float] | list[int | float]` is possibly unbound
- error[unsupported-operator] kornia/augmentation/utils/param_validation.py:52:12: Operator `<` is not supported for types `tuple[int | float, int | float]` and `Literal[0]`, in comparing `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` with `Literal[0]`
+ error[unsupported-operator] kornia/augmentation/utils/param_validation.py:52:12: Operator `<` is not supported for types `tuple[int | float, int | float]` and `Literal[0]`, in comparing `Unknown | int | float | tuple[int | float, int | float] | list[int | float]` with `Literal[0]`
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:24: Attribute `repeat` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:24: Attribute `repeat` on type `Unknown | int | float | tuple[int | float, int | float] | list[int | float]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:70: Attribute `device` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:70: Attribute `device` on type `Unknown | int | float | tuple[int | float, int | float] | list[int | float]` is possibly unbound
- warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:91: Attribute `dtype` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:91: Attribute `dtype` on type `Unknown | int | float | tuple[int | float, int | float] | list[int | float]` is possibly unbound
- Found 940 diagnostics
+ Found 941 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- warning[possibly-unbound-attribute] pydantic/deprecated/copy_internals.py:54:24: Attribute `items` on type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-attribute] pydantic/deprecated/copy_internals.py:54:24: Attribute `items` on type `dict[str, Any] | None` is possibly unbound
- warning[possibly-unbound-attribute] pydantic/deprecated/copy_internals.py:63:52: Attribute `items` on type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-attribute] pydantic/deprecated/copy_internals.py:63:52: Attribute `items` on type `dict[str, Any] | None` is possibly unbound
- warning[possibly-unbound-implicit-call] pydantic/main.py:109:5: Method `__getitem__` of type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] pydantic/main.py:109:5: Method `__getitem__` of type `dict[str, Any] | None` is possibly unbound
- error[not-iterable] pydantic/v1/env_settings.py:240:101: Object of type `list[Unknown] | None` may not be iterable
+ error[not-iterable] pydantic/v1/env_settings.py:240:101: Object of type `list[ModelField] | None` may not be iterable
- error[invalid-parameter-default] pydantic/v1/main.py:935:5: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] pydantic/v1/main.py:935:5: Default value of type `None` is not assignable to annotated parameter type `dict[str, Unknown]`
- error[invalid-parameter-default] pydantic/v1/main.py:936:5: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] pydantic/v1/main.py:936:5: Default value of type `None` is not assignable to annotated parameter type `dict[str, Any]`
- error[invalid-parameter-default] pydantic/v1/main.py:949:5: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] pydantic/v1/main.py:949:5: Default value of type `None` is not assignable to annotated parameter type `dict[str, Unknown]`
- error[invalid-parameter-default] pydantic/v1/main.py:950:5: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] pydantic/v1/main.py:950:5: Default value of type `None` is not assignable to annotated parameter type `dict[str, Any]`
- error[invalid-parameter-default] pydantic/v1/main.py:962:5: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] pydantic/v1/main.py:962:5: Default value of type `None` is not assignable to annotated parameter type `dict[str, Unknown]`
- error[invalid-parameter-default] pydantic/v1/main.py:963:5: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] pydantic/v1/main.py:963:5: Default value of type `None` is not assignable to annotated parameter type `dict[str, Any]`
- error[invalid-argument-type] pydantic/v1/schema.py:390:52: Argument to function `get_flat_models_from_fields` is incorrect: Expected `Sequence[ModelField]`, found `list[Unknown] | None`
+ error[invalid-argument-type] pydantic/v1/schema.py:390:52: Argument to function `get_flat_models_from_fields` is incorrect: Expected `Sequence[ModelField]`, found `list[ModelField] | None`
- warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[str, ModelField] | None` is possibly unbound
- warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[str, ModelField] | None` is possibly unbound
- warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[str, ModelField] | None` is possibly unbound
- error[not-iterable] pydantic/v1/typing.py:540:22: Object of type `list[Unknown] | None` may not be iterable
+ error[not-iterable] pydantic/v1/typing.py:540:22: Object of type `list[ModelField] | None` may not be iterable

rich (https://github.com/Textualize/rich)
- error[invalid-argument-type] rich/traceback.py:737:21: Argument to function `render_scope` is incorrect: Expected `Mapping[str, Any]`, found `dict[Unknown, Unknown] | None`
+ error[invalid-argument-type] rich/traceback.py:737:21: Argument to function `render_scope` is incorrect: Expected `Mapping[str, Any]`, found `dict[str, Node] | None`
+ warning[possibly-unbound-attribute] tests/test_text.py:247:9: Attribute `link` on type `str | Style` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_text.py:261:9: Attribute `link` on type `str | Style` is possibly unbound
- Found 392 diagnostics
+ Found 394 diagnostics

alerta (https://github.com/alerta/alerta)
- error[invalid-parameter-default] alerta/app.py:48:16: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] alerta/app.py:48:16: Default value of type `None` is not assignable to annotated parameter type `dict[str, Any]`
+ error[invalid-argument-type] alerta/auth/basic.py:62:47: Argument to function `create_token` is incorrect: Expected `list[str]`, found `list[Scope]`
+ error[invalid-argument-type] alerta/auth/basic.py:103:47: Argument to function `create_token` is incorrect: Expected `list[str]`, found `list[Scope]`
+ error[invalid-argument-type] alerta/auth/basic_ldap.py:172:47: Argument to function `create_token` is incorrect: Expected `list[str]`, found `list[Scope]`
+ error[invalid-argument-type] alerta/auth/github.py:97:47: Argument to function `create_token` is incorrect: Expected `list[str]`, found `list[Scope]`
+ error[invalid-argument-type] alerta/auth/oidc.py:204:47: Argument to function `create_token` is incorrect: Expected `list[str]`, found `list[Scope]`
+ error[invalid-argument-type] alerta/auth/saml.py:114:47: Argument to function `create_token` is incorrect: Expected `list[str]`, found `list[Scope]`
- error[invalid-parameter-default] alerta/commands.py:17:17: Default value of type `None` is not assignable to annotated parameter type `dict[Unknown, Unknown]`
+ error[invalid-parameter-default] alerta/commands.py:17:17: Default value of type `None` is not assignable to annotated parameter type `dict[str, Any]`
- error[invalid-argument-type] alerta/models/alert.py:173:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/alert.py:173:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/alert.py:174:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/alert.py:174:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-parameter-default] alerta/models/alert.py:413:29: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[invalid-parameter-default] alerta/models/alert.py:413:29: Default value of type `None` is not assignable to annotated parameter type `list[str]`
- error[invalid-parameter-default] alerta/models/blackout.py:225:29: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[invalid-parameter-default] alerta/models/blackout.py:225:29: Default value of type `None` is not assignable to annotated parameter type `list[str]`
- error[invalid-argument-type] alerta/models/customer.py:44:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/customer.py:44:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/customer.py:45:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/customer.py:45:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/group.py:32:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/group.py:32:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/group.py:33:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/group.py:33:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/group.py:34:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | Unknown | None`
+ error[invalid-argument-type] alerta/models/group.py:34:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Any | None`
- error[invalid-argument-type] alerta/models/group.py:35:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/group.py:35:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/group.py:106:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/group.py:106:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/group.py:107:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/group.py:107:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-parameter-default] alerta/models/heartbeat.py:28:44: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[invalid-parameter-default] alerta/models/heartbeat.py:28:44: Default value of type `None` is not assignable to annotated parameter type `list[str]`
- error[invalid-argument-type] alerta/models/heartbeat.py:120:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/heartbeat.py:120:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/heartbeat.py:124:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/heartbeat.py:124:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `Any | None`
- error[invalid-argument-type] alerta/models/heartbeat.py:125:13: Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/heartbeat.py:125:13: Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | None`
- error[invalid-argument-type] alerta/models/heartbeat.py:129:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/heartbeat.py:129:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-parameter-default] alerta/models/heartbeat.py:161:29: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[invalid-parameter-default] alerta/models/heartbeat.py:161:29: Default value of type `None` is not assignable to annotated parameter type `list[str]`
- error[invalid-parameter-default] alerta/models/heartbeat.py:170:28: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[invalid-parameter-default] alerta/models/heartbeat.py:170:28: Default value of type `None` is not assignable to annotated parameter type `list[str]`
- error[invalid-argument-type] alerta/models/key.py:89:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:89:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/key.py:91:17: Argument to bound method `type_to_scopes` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:91:17: Argument to bound method `type_to_scopes` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/key.py:91:40: Argument to bound method `type_to_scopes` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:91:40: Argument to bound method `type_to_scopes` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/key.py:92:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:92:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/key.py:93:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:93:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `Any | None`
- error[invalid-argument-type] alerta/models/key.py:96:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:96:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/note.py:76:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/note.py:76:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/note.py:77:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/note.py:77:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/note.py:79:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/note.py:79:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/permission.py:49:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/permission.py:49:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/user.py:107:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/user.py:107:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/user.py:109:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/user.py:109:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/user.py:110:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/user.py:110:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] alerta/models/user.py:116:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/user.py:116:13: Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- error[invalid-argument-type] tests/test_scopes.py:30:17: Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `Unknown | None`
+ error[invalid-argument-type] tests/test_scopes.py:30:17: Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | None`
- Found 471 diagnostics
+ Found 477 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[invalid-argument-type] examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:360:58: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:360:58: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[call-non-callable] ignite/handlers/base_logger.py:48:20: Object of type `list[Unknown]` is not callable
+ error[call-non-callable] ignite/handlers/base_logger.py:48:20: Object of type `list[str]` is not callable
- error[not-iterable] ignite/handlers/base_logger.py:52:29: Object of type `list[Unknown] | ((str, Unknown, /) -> bool)` may not be iterable
+ error[not-iterable] ignite/handlers/base_logger.py:52:29: Object of type `list[str] | ((str, Unknown, /) -> bool)` may not be iterable
- warning[unused-ignore-comment] ignite/handlers/fbresearch_logger.py:232:76: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] ignite/handlers/time_profilers.py:729:77: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] ignite/handlers/time_profilers.py:730:75: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] ignite/metrics/vision/object_detection_average_precision_recall.py:36:12: Attribute `size` on type `Unknown | dict[str, Unknown]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/conftest.py:125:34: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/conftest.py:125:34: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- error[invalid-argument-type] tests/ignite/contrib/engines/test_common.py:395:83: Argument to function `_setup_logging` is incorrect: Expected `Engine | dict[Unknown, Unknown] | None`, found `Literal["abc"]`
+ error[invalid-argument-type] tests/ignite/contrib/engines/test_common.py:395:83: Argument to function `_setup_logging` is incorrect: Expected `Engine | dict[str, Engine] | None`, found `Literal["abc"]`
- warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:194:12: Attribute `shape` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:194:12: Attribute `shape` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:195:12: Attribute `dtype` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:195:12: Attribute `dtype` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- error[invalid-argument-type] tests/ignite/distributed/utils/__init__.py:437:25: Argument to function `new_group` is incorrect: Expected `list[Unknown]`, found `Literal[1]`
+ error[invalid-argument-type] tests/ignite/distributed/utils/__init__.py:437:25: Argument to function `new_group` is incorrect: Expected `list[int]`, found `Literal[1]`
+ warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:457:24: Attribute `item` on type `Unknown | int | float | str` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:457:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:457:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:459:24: Attribute `item` on type `Unknown | int | float | str` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:459:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:459:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:482:24: Attribute `item` on type `Unknown | int | float | str` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:482:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:482:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/utils/__init__.py:484:24: Attribute `item` on type `Unknown | int | float | str` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:484:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:484:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:19:28: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["a"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:19:28: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["a"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:19:33: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["b"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:19:33: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["b"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:19:38: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["c"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:19:38: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["c"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:38:28: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["a"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:38:28: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["a"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:38:33: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["b"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:38:33: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["b"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:38:38: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["c"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:38:38: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["c"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:57:32: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `None`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:57:32: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `None`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:60:32: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["str"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:60:32: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["str"]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:60:39: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `None`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:60:39: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `None`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:63:32: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal[1]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:63:32: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal[1]`
- error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:66:32: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `A`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:66:32: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `A`
- error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:154:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:154:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:174:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:174:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:192:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:192:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:212:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_clearml_logger.py:212:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:338:25: Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `None`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:338:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `None`
- error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:356:64: Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `Literal["abc"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:356:64: Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `Literal["abc"]`
- error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:360:84: Argument to bound method `simulate_values` is incorrect: Expected `list[Unknown] | tuple[str] | None`, found `Literal["abc"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:360:84: Argument to bound method `simulate_values` is incorrect: Expected `list[str] | tuple[str] | None`, found `Literal["abc"]`
- error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:755:42: Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `None`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:755:42: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `None`
- error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1161:29: Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `None`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1161:29: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `None`
- error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1170:72: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["ab"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1170:72: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["ab"]`
- error[invalid-argument-type] tests/ignite/handlers/test_polyaxon_logger.py:96:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_polyaxon_logger.py:96:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_polyaxon_logger.py:110:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_polyaxon_logger.py:110:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_state_param_scheduler.py:145:76: Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `None`
+ error[invalid-argument-type] tests/ignite/handlers/test_state_param_scheduler.py:145:76: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `None`
- error[invalid-argument-type] tests/ignite/handlers/test_tensorboard_logger.py:144:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_tensorboard_logger.py:144:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_tensorboard_logger.py:158:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_tensorboard_logger.py:158:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_tensorboard_logger.py:173:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_tensorboard_logger.py:173:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:191:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:193:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:243:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:245:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:289:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_start"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:331:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_complete"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:377:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_epoch_start"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:381:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:382:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:427:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_epoch_complete"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:431:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:432:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:477:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_iter_start"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:481:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:482:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:527:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_iter_complete"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:531:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:532:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:577:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_get_batch_started"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:581:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:582:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:627:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_get_batch_completed"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:631:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:632:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:653:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["do_something_once_on_2_epoch"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:674:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["do_something_once_on_2_epoch"]` with `str | int | float | tuple[str | int | float, str | int | float]`
- error[invalid-argument-type] tests/ignite/handlers/test_time_profilers.py:688:35: Argument to bound method `register_events` is incorrect: Expected `list[Unknown]`, found `Literal["custom_event"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_time_profilers.py:688:35: Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Literal["custom_event"]`
+ error[unsupported-operator] tests/ignite/handlers/test_time_profilers.py:709:12: Operator `in` is not supported for types `str` and `int`, in comparing `Literal["on_custom_event"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:712:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:713:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- error[invalid-argument-type] tests/ignite/handlers/test_wandb_logger.py:121:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_wandb_logger.py:121:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_wandb_logger.py:133:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_wandb_logger.py:133:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- error[invalid-argument-type] tests/ignite/handlers/test_wandb_logger.py:156:36: Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Literal["all"]`
+ error[invalid-argument-type] tests/ignite/handlers/test_wandb_logger.py:156:36: Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `Literal["all"]`
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:141:27: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:141:27: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:142:25: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:142:25: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:192:25: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:192:25: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:193:27: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:193:27: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:141:27: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:141:27: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:142:25: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:142:25: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:192:25: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:192:25: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:193:27: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:193:27: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:141:27: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:141:27: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:142:25: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:142:25: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:192:25: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:192:25: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:193:27: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/clustering/test_silhouette_score.py:193:27: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:112:21: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:112:21: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:113:16: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:113:16: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:159:21: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:159:21: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:160:22: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_canberra_metric.py:160:22: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:106:16: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:106:16: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:107:21: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:107:21: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:156:16: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:156:16: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:157:21: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_absolute_error.py:157:21: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_bias.py:127:21: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_bias.py:127:21: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_bias.py:128:16: Attribute `cpu` on type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/regression/test_fractional_bias.py:128:16: Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- error[invalid-argument-type] tests/ignite/metrics/regression/test_fractional_bias.py:133:22: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[Unknown]`
+ error[invalid-argument-type] tests/ignite/metrics/regression/test_fractional_bias.py:133:22: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:159:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py...*[Comment body truncated]*

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:34 on 2025-06-02 13:14_

Can you add an example of each of these aliases being specialized with the wrong number of arguments?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8870 on 2025-06-02 13:14_

And here add a TODO comment that we want to add more specific diagnostics here for arity mismatches.

(And it doesn't need to be included in the comment text, but there's a fair bit of repetition in all of these match arms. So when we do tackle this follow-on work and add diagnostics, I could see it being enough at that point to be worth factoring out into a helper method.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8905 on 2025-06-02 13:17_

this gets us the right answer if we have the right number of type arguments provided. Unfortunately, however, it doesn't cause us to emit a diagnostic if the wrong number of type arguments is provided. If one or three arguments are provided to `Dict` (`Dict[int]` or `Dict[str, bytes, bool]`), we need to emit an error telling the user that they made a mistake.

There's the same issue for all of these branches, but each branch expects a different number of arguments, so I suggest you do something like this:

```diff
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index e931013755..85d8ab92e9 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -1849,6 +1849,21 @@ pub(crate) fn report_invalid_arguments_to_annotated(
     ));
 }
 
+pub(crate) fn report_invalid_arguments_to_legacy_alias(
+    context: &InferContext,
+    subscript: &ast::ExprSubscript,
+    alias: SpecialFormType,
+    expected_number: usize,
+    actual_number: usize,
+) {
+    let Some(builder) = context.report_lint(&INVALID_TYPE_FORM, subscript) else {
+        return;
+    };
+    builder.into_diagnostic(format_args!(
+        "Legacy alias `{alias}` expected exactly {expected_number} arguments, got {actual_number}"
+    ));
+}
+
 pub(crate) fn report_bad_argument_to_get_protocol_members(
     context: &InferContext,
     call: &ast::ExprCall,
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index 12e455c9bf..4f24714fda 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -104,7 +104,8 @@ use super::diagnostic::{
     INVALID_METACLASS, INVALID_OVERLOAD, INVALID_PROTOCOL, REDUNDANT_CAST, STATIC_ASSERT_ERROR,
     SUBCLASS_OF_FINAL_CLASS, TYPE_ASSERTION_FAILURE, report_attempted_protocol_instantiation,
     report_bad_argument_to_get_protocol_members, report_duplicate_bases,
-    report_index_out_of_bounds, report_invalid_exception_caught, report_invalid_exception_cause,
+    report_index_out_of_bounds, report_invalid_arguments_to_legacy_alias,
+    report_invalid_exception_caught, report_invalid_exception_cause,
     report_invalid_exception_raised, report_invalid_or_unsupported_base,
     report_invalid_type_checking_constant, report_non_subscriptable,
     report_possibly_unresolved_reference,
@@ -8869,6 +8870,15 @@ impl<'db> TypeInferenceBuilder<'db> {
 
             SpecialFormType::ChainMap => match arguments_slice {
                 ast::Expr::Tuple(t) => {
+                    if t.len() != 2 {
+                        report_invalid_arguments_to_legacy_alias(
+                            &self.context,
+                            subscript,
+                            SpecialFormType::ChainMap,
+                            2,
+                            t.len(),
+                        );
+                    }
                     let args_ty = t.elts.iter().map(|elt| self.infer_type_expression(elt));
                     let ty = KnownClass::ChainMap.to_specialized_instance(db, args_ty);
                     self.store_expression_type(arguments_slice, ty);
@@ -8876,6 +8886,13 @@ impl<'db> TypeInferenceBuilder<'db> {
                 }
                 _ => {
                     self.infer_type_expression(arguments_slice);
+                    report_invalid_arguments_to_legacy_alias(
+                        &self.context,
+                        subscript,
+                        SpecialFormType::ChainMap,
+                        2,
+                        1,
+                    );
                     KnownClass::ChainMap.to_instance(db)
                 }
             },
```

---

_@dcreager reviewed on 2025-06-02 13:17_

This looks great! Just one final recommendation related to the suggestion to use `to_specialized_instance`'s diagnostics for now, but that we'd want more custom diagnostics as follow-on work

---

_@AlexWaygood reviewed on 2025-06-02 13:17_

Thank you! This looks like it's on the right track to me

---

_@AlexWaygood reviewed on 2025-06-02 13:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8905 on 2025-06-02 13:20_

oops, I see I posted this simultaneously to @dcreager's review comments! I'm okay with just adding a TODO regarding the diagnostics, as he suggests, instead of adding them now

---

_@dcreager reviewed on 2025-06-02 13:34_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8905 on 2025-06-02 13:34_

> this gets us the right answer if we have the right number of type arguments provided. Unfortunately, however, it doesn't cause us to emit a diagnostic if the wrong number of type arguments is provided. If one or three arguments are provided to `Dict` (`Dict[int]` or `Dict[str, bytes, bool]`), we need to emit an error telling the user that they made a mistake.

I think we are emitting _something_, it's just that we're relying on a diagnostic coming from `to_specialized_instance`, which actually produces an `ERROR`-level log message instead of a proper diagnostic. (The intent was that this is more likely the result of a custom typeshed installing e.g. a `list` that is not parameterized with one typevar, and so we only wanted to output the message once instead of every single place that `list` appears.)

Also, @lipefree did this at my suggestion (https://github.com/astral-sh/ty/issues/548#issuecomment-2922512743). I agree that we need to address this but had suggested it as a follow-on — but if you feel that it should be folded into this PR I'm :+1: to that too.

---

_@dcreager reviewed on 2025-06-02 13:35_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8905 on 2025-06-02 13:35_

Er, double-jinx! :sweat_smile: 

---

_@AlexWaygood reviewed on 2025-06-02 13:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8905 on 2025-06-02 13:37_

Nope, I'm definitely okay with leaving diagnostics to a followup, as long as we keep track of the TODO!

---

_@lipefree reviewed on 2025-06-02 14:56_

---

_Review comment by @lipefree on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:34 on 2025-06-02 14:56_

Yes, I will add them !

---

_@lipefree reviewed on 2025-06-02 15:03_

---

_Review comment by @lipefree on `crates/ty_python_semantic/src/types/infer.rs`:8905 on 2025-06-02 15:03_

If the snippet @AlexWaygood provided above is enough (checking arity and provide the new diagnosis `report_invalid_arguments_to_legacy_alias`), then I am more than happy to cover the case of arity mismatch in this PR aswell and adapt my tests. I feel like having a TODO when I could revolve the task now just adds more review time for everyone.

---

_Comment by @lipefree on 2025-06-03 07:23_

I feel like factoring in a helper function as @dcreager suggested may already be useful but I don't know where to define it

---

_@AlexWaygood approved on 2025-06-03 11:06_

Thanks @lipefree!!

This was a bit trickier than I realised when I wroteup the issue. I ended up refactoring your logic a little bit to make it a bit less repetitive. But you had the basic idea exactly right!

---

_Merged by @AlexWaygood on 2025-06-03 11:09_

---

_Closed by @AlexWaygood on 2025-06-03 11:09_

---

_Comment by @lipefree on 2025-06-03 13:03_

Thank you for helping me closing this issue @AlexWaygood !

---

_@dcreager reviewed on 2025-06-03 13:20_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8666 on 2025-06-03 13:20_

nit: I think you could also have done this with `std::slice::from_ref` for the non-tuple case, so that in both cases you would end up with a slice iterator. Then you wouldn't need `Either`. (That would be a probably-not-actually-noticeable performance improvement, since iterating over an `Either` requires a conditional check on whether it's `Left` or `Right` at each step.)

---

_@dhruvmanila reviewed on 2025-06-03 16:15_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:8912 on 2025-06-03 16:15_

nit: It seems like we could reduce the number of parameters here by:
1. Implement a `as_known_class` on `SpecialFormType` that converts it into `KnownClass`
2. Implement a `required_argument_count` on `SpecialFormType` that returns the number of required arguments

And, that would also then mean that we can combine all of these match arms into a single arm that calls into the method:
```rs
SpecialFormType::ChainMap | SpecialFormType::OrderedDict | ... => {
  self.infer_parameterized_legacy_typing_alias(subscript, special_form);
}
```

---

_@AlexWaygood reviewed on 2025-06-03 16:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8912 on 2025-06-03 16:30_

> Implement a `as_known_class` on `SpecialFormType` that converts it into `KnownClass`

I considered adding a `SpecialFormType::as_legacy_alias()` method. But it would have to return `Option<KnownClass>`, because many of the `SpecialFormType` variants are not legacy aliases -- the new method would do something quite different to the existing `SpecialFormType::class()` method.

Note that the legacy aliases are distinct objects to the objects they alias, and behave differently in several contexts, so it's important for them to have different representations in our model:

```py
>>> import typing as t
>>> t.Dict is dict
False
>>> t.Dict[int, str]
typing.Dict[int, str]
>>> isinstance({}, t.Dict)
True
>>> t.Dict()
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    t.Dict()
    ~~~~~~^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1315, in __call__
    raise TypeError(f"Type {self._name} cannot be instantiated; "
                    f"use {self.__origin__.__name__}() instead")
TypeError: Type Dict cannot be instantiated; use dict() instead
>>> t.Dict[int, str]()
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    t.Dict[int, str]()
    ~~~~~~~~~~~~~~~~^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1315, in __call__
    raise TypeError(f"Type {self._name} cannot be instantiated; "
                    f"use {self.__origin__.__name__}() instead")
TypeError: Type Dict cannot be instantiated; use dict() instead
>>> dict()
{}
>>> dict[int, str]()
{}
```

So if you added a `SpecialFormType::as_legacy_alias()` method, you would end up having to call `.unwrap()` or `.expect()` on the result of `SpecialFormType::as_legacy_alias()` at its sole point of use in this branch, which felt a bit silly.

---

_@dhruvmanila reviewed on 2025-06-04 07:17_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:8912 on 2025-06-04 07:17_

Ah, that makes sense. Thank you for explaining!

---

_@carljm reviewed on 2025-06-06 01:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8666 on 2025-06-06 01:24_

https://github.com/astral-sh/ruff/pull/18489

---
