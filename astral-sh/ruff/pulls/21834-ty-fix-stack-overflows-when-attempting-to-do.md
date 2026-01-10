```yaml
number: 21834
title: "[ty] Fix stack overflows when attempting to do subtyping checks involving TypeVars with invalid recursive bounds or constraints"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: alex/recursive-typevar-panic
created_at: 2025-12-07T18:20:53Z
updated_at: 2025-12-11T21:44:28Z
url: https://github.com/astral-sh/ruff/pull/21834
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Fix stack overflows when attempting to do subtyping checks involving TypeVars with invalid recursive bounds or constraints

---

_Pull request opened by @AlexWaygood on 2025-12-07 18:20_

## Summary

This PR fixes the stack overflows on the second snippet in https://github.com/astral-sh/ty/issues/1794

## Test Plan

I added two corpus tests that both cause us to overflow our stack on `main`


---

_Review requested from @carljm by @AlexWaygood on 2025-12-07 18:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-07 18:20_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-07 18:20_

---

_Label `bug` added by @AlexWaygood on 2025-12-07 18:20_

---

_Label `ty` added by @AlexWaygood on 2025-12-07 18:20_

---

_Comment by @astral-sh-bot[bot] on 2025-12-07 18:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-07 18:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_fileio.py:201:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@wrap_file` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/anyio/_core/_fileio.py:201:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `IO[AnyStr@wrap_file]`
+ src/anyio/_core/_tempfile.py:108:9: error[invalid-assignment] Object of type `AsyncFile[str]` is not assignable to attribute `_async_file` of type `AsyncFile[AnyStr@TemporaryFile]`
+ src/anyio/_core/_tempfile.py:208:9: error[invalid-assignment] Object of type `AsyncFile[str]` is not assignable to attribute `_async_file` of type `AsyncFile[AnyStr@NamedTemporaryFile]`
- Found 90 diagnostics
+ Found 92 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/util.py:65:30: error[invalid-argument-type] Argument to function `file_is_a_tty` is incorrect: Argument type `AnyStr@file_supports_color` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ pyinstrument/util.py:65:30: error[invalid-argument-type] Argument to function `file_is_a_tty` is incorrect: Expected `IO[str]`, found `IO[AnyStr@file_supports_color]`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/progress.py:294:16: error[invalid-argument-type] Argument to bound method `__enter__` is incorrect: Expected `TextIO`, found `_I@_ReadContext`
- Found 601 diagnostics
+ Found 600 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/capture.py:940:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- src/_pytest/capture.py:941:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/_pytest/capture.py:940:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `CaptureBase[str] | None`, found `CaptureBase[AnyStr@CaptureFixture]`
+ src/_pytest/capture.py:941:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `CaptureBase[str] | None`, found `CaptureBase[AnyStr@CaptureFixture]`
+ src/_pytest/capture.py:968:16: error[invalid-return-type] Return type does not match returned value: expected `CaptureResult[AnyStr@CaptureFixture]`, found `CaptureResult[str]`
- src/_pytest/capture.py:968:30: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture | Unknown` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- src/_pytest/capture.py:968:44: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture | Unknown` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 446 diagnostics
+ Found 445 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/unicode.py:175:39: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
- pylint/checkers/unicode.py:175:53: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str | tuple[str, ...]`, found `_StrLike@_map_positions_to_result`
- pylint/checkers/unicode.py:181:15: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
- pylint/checkers/unicode.py:181:25: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
- pylint/checkers/unicode.py:188:19: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
- pylint/checkers/unicode.py:188:29: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
- Found 212 diagnostics
+ Found 206 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/http/headers.py:40:42: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `str | int | AnyStr@update` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ scrapy/http/headers.py:40:42: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `str | int` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/http/headers.py:72:60: error[invalid-argument-type] Argument to bound method `__getitem__` is incorrect: Argument type `AnyStr@__getitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/http/headers.py:78:52: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `AnyStr@get` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/http/headers.py:84:60: error[invalid-argument-type] Argument to bound method `__getitem__` is incorrect: Argument type `AnyStr@getlist` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/http/headers.py:91:9: error[invalid-assignment] Invalid subscript assignment with key of type `AnyStr@setlist` and value of type `Iterable[bytes | str | int]` on object of type `Self@setlist`
- scrapy/http/headers.py:96:32: error[invalid-argument-type] Argument to bound method `setdefault` is incorrect: Argument type `AnyStr@setlistdefault` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/http/headers.py:99:28: error[invalid-argument-type] Argument to bound method `getlist` is incorrect: Argument type `AnyStr@appendlist` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/http/headers.py:101:9: error[invalid-assignment] Invalid subscript assignment with key of type `AnyStr@appendlist` and value of type `list[bytes]` on object of type `Self@appendlist`
- scrapy/utils/datatypes.py:56:52: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `AnyStr@__getitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:59:45: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `AnyStr@__setitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:62:45: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `AnyStr@__delitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:65:53: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `AnyStr@__contains__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@normkey`, found `Unknown | bytes`
+ scrapy/utils/datatypes.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@normkey`, found `str | bytes`
- scrapy/utils/datatypes.py:83:44: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `AnyStr@get` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:91:31: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `str | int | AnyStr@update` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ scrapy/utils/datatypes.py:91:31: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `str | int` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:99:44: error[invalid-argument-type] Argument to bound method `normkey` is incorrect: Argument type `AnyStr@pop` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:112:40: error[invalid-argument-type] Argument to bound method `_normkey` is incorrect: Argument type `AnyStr@__getitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:116:40: error[invalid-argument-type] Argument to bound method `_normkey` is incorrect: Argument type `AnyStr@__setitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:126:40: error[invalid-argument-type] Argument to bound method `_normkey` is incorrect: Argument type `AnyStr@__delitem__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- scrapy/utils/datatypes.py:131:40: error[invalid-argument-type] Argument to bound method `_normkey` is incorrect: Argument type `AnyStr@__contains__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 1735 diagnostics
+ Found 1718 diagnostics

rich (https://github.com/Textualize/rich)
- rich/progress.py:294:16: error[invalid-argument-type] Argument to bound method `__enter__` is incorrect: Expected `TextIO`, found `_I@_ReadContext`
- Found 347 diagnostics
+ Found 346 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/net/check.py:17:26: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `AnyStr@is_valid_host`
- mitmproxy/net/http/url.py:147:21: error[invalid-argument-type] Argument to function `default_port` is incorrect: Argument type `AnyStr@hostport` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- test/mitmproxy/net/http/test_url.py:193:28: error[invalid-argument-type] Argument to function `parse_authority` is incorrect: Argument type `AnyStr@test_parse_authority` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- test/mitmproxy/net/http/test_url.py:196:32: error[invalid-argument-type] Argument to function `parse_authority` is incorrect: Argument type `AnyStr@test_parse_authority` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- test/mitmproxy/net/http/test_url.py:199:29: error[invalid-argument-type] Argument to function `parse_authority` is incorrect: Argument type `AnyStr@test_parse_authority` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 2139 diagnostics
+ Found 2136 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/datasets/utils.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `T@verify_str_arg`, found `str`
- torchvision/datasets/utils.py:426:12: error[invalid-return-type] Return type does not match returned value: expected `T@verify_str_arg`, found `str`
- Found 1474 diagnostics
+ Found 1472 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:152:56: error[invalid-argument-type] Argument is incorrect: Argument type `ExactMatchT@EqualsValidator` does not satisfy constraints (`bool`, `bytes`, `int`, `Decimal`, `str`, `int | float`, `date`, `datetime`, `UUID`) of type variable `ExactMatchT`
- koda_validate/generic.py:290:16: error[invalid-argument-type] Argument to bound method `startswith` is incorrect: Expected `str`, found `StrOrBytes@StartsWith`
- koda_validate/generic.py:290:31: error[invalid-argument-type] Argument to bound method `startswith` is incorrect: Expected `str | tuple[str, ...]`, found `StrOrBytes@StartsWith`
- koda_validate/generic.py:298:16: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `StrOrBytes@EndsWith`
- koda_validate/generic.py:298:29: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str | tuple[str, ...]`, found `StrOrBytes@EndsWith`
- koda_validate/generic.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@Strip`, found `Unknown | bytes`
+ koda_validate/generic.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@Strip`, found `str | bytes`
- koda_validate/generic.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@UpperCase`, found `Unknown | bytes`
+ koda_validate/generic.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@UpperCase`, found `str | bytes`
- koda_validate/generic.py:336:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@LowerCase`, found `Unknown | bytes`
+ koda_validate/generic.py:336:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@LowerCase`, found `str | bytes`
- Found 400 diagnostics
+ Found 395 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/regex.py:133:16: error[no-matching-overload] No overload of function `compile` matches arguments
+ bson/regex.py:133:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[_T@Regex]`, found `Pattern[str]`

pyodide (https://github.com/pyodide/pyodide)
- src/tests/test_static_typing.py:56:20: error[unsupported-operator] Operator `*` is unsupported between objects of type `T@f` and `Literal[2]`
+ src/tests/test_static_typing.py:56:20: error[invalid-return-type] Return type does not match returned value: expected `T@f`, found `int`

cibuildwheel (https://github.com/pypa/cibuildwheel)
- cibuildwheel/logger.py:409:30: error[invalid-argument-type] Argument to function `file_is_a_tty` is incorrect: Argument type `AnyStr@file_supports_color` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ cibuildwheel/logger.py:409:30: error[invalid-argument-type] Argument to function `file_is_a_tty` is incorrect: Expected `IO[str]`, found `IO[AnyStr@file_supports_color]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cargo/builder.py:33:72: error[invalid-argument-type] Argument is incorrect: Argument type `TV_TokenTypes@_token` does not satisfy constraints (`int`, `str`, `bool`) of type variable `TV_TokenTypes`
+ mesonbuild/cargo/builder.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Token[TV_TokenTypes@_token]`, found `Token[int]`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/utilities/templating.py:92:47: error[invalid-argument-type] Argument to function `find_placeholders` is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:94:47: error[invalid-argument-type] Argument to function `find_placeholders` is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:166:20: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `str`
- src/prefect/utilities/templating.py:199:36: error[invalid-assignment] Object of type `str` is not assignable to `T@apply_values`
+ src/prefect/utilities/templating.py:199:36: error[invalid-assignment] Object of type `str | Unknown` is not assignable to `T@apply_values`
+ src/prefect/utilities/templating.py:199:36: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T@apply_values & ~int & ~float`
- src/prefect/utilities/templating.py:201:32: error[invalid-assignment] Object of type `str` is not assignable to `T@apply_values`
+ src/prefect/utilities/templating.py:201:32: error[invalid-assignment] Object of type `str | Unknown` is not assignable to `T@apply_values`
+ src/prefect/utilities/templating.py:201:32: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T@apply_values & ~int & ~float`
- src/prefect/utilities/templating.py:203:20: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `str | T@apply_values`
- src/prefect/utilities/templating.py:207:29: error[no-matching-overload] No overload of function `apply_values` matches arguments
- src/prefect/utilities/templating.py:214:17: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `Unknown & ~<class 'NotSet'>` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:216:17: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `object` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:222:29: error[no-matching-overload] No overload of function `apply_values` matches arguments
- src/prefect/utilities/templating.py:336:20: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `str`
- src/prefect/utilities/templating.py:412:20: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `str`
- src/prefect/utilities/templating.py:432:36: error[invalid-assignment] Object of type `str` is not assignable to `T@resolve_variables`
+ src/prefect/utilities/templating.py:432:36: error[invalid-assignment] Object of type `str | Unknown` is not assignable to `T@resolve_variables`
+ src/prefect/utilities/templating.py:432:36: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T@resolve_variables`
- src/prefect/utilities/templating.py:434:36: error[invalid-assignment] Object of type `str` is not assignable to `T@resolve_variables`
+ src/prefect/utilities/templating.py:434:36: error[invalid-assignment] Object of type `str | Unknown` is not assignable to `T@resolve_variables`
- src/prefect/utilities/templating.py:435:20: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `str | T@resolve_variables`
+ src/prefect/utilities/templating.py:434:36: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T@resolve_variables`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[Unknown, Unknown]`
- src/prefect/utilities/templating.py:438:42: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `object`
- src/prefect/utilities/templating.py:442:41: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `object`
- Found 5480 diagnostics
+ Found 5471 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Unknown` and `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Unknown, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]`
+ xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Hashable` and `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Hashable, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]`
- xarray/computation/rolling.py:1096:9: error[invalid-assignment] Object of type `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Unknown, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]` is not assignable to attribute `coord_func` of type `Mapping[Hashable, str | ((...) -> Unknown)]`
+ xarray/computation/rolling.py:1096:9: error[invalid-assignment] Object of type `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Hashable, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]` is not assignable to attribute `coord_func` of type `Mapping[Hashable, str | ((...) -> Unknown)]`
- xarray/computation/rolling.py:1192:19: error[invalid-argument-type] Argument to bound method `_to_temp_dataset` is incorrect: Expected `DataArray`, found `T_Xarray@Coarsen`
- xarray/computation/rolling.py:1216:20: error[invalid-argument-type] Argument to bound method `_from_temp_dataset` is incorrect: Argument type `T_Xarray@Coarsen` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/_typed_ops.py:1447:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__add__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1450:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__sub__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1453:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__mul__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1456:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__pow__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1459:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__truediv__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1462:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__floordiv__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1465:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__mod__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1468:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__and__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1471:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__xor__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1474:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__or__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1477:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__lshift__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1480:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rshift__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1483:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__lt__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1486:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__le__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1489:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__gt__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1492:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__ge__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
+ xarray/core/_typed_ops.py:1447:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__add__`, found `DataArray`
+ xarray/core/_typed_ops.py:1450:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__sub__`, found `DataArray`
+ xarray/core/_typed_ops.py:1453:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__mul__`, found `DataArray`
+ xarray/core/_typed_ops.py:1456:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__pow__`, found `DataArray`
+ xarray/core/_typed_ops.py:1459:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__truediv__`, found `DataArray`
+ xarray/core/_typed_ops.py:1462:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__floordiv__`, found `DataArray`
+ xarray/core/_typed_ops.py:1465:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__mod__`, found `DataArray`
+ xarray/core/_typed_ops.py:1468:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__and__`, found `DataArray`
+ xarray/core/_typed_ops.py:1471:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__xor__`, found `DataArray`
+ xarray/core/_typed_ops.py:1474:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__or__`, found `DataArray`
+ xarray/core/_typed_ops.py:1477:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__lshift__`, found `DataArray`
+ xarray/core/_typed_ops.py:1480:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rshift__`, found `DataArray`
+ xarray/core/_typed_ops.py:1483:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__lt__`, found `DataArray`
+ xarray/core/_typed_ops.py:1486:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__le__`, found `DataArray`
+ xarray/core/_typed_ops.py:1489:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__gt__`, found `DataArray`
+ xarray/core/_typed_ops.py:1492:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__ge__`, found `DataArray`
- xarray/core/_typed_ops.py:1495:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__eq__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
+ xarray/core/_typed_ops.py:1495:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__eq__`, found `DataArray`
- xarray/core/_typed_ops.py:1498:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__ne__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
+ xarray/core/_typed_ops.py:1498:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__ne__`, found `DataArray`
- xarray/core/_typed_ops.py:1505:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__radd__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1508:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rsub__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1511:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rmul__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1514:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rpow__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1517:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rtruediv__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1520:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rfloordiv__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1523:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rmod__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1526:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rand__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1529:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__rxor__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/_typed_ops.py:1532:32: error[invalid-argument-type] Argument to bound method `_binary_op` is incorrect: Argument type `T_Xarray@__ror__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
+ xarray/core/_typed_ops.py:1505:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__radd__`, found `DataArray`
+ xarray/core/_typed_ops.py:1508:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rsub__`, found `DataArray`
+ xarray/core/_typed_ops.py:1511:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rmul__`, found `DataArray`
+ xarray/core/_typed_ops.py:1514:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rpow__`, found `DataArray`
+ xarray/core/_typed_ops.py:1517:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rtruediv__`, found `DataArray`
+ xarray/core/_typed_ops.py:1520:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rfloordiv__`, found `DataArray`
+ xarray/core/_typed_ops.py:1523:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rmod__`, found `DataArray`
+ xarray/core/_typed_ops.py:1526:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rand__`, found `DataArray`
+ xarray/core/_typed_ops.py:1529:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__rxor__`, found `DataArray`
+ xarray/core/_typed_ops.py:1532:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__ror__`, found `DataArray`
- xarray/core/coordinates.py:1196:69: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Unknown` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/coordinates.py:1196:69: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Hashable` on object of type `T_Xarray@assert_coordinate_consistent`
- xarray/core/coordinates.py:1199:51: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Unknown` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/coordinates.py:1199:51: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Hashable` on object of type `T_Xarray@assert_coordinate_consistent`
- xarray/core/dataarray.py:2556:13: error[invalid-argument-type] Argument to bound method `interp_like` is incorrect: Argument type `T_Xarray@interp_like` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/dataarray.py:4861:25: error[invalid-argument-type] Argument to bound method `dot` is incorrect: Argument type `T_Xarray@__matmul__` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
+ xarray/core/dataarray.py:4861:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@__matmul__`, found `DataArray`
+ xarray/core/dtypes.py:94:32: warning[possibly-missing-attribute] Attribute `itemsize` may be missing on object of type `T_dtype@maybe_promote`
- xarray/core/groupby.py:686:25: error[invalid-argument-type] Argument to bound method `transpose` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:710:27: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:768:22: error[invalid-argument-type] Argument to bound method `_to_temp_dataset` is incorrect: Expected `DataArray`, found `T_Xarray@GroupBy`
- xarray/core/groupby.py:774:20: error[invalid-argument-type] Argument to bound method `_shuffle` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:779:20: error[invalid-argument-type] Argument to bound method `_from_temp_dataset` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:841:16: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:869:23: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:961:24: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Unknown` on object of type `T_Xarray@GroupBy`
+ xarray/core/groupby.py:961:24: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Hashable` on object of type `T_Xarray@GroupBy`
- xarray/core/groupby.py:962:35: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Unknown` on object of type `T_Xarray@GroupBy`
+ xarray/core/groupby.py:962:35: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Hashable` on object of type `T_Xarray@GroupBy`
+ xarray/core/groupby.py:962:35: error[invalid-argument-type] Argument to bound method `reset_coords` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
+ xarray/core/groupby.py:962:35: error[invalid-argument-type] Argument to bound method `broadcast_like` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
+ xarray/core/groupby.py:962:35: error[no-matching-overload] No overload of bound method `reset_coords` matches arguments
- xarray/core/groupby.py:1138:13: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/indexing.py:193:48: error[invalid-argument-type] Argument to function `group_indexers_by_index` is incorrect: Argument type `T_Xarray@map_index_queries` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- xarray/core/parallel.py:139:12: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@infer_template`, found `Dataset | DataArray`
+ xarray/core/parallel.py:139:12: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@infer_template`, found `Dataset | T_Xarray@infer_template`
- xarray/core/resample.py:107:16: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:129:23: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:151:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:178:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:206:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:245:16: error[invalid-argument-type] Argument to bound method `interp` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/tests/test_indexing.py:151:50: error[invalid-argument-type] Argument to function `map_index_queries` is incorrect: Argument type `T_Xarray@test_indexer` does not satisfy constraints (`DataArray`, `Dataset`) of type variable `T_Xarray`
- Found 1757 diagnostics
+ Found 1742 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/util.py:152:20: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@change_root`, found `str`
+ setuptools/_distutils/util.py:154:20: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@change_root`, found `str`
- setuptools/_distutils/util.py:151:30: error[invalid-argument-type] Argument to function `isabs` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `AnyStr@change_root | PathLike[AnyStr@change_root]`
- setuptools/_distutils/util.py:152:20: error[no-matching-overload] No overload of function `join` matches arguments
- setuptools/_distutils/util.py:154:20: error[no-matching-overload] No overload of function `join` matches arguments
+ setuptools/_distutils/util.py:160:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@change_root`, found `str`
- setuptools/_distutils/util.py:157:25: error[no-matching-overload] No overload of function `splitdrive` matches arguments
- setuptools/_distutils/util.py:160:16: error[no-matching-overload] No overload of function `join` matches arguments
+ setuptools/glob.py:34:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[AnyStr@glob]`, found `Iterator[str]`
+ setuptools/glob.py:52:12: error[invalid-return-type] Return type does not match returned value: expected `Iterator[AnyStr@iglob]`, found `Iterator[str]`
- setuptools/glob.py:34:23: error[invalid-argument-type] Argument to function `iglob` is incorrect: Argument type `AnyStr@glob` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- setuptools/glob.py:48:17: error[invalid-argument-type] Argument to function `_iglob` is incorrect: Argument type `AnyStr@iglob` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- setuptools/glob.py:56:25: error[no-matching-overload] No overload of function `split` matches arguments
- setuptools/glob.py:82:21: error[no-matching-overload] No overload of function `glob2` matches arguments
- setuptools/glob.py:82:21: error[no-matching-overload] No overload of function `glob1` matches arguments
- setuptools/glob.py:83:19: error[no-matching-overload] No overload of function `join` matches arguments
- Found 1271 diagnostics
+ Found 1265 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/heap/ptmalloc.py:1192:74: error[invalid-argument-type] Argument to bound method `keys` is incorrect: Expected `Type`, found `TheType@GlibcMemoryAllocator`
- pwndbg/aglib/heap/ptmalloc.py:1201:16: error[invalid-argument-type] Argument to bound method `keys` is incorrect: Expected `Type`, found `TheType@GlibcMemoryAllocator`
- Found 2819 diagnostics
+ Found 2817 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 39 diagnostics
+ Found 41 diagnostics

altair (https://github.com/vega/altair)
+ altair/utils/schemapi.py:1048:16: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Argument type `_CopyImpl@_shallow_copy` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ altair/utils/schemapi.py:1048:16: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Argument type `_CopyImpl@_shallow_copy` does not satisfy upper bound `list[_T@list]` of type variable `Self`
+ altair/utils/schemapi.py:1048:25: error[unknown-argument] Argument `deep` does not match any known parameter of bound method `copy`
+ altair/utils/schemapi.py:1048:25: error[unknown-argument] Argument `deep` does not match any known parameter of bound method `copy`
+ altair/utils/schemapi.py:1065:38: warning[possibly-missing-attribute] Attribute `_args` may be missing on object of type `_CopyImpl@_deep_copy | (Any & SchemaBase)`
+ altair/utils/schemapi.py:1066:68: warning[possibly-missing-attribute] Attribute `_kwds` may be missing on object of type `_CopyImpl@_deep_copy | (Any & SchemaBase)`
- Found 1107 diagnostics
+ Found 1113 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/client.py:1400:22: error[invalid-argument-type] Argument to bound method `__aenter__` is incorrect: Expected `ClientResponse`, found `_RetType@_BaseRequestContextManager`
- aiohttp/client.py:1408:15: error[invalid-argument-type] Argument to bound method `__aexit__` is incorrect: Expected `ClientResponse`, found `_RetType@_BaseRequestContextManager`
+ aiohttp/http_parser.py:397:40: warning[possibly-missing-attribute] Attribute `method` may be missing on object of type `_MsgT@HttpParser`
- Found 178 diagnostics
+ Found 177 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/algorithms.py:214:16: error[invalid-return-type] Return type does not match returned value: expected `ArrayLikeT@_reconstruct_data`, found `ExtensionArray`
+ pandas/core/dtypes/cast.py:408:16: error[invalid-return-type] Return type does not match returned value: expected `NumpyIndexT@maybe_upcast_numeric_to_64bit`, found `ndarray[tuple[Any, ...], Unknown] | Unknown`
+ pandas/core/dtypes/cast.py:410:16: error[invalid-return-type] Return type does not match returned value: expected `NumpyIndexT@maybe_upcast_numeric_to_64bit`, found `ndarray[tuple[Any, ...], Unknown] | Unknown`
+ pandas/core/dtypes/cast.py:412:16: error[invalid-return-type] Return type does not match returned value: expected `NumpyIndexT@maybe_upcast_numeric_to_64bit`, found `ndarray[tuple[Any, ...], Unknown] | Unknown`
- Found 3762 diagnostics
+ Found 3764 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/devicetools.py:2541:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[t@_make_tuple | None, t@_make_tuple | None]`, found `tuple[None | str | int, None | str | int]`
+ hydpy/core/devicetools.py:2541:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[t@_make_tuple | None, t@_make_tuple | None]`, found `tuple[None | t@_make_tuple | int, None | t@_make_tuple | int]`
- hydpy/core/devicetools.py:2724:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[t@_make_tuple | None, t@_make_tuple | None]`, found `tuple[None | str | int, None | str | int]`
+ hydpy/core/devicetools.py:2724:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[t@_make_tuple | None, t@_make_tuple | None]`, found `tuple[None | t@_make_tuple | int, None | t@_make_tuple | int]`
- hydpy/exe/xmltools.py:1869:34: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `_TypeGetOrChangeItem@_get_items_of_certain_item_types`, found `GetItem | (_TypeGetOrChangeItem@_get_items_of_certain_item_types & ChangeItem)`
- hydpy/exe/xmltools.py:1874:38: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `_TypeGetOrChangeItem@_get_items_of_certain_item_types`, found `GetItem | (_TypeGetOrChangeItem@_get_items_of_certain_item_types & ChangeItem)`
- Found 698 diagnostics
+ Found 696 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Index[Any], TVDtype@Index]`
+ static_frame/core/node_fill_value.py:179:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/node_fill_value.py:187:13: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `TVContainer_co@InterfaceFillValue`
+ static_frame/core/node_fill_value.py:198:49: warning[possibly-missing-attribute] Attribute `columns` may be missing on object of type `TVContainer_co@InterfaceFillValue`
+ static_frame/core/node_fill_value.py:217:53: warning[possibly-missing-attribute] Attribute `columns` may be missing on object of type `TVContainer_co@InterfaceFillValue`
- static_frame/core/node_fill_value.py:231:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/node_selector.py:452:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[TVContainer_co@InterfaceSelectDuo, Any]`, found `InterGetItemILocReduces[Index[Any], Any]`
+ static_frame/core/node_selector.py:456:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Hashable, /) -> Index[Any]`, found `Unknown | TLocSelectorFunc@__init__`
+ static_frame/core/node_selector.py:487:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILoc[TVContainer_co@InterfaceSelectTrio]`, found `InterGetItemILoc[Index[Any]]`
+ static_frame/core/node_selector.py:492:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Hashable, /) -> Index[Any]`, found `Unknown | TLocSelectorFunc@__init__`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Index[Any], Any]`
+ static_frame/core/node_selector.py:531:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocRedu

... (truncated 20 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Converted to draft by @AlexWaygood on 2025-12-07 18:26_

---

_Comment by @AlexWaygood on 2025-12-11 21:44_

https://github.com/astral-sh/ruff/pull/21840 does it better

---

_Closed by @AlexWaygood on 2025-12-11 21:44_

---

_Branch deleted on 2025-12-11 21:44_

---
