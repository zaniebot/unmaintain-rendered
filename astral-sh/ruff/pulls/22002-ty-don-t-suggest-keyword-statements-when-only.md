```yaml
number: 22002
title: "[ty] Don't suggest keyword statements when only expressions are valid"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: detect-all-expr-only-cases
created_at: 2025-12-16T10:56:12Z
updated_at: 2025-12-18T17:27:57Z
url: https://github.com/astral-sh/ruff/pull/22002
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Don't suggest keyword statements when only expressions are valid

---

_@RasmusNygren_

There are cases where the python grammar enforces expressions after certain statements. In such cases we want to suppress irrelevant keywords from the auto-complete suggestions.

E.g. `with a<CURSOR>`, suggesting `raise` here never makes sense because it is not valid by the grammar.

## Summary
This tries to detect all cases where the python grammar disallows anything but expressions and eliminates
statements keywords in those cases as we know those are invalid.

## Test Plan
New test cases and testing in the playground.



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 10:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 11:00_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/examples/jointree.py:38:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:38:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:39:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:39:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:40:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:40:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:41:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:41:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:42:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:42:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:43:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:43:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:53:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:53:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:64:22: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:64:22: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/array_api_jit.py:32:86: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown & BaseExpr`
+ python/egglog/exp/array_api_jit.py:32:86: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> BaseExpr & Unknown`
- python/egglog/exp/program_gen.py:131:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:131:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:181:46: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:181:46: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:183:39: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:183:39: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:188:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:188:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:198:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:198:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:224:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:224:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:228:46: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:228:46: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:231:66: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:231:66: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:234:46: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:234:46: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:234:67: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:234:67: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:237:47: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:237:47: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:237:91: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:237:91: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:252:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:252:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:253:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:253:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:265:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:265:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:266:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:266:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:274:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:274:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:275:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:275:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:285:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:285:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:286:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:286:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:302:56: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:302:56: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:304:49: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:304:49: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:312:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:312:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:325:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:325:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:340:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:340:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:353:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:353:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:373:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:373:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:374:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:374:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:375:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:375:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:403:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:403:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:404:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:404:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:405:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:405:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:406:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:406:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/tests/test_array_api.py:290:92: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown & BaseExpr`
+ python/tests/test_array_api.py:290:92: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> BaseExpr & Unknown`
- python/tests/test_program_gen.py:87:47: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/tests/test_program_gen.py:87:47: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- Found 5079 diagnostics
+ Found 5081 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14409 diagnostics
+ Found 14410 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @RasmusNygren on 2025-12-16 11:06_

@BurntSushi I tried to take a bigger stab at https://github.com/astral-sh/ty/issues/1774 and actually go through the grammar to find all cases where we know only expressions are valid. I think this makes sense and I believe most cases should be covered.

Down the line there will most certainly be other things to remove than just keywords, and there are most certainly situations where we do want to suppress statement keywords even when they are syntactically valid but I think this is a reasonable start.

I'll leave this PR as a draft though as I'd like to hear your thoughts

---

_Label `server` added by @AlexWaygood on 2025-12-16 12:36_

---

_Label `ty` added by @AlexWaygood on 2025-12-16 12:36_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-17 11:20_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1687 on 2025-12-17 14:31_

```suggestion
        return Some(FxHashSet::from(["lambda"]));
```

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1694 on 2025-12-17 14:32_

I think you want to use `then` with a closure here. And `FxHashSet::from([...])`.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1694 on 2025-12-17 14:32_

Same deal below.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1731 on 2025-12-17 14:35_

Can you add a test for this and attach a TODO to it to fix in the future?

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:6730 on 2025-12-17 14:37_

Do the tests here cover every case in `is_expecting_expression`? I know it's probably a pain, but I think it's probably worth doing.

---

_@BurntSushi reviewed on 2025-12-17 14:38_

Nice! I think this overall approach LGTM. And I like that `covering_node` is only called once here despite doing more analysis. :-)

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:6730 on 2025-12-17 19:00_

I left some out while drafting this and validating the approach but now there should be a test for all cases. While tedious to do indeed, it doesn't really make sense imo to not have a test for every case :D

---

_@RasmusNygren reviewed on 2025-12-17 19:00_

---

_Marked ready for review by @RasmusNygren on 2025-12-17 19:14_

---

_Review requested from @carljm by @RasmusNygren on 2025-12-17 19:14_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-17 19:14_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-17 19:14_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-17 19:14_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-17 19:14_

---

_Review requested from @BurntSushi by @RasmusNygren on 2025-12-17 19:15_

---

_Review request for @carljm removed by @carljm on 2025-12-17 22:47_

---

_@BurntSushi approved on 2025-12-18 15:32_

Beautiful. I'll merge #22037 first and then handle the conflicts for you and get this PR merged.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 15:34_

---

_Merged by @BurntSushi on 2025-12-18 17:27_

---

_Closed by @BurntSushi on 2025-12-18 17:27_

---
