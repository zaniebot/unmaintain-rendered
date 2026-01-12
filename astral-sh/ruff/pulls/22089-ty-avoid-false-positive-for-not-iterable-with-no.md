```yaml
number: 22089
title: "[ty] Avoid false positive for `not-iterable` with no-positive intersection types"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/neg
created_at: 2025-12-19T16:29:04Z
updated_at: 2025-12-20T02:10:02Z
url: https://github.com/astral-sh/ruff/pull/22089
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Avoid false positive for `not-iterable` with no-positive intersection types

---

_@charliermarsh_

## Summary

When checking if `list[~str]` is iterable, we end up looking for a positive element to satisfy assignability to `Iterable[~str]`. But when there are no positive elements, we return `false`, rather than falling back to `object`.

Closes https://github.com/astral-sh/ty/issues/1880.


---

_Renamed from "Avoid false positive for `not-iterable` with no-positive intersection types" to "[ty] Avoid false positive for `not-iterable` with no-positive intersection types" by @charliermarsh on 2025-12-19 16:29_

---

_Label `ty` added by @charliermarsh on 2025-12-19 16:29_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/lending.py:811:22: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown | str, Unknown | str | ~AlwaysTruthy | dict[Unknown | str, Unknown] | int]`
- Found 1153 diagnostics
+ Found 1152 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/permission.py:163:35: error[not-iterable] Object of type `list[~AlwaysFalsy | Unknown]` is not iterable
- Found 393 diagnostics
+ Found 392 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

altair (https://github.com/vega/altair)
+ altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SchemaBase | Mapping[str, Any] | UndefinedType`, found `object`
- altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | dict[Unknown | str, Unknown] | dict[~Literal["values"] | Unknown, object]`
+ altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
- Found 1108 diagnostics
+ Found 1109 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5086 diagnostics
+ Found 5085 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/bluetooth/passive_update_coordinator.py:78:20: error[not-iterable] Object of type `GeneratorType[~None, None, None]` is not iterable
- homeassistant/helpers/update_coordinator.py:218:20: error[not-iterable] Object of type `GeneratorType[~None, None, None]` is not iterable
- Found 14425 diagnostics
+ Found 14423 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-19 16:35_

---

_Comment by @codspeed-hq[bot] on 2025-12-19 16:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22089 will **degrade performance by 29.26%**

<sub>Comparing <code>charlie/neg</code> (905a38d) with <code>main</code> (df1552b)</sub>



### Summary

`❌ 6` regressions  
`✅ 16` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 19.9 s | 21.7 s | -8.04% |
| ❌ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 106.8 s | 151 s | -29.26% |
| ❌ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 63.8 s | 70.8 s | -9.97% |
| ❌ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.9 s | 8.6 s | -7.48% |
| ❌ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.3 ms | 6.8 ms | -6.47% |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1.3 s | -5.41% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @charliermarsh on 2025-12-19 17:15_

Ok, wasn't expecting that!

---

_Comment by @AlexWaygood on 2025-12-19 18:06_

This does look correct, but we obviously can't merge this if it causes the benchmarks to start timing out 🙃

I think we probably need to do similar for the `when_any` calls in the `Type::Intersection` branch above this one, too

---

_Comment by @charliermarsh on 2025-12-19 18:34_

(Looking into it...)

---

_Converted to draft by @charliermarsh on 2025-12-19 18:34_

---

_@carljm reviewed on 2025-12-19 18:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 18:56_

I expect this fallback to object is the source of the regression, and I'm surprised that we would need it here. If we are going to treat an intersection type with no positive elements as equivalent to `object`, then the only thing it can be a subtype of is `object` (and the only things it can be assignable to are `Any/Unknown` and `object`), and those cases should already be handled above. What was failing without this change? That is, in what form does the `list[~str]` iteration case reach here?

---

_@AlexWaygood reviewed on 2025-12-19 18:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 18:59_

`~str` can also be a subtype of a union, like `~str | int` -- but I guess that's also handled by a higher-up branch.

---

_@AlexWaygood reviewed on 2025-12-19 19:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 19:02_

oh, and `~str & ~int` is a subtype of both `~str` and `~str & ~int`, right?

---

_@carljm reviewed on 2025-12-19 19:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 19:06_

Yes, seems like we could add consideration of cases like that to get more precise results here, but that seems like a separate issue. We don't consider negative types in the intersection either before or after the current change in this PR. The only change here is to explicitly test `object`, which seems to me like it shouldn't be necessary (but clearly I'm missing something, if it makes the test in this PR pass.)

---

_@AlexWaygood reviewed on 2025-12-19 19:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 19:08_

I was just answering the immediate question, not trying to say that these cases were relevant. I think they're handled by the branch immediately above this one, anyway

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-20 02:10_

I am of course not an expert here but my understanding is that when we `try_iterate` over `list[~str]`, we eventually check `list.__iter__` which is `def __iter__(self) -> Iterator[_T]`, and then need to infer specialization to determine that `_T` is `~str`.

We then end up in `has_relation_to_impl` and in this big match on `(self, target)`, `self` is an intersection (`~str`) and `target` is a type var (`_T`), which don't match any of the above cases... So we land in this case, and at present, `intersection.positive(db).iter()` is empty, so we return false.

---

_@charliermarsh reviewed on 2025-12-20 02:10_

---
