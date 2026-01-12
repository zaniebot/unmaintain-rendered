```yaml
number: 21683
title: "[ty] increase the limit on the number of elements in a non-recursively defined literal union"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: faster-literal-type-convergence
created_at: 2025-11-28T18:41:02Z
updated_at: 2025-12-05T04:31:02Z
url: https://github.com/astral-sh/ruff/pull/21683
synced_at: 2026-01-12T15:57:30Z
```

# [ty] increase the limit on the number of elements in a non-recursively defined literal union

---

_@mtshiba_

## Summary

Closes https://github.com/astral-sh/ty/issues/957

As explained in https://github.com/astral-sh/ty/issues/957, literal union types for recursively defined values ​​can be widened early to speed up the convergence of fixed-point iterations.
This PR achieves this by embedding a marker in `UnionType` that distinguishes whether a value is recursively defined.

This also allows us to identify values ​​that are not recursively defined, so I've increased the limit on the number of elements in a literal union type for such values.

Edit: while this PR doesn't provide the significant performance improvement initially hoped for, it does have the benefit of allowing the number of elements in a literal union to be raised above the salsa limit, and indeed mypy_primer results revealed that a literal union of 220 elements was actually being used.

## Test Plan

`call/union.md` has been updated

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 18:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ty` added by @AlexWaygood on 2025-11-28 18:43_

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 18:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pandas/model.py:210:13: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | str | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown | None]`
+ pandera/api/pandas/model.py:210:13: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown | None]`
+ pandera/engines/numpy_engine.py:52:41: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `str`
- pandera/engines/pandas_engine.py:1962:26: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | str | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
+ pandera/engines/pandas_engine.py:1962:26: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
- pandera/engines/pandas_engine.py:1988:26: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | str | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
+ pandera/engines/pandas_engine.py:1988:26: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
- pandera/engines/pyarrow_engine.py:440:22: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | str | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
+ pandera/engines/pyarrow_engine.py:440:22: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
- pandera/engines/pyarrow_engine.py:465:22: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | str | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
+ pandera/engines/pyarrow_engine.py:465:22: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `ArrowDtype | None`
- tests/pandas/test_pydantic.py:308:53: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | str | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown] | dict[str | Unknown, Any | None | type[Any]]`
+ tests/pandas/test_pydantic.py:308:53: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 220 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown] | dict[str | Unknown, Any | None | type[Any]]`
- Found 1635 diagnostics
+ Found 1636 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/rpc/rpc.py:1451:88: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `Sequence[str | bytes | date | ... omitted 10 union elements] | NAType | date | ... omitted 13 union elements`, found `dict[Unknown | NaTType, Unknown | None]`
+ freqtrade/rpc/rpc.py:1448:28: error[no-matching-overload] No overload of bound method `select_dtypes` matches arguments

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/code.py:1207:9: error[invalid-argument-type] Argument to function `run_solr_query` is incorrect: Expected `Literal["UNLABELLED", "BOOK_SEARCH", "BOOK_SEARCH_API", "BOOK_SEARCH_FACETS", "BOOK_CAROUSEL", ... omitted 11 literals]`, found `str`
- Found 1122 diagnostics
+ Found 1123 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/indexes/test_index_float.py:113:13: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `Index[Any]`
- tests/series/test_series_float.py:52:13: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Series[Any]`
+ tests/series/test_series_float.py:52:13: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Series[list[Unknown | float]]`
+ tests/series/test_series_float.py:108:13: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/series/test_series_float.py:108:25: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ tests/series/test_series_float.py:110:15: error[no-matching-overload] No overload of bound method `astype` matches arguments
- Found 5859 diagnostics
+ Found 5863 diagnostics

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

_Comment by @codspeed-hq[bot] on 2025-11-28 18:59_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afaster-literal-type-convergence?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21683 will **degrade performances by 9.91%**

<sub>Comparing <code>mtshiba:faster-literal-type-convergence</code> (a515490) with <code>main</code> (ecab623)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afaster-literal-type-convergence?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` ty_micro[many_string_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afaster-literal-type-convergence?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_string_assignments%3A%3Aty_micro%5Bmany_string_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 76.4 ms | 84.8 ms | -9.91% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afaster-literal-type-convergence?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @mtshiba on 2025-11-29 02:55_

Hmm, for example, when inspecting the following code, I see a clear performance improvement locally, but the change on codspeed seems less clear:

```python
class Counter:
    def __init__(self: "Counter"):
        self.count = 0

    def increment(self: "Counter"):
        self.count = self.count + 1

reveal_type(Counter().count)  # revealed: Unknown | int
```

main:
```sh
# release
$ hyperfine -i "ty check widen.py"
Benchmark 1: ty check widen.py
  Time (mean ± σ):     103.3 ms ±   8.6 ms    [User: 56.9 ms, System: 45.0 ms]
  Range (min … max):    96.4 ms … 136.6 ms    22 runs

# debug
$ hyperfine -i "ty check widen.py"
Benchmark 1: ty check widen.py
  Time (mean ± σ):     899.9 ms ±   6.2 ms    [User: 849.7 ms, System: 51.9 ms]
  Range (min … max):   889.4 ms … 910.3 ms    10 runs
```

#21683:
```sh
# release
$ hyperfine -i "ty check widen.py"
Benchmark 1: ty check widen.py
  Time (mean ± σ):      59.6 ms ±   9.2 ms    [User: 26.5 ms, System: 37.8 ms]
  Range (min … max):    52.5 ms …  95.7 ms    31 runs

# debug
$ hyperfine -i "ty check widen.py"
Benchmark 1: ty check widen.py
  Time (mean ± σ):     267.2 ms ±   4.1 ms    [User: 222.2 ms, System: 55.6 ms]
  Range (min … max):   261.6 ms … 276.5 ms    10 runs
```

---

_Comment by @mtshiba on 2025-11-29 02:58_

Conversely, the apparent performance degradation is due to increasing `MAX_NON_RECURSIVE_UNION_LITERALS`.
Even setting it to 512 did not result in a panic, which shows that the divergent cases are being correctly avoided.

The performance impact of changing `MAX_NON_RECURSIVE_UNION_LITERALS` is as follows:

190: https://codspeed.io/astral-sh/ruff/runs/compare/6929edd91e804897641888e6..6929fa9ed77b771deb1a9244
256: https://codspeed.io/astral-sh/ruff/runs/compare/6929edd91e804897641888e6..6929f4ded77b771deb1a9211
512: https://codspeed.io/astral-sh/ruff/runs/compare/6929edd91e804897641888e6..6929ef181e804897641888ec

---

_Comment by @MichaReiser on 2025-11-29 14:43_

> Hmm, for example, when inspecting the following code, I see a clear performance improvement locally, but the change on codspeed seems less clear:

There might just not be enough very large unions so that, in the grand picture, the improvement is lost in noise

---

_Renamed from "[ty] WIP: accelerate fixed-point convergence of recursively defined literal types" to "[ty] increase the limit on the number of elements in a non-recursively defined literal union" by @mtshiba on 2025-12-04 10:23_

---

_Comment by @mtshiba on 2025-12-04 10:49_

I think this is ready for review. Here are some comments:

The performance degradation of codspeed(many_string_assignments) is entirely expected.

There is no clear standard for what the new upper limit for literal unions should be, but I've set it to 256 for now.
mypy_primer revealed that literal unions of 220 elements are actually used, but said that of 512 elements is likely not present in practice.

---

_Marked ready for review by @mtshiba on 2025-12-04 10:49_

---

_Review requested from @carljm by @mtshiba on 2025-12-04 10:49_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-04 10:49_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-04 10:49_

---

_Review requested from @dcreager by @mtshiba on 2025-12-04 10:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-04 11:11_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/union.md`:234 on 2025-12-05 01:59_

Why do we need to add the `if flag else 512` here? It wasn't in the previous version of the test, and it seems the test passes without it. Did some intermediate version of the code fail without this?

---

_@carljm approved on 2025-12-05 02:01_

This is great, thank you!

---

_Merged by @carljm on 2025-12-05 02:01_

---

_Closed by @carljm on 2025-12-05 02:01_

---

_Branch deleted on 2025-12-05 02:20_

---

_@mtshiba reviewed on 2025-12-05 04:31_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/call/union.md`:234 on 2025-12-05 04:31_

Oops, it seems this was written based on the old limit.
Opened #21804.

---
