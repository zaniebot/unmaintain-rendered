```yaml
number: 22614
title: "[ty] Use distributed versions of AND and OR on constraint sets"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - performance
  - ty
assignees: []
base: main
head: dcreager/distributed-ops
created_at: 2026-01-16T10:01:46Z
updated_at: 2026-01-16T12:37:40Z
url: https://github.com/astral-sh/ruff/pull/22614
synced_at: 2026-01-16T12:56:31Z
```

# [ty] Use distributed versions of AND and OR on constraint sets

---

_@dcreager_

There are some pathological examples where we create a constraint set which is the AND or OR of several smaller constraint sets. For example, when calling a function with many overloads, where the argument is a typevar, we create an OR of the typevar specializing to a type compatible with the respective parameter of each overload.

Most functions have a small number of overloads. But there are some examples of methods with 15-20 overloads (pydantic, numpy, our own auto-generated `__getitem__` for large tuple literals). For those cases, it is helpful to be more clever about how we construct the final result.

Before, we would just step through the `Iterator` of elements and accumulate them into a result constraint set. That results in an `O(n)` number of calls to the underlying `and` or `or` operator — each of which might have to construct a large temporary BDD tree!

AND and OR are both associative, so we can do better! We now collect the smaller constraint sets into a vector, and then divide the resulting slice in half, calling ourselves recursively and then combining the results. This results in an `O(log n)` number of calls. But more importantly, it means that more of the calls operate on smaller trees, so the overall reduction is work is even greater.

---

_Label `internal` added by @dcreager on 2026-01-16 10:01_

---

_Label `performance` added by @dcreager on 2026-01-16 10:01_

---

_Label `ty` added by @dcreager on 2026-01-16 10:01_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["spam"]`
+ tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `float | int`, found `Literal["spam"]`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/kernel/__init__.py:218:20: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Literal[1]`
+ pwndbg/aglib/kernel/__init__.py:218:20: error[unsupported-operator] Operator `+` is not supported between objects of type `None | Unknown` and `Literal[1]`
- pwndbg/aglib/kernel/__init__.py:224:22: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `int`
+ pwndbg/aglib/kernel/__init__.py:224:22: error[unsupported-operator] Operator `+` is not supported between objects of type `None | Unknown` and `int`

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

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 5 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- Found 1823 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14509 diagnostics
+ Found 14508 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~73MB
+     memo metadata = ~76MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~725MB
-     struct metadata = ~49MB
+     struct metadata = ~52MB
-     memo metadata = ~167MB
+     memo metadata = ~176MB


```

</details>




---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-16 10:08_

While we're here, I'm updating the BDD variable ordering to be less clever. For the pathological example from #21902 this has a huge benefit.

---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:4028 on 2026-01-16 10:08_

and also update the tree display to make it more obvious where we're sharing tree structure

---

_Marked ready for review by @dcreager on 2026-01-16 10:09_

---

_Review requested from @carljm by @dcreager on 2026-01-16 10:09_

---

_Review requested from @AlexWaygood by @dcreager on 2026-01-16 10:09_

---

_Review requested from @sharkdp by @dcreager on 2026-01-16 10:09_

---

_Comment by @codspeed-hq[bot] on 2026-01-16 10:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 82.67%**

<sub>Comparing <code>dcreager/distributed-ops</code> (13ad151) with <code>main</code> (28bfcf8)</sub>



### Summary

`❌ 9` regressed benchmarks  
`✅ 14` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 90 s | 519.4 s | -82.67% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 8 s | 10.2 s | -21.33% |
| ❌ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.3 s | 40.3 s | -74.37% |
| ❌ | WallTime | [`` tanjun ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Atanjun&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.5 s | 2.7 s | -4.11% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 64.5 s | 104.7 s | -38.41% |
| ❌ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.7 s | 4.9 s | -5.43% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.9 s | 34.2 s | -36.08% |
| ❌ | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 240.5 ms | 345.5 ms | -30.39% |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1.3 s | -4.95% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2026-01-16 10:43_

From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

---

_@MichaReiser reviewed on 2026-01-16 10:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 10:45_

Could this be a `FxIndexSet` or why is it important that `seen` has `Eq` and `PartialEq`?

---

_@MichaReiser reviewed on 2026-01-16 10:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 10:47_

Could it be that the collect calls are expensive? Given that `distributed_or` and `distributed_and` are very small methods, would it make sense to pass the `f` through and apply the mapping in `distributed_or`/and?

Another alternative is to use a `SmallVec` instead. But I wonder if part of the perf regression simply comes from writing all the constraints to a vec


---

_Comment by @dcreager on 2026-01-16 12:37_

> From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

Me too! Maybe the collecting is the culprit, as you suggest? Or maybe because we're not short circuiting anymore? I have an idea that might help with both.

---
