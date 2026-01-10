```yaml
number: 22080
title: "[ty] Only print dashed line for failing tests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/fix-mdtest-output
created_at: 2025-12-19T12:56:55Z
updated_at: 2025-12-19T13:20:06Z
url: https://github.com/astral-sh/ruff/pull/22080
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Only print dashed line for failing tests

---

_Pull request opened by @MichaReiser on 2025-12-19 12:56_

## Summary

Don't print unnecessary dashed lines between passing tests. But keep the line between failing tests, as it improves readability if they're from the same mdtest file

## Test Plan

```
test mdtest::assignment/augmented.md                       ... ok
test mdtest::binary/booleans.md                            ... ok
test mdtest::binary/unions.md                              ... ok
test mdtest::binary/tuples.md                              ... ok

short_circuit.md - Short-Circuit Evalua… - First expression is … (80ce6787b0f38ead)

  crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md:25 unmatched assertion: revealed: Literal[2]
  crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md:25 unexpected error: 21 [revealed-type] "Revealed type: `Literal[1]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='short_circuit.md - Short-Circuit Evalua… - First expression is … (80ce6787b0f38ead)'
MDTEST_TEST_FILTER='short_circuit.md - Short-Circuit Evalua… - First expression is … (80ce6787b0f38ead)' cargo test -p ty_python_semantic --test mdtest -- boolean/short_circuit.md

--------------------------------------------------

test mdtest::binary/integers.md                            ... ok

short_circuit.md - Short-Circuit Evalua… - Statically known tru… (e71ec3b0aa226439)

  crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md:39 unmatched assertion: revealed: Literal[2]
  crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md:39 unexpected error: 17 [revealed-type] "Revealed type: `Literal[1]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='short_circuit.md - Short-Circuit Evalua… - Statically known tru… (e71ec3b0aa226439)'
MDTEST_TEST_FILTER='short_circuit.md - Short-Circuit Evalua… - Statically known tru… (e71ec3b0aa226439)' cargo test -p ty_python_semantic --test mdtest -- boolean/short_circuit.md

--------------------------------------------------


thread '<unnamed>' (6663782) panicked at crates/ty_test/src/lib.rs:157:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
test mdtest::boolean/short_circuit.md                      ... FAILED
test mdtest::binary/custom.md                              ... ok
test mdtest::boundness_declaredness/public.md              ... ok
test mdtest::binary/instances.md                           ... ok
test mdtest::call/annotation.md                            ... ok
test mdtest::binary/classes.md                             ... ok
test mdtest::call/builtins.md                              ... ok

```


---

_Label `testing` added by @MichaReiser on 2025-12-19 12:56_

---

_Review requested from @carljm by @MichaReiser on 2025-12-19 12:56_

---

_Label `ty` added by @MichaReiser on 2025-12-19 12:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-19 12:56_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-19 12:56_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-19 12:56_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 12:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 12:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, generic[object]]`
- Found 1839 diagnostics
+ Found 1838 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14416 diagnostics
+ Found 14415 diagnostics

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


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-12-19 13:17_

---

_Merged by @MichaReiser on 2025-12-19 13:20_

---

_Closed by @MichaReiser on 2025-12-19 13:20_

---

_Branch deleted on 2025-12-19 13:20_

---
