```yaml
number: 22071
title: "[ty] Skip redundancy checks for sub-union-elements"
type: pull_request
state: open
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: micha/union-builder-sub-unions
created_at: 2025-12-19T07:54:14Z
updated_at: 2025-12-27T14:04:04Z
url: https://github.com/astral-sh/ruff/pull/22071
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Skip redundancy checks for sub-union-elements

---

_@MichaReiser_

## Summary

When adding a sub-union to a union, don't compare its elements to each other, as they are already simplified. 

While the `is_redundant_with` calls are mostly cached, reducing the impact of this change, other calls like `is_subtype_of` aren't cached and can be skipped with this change.

It turned out that this change is slightly trickier than I thought because we build unions in different modes, and the optimization is only correct if both unions use the same mode.

---

_Label `ty` added by @MichaReiser on 2025-12-19 07:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 07:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 07:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mkosi (https://github.com/systemd/mkosi)
+ mkosi/qemu.py:527:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
- Found 97 diagnostics
+ Found 98 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/embed/notebook.py:86:37: error[invalid-argument-type] Argument is incorrect: Expected `Theme | type[FromCurdoc] | None`, found `Theme | FromCurdoc | None`
+ src/bokeh/embed/standalone.py:265:28: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Model]`, found `Model | Sequence[Model] | dict[str, Model]`
- Found 893 diagnostics
+ Found 895 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | Top[Yarn[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1843 diagnostics
+ Found 1844 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2799 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-19 08:12_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-sub-unions?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22071 will **improve performance by 32.09%**

<sub>Comparing <code>micha/union-builder-sub-unions</code> (54347a5) with <code>main</code> (7ac1874)</sub>



### Summary

`⚡ 3` improvements  
`✅ 19` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-sub-unions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.2 s | 4.6 s | +12.73% |
| ⚡ | WallTime | [`` multithreaded ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-sub-unions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.5 s | 1.1 s | +32.09% |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-sub-unions?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.4 s | 9.9 s | +4.98% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-sub-unions?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2025-12-19 12:22_

The speed-ups in the ty-benchmark are between 0-6%

---

_Marked ready for review by @MichaReiser on 2025-12-19 12:24_

---

_Review requested from @carljm by @MichaReiser on 2025-12-19 12:24_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-19 12:24_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-19 12:24_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-19 12:24_

---

_Comment by @MichaReiser on 2025-12-19 12:25_

Not sure how to tell whether there are any new mypy primer hits or if it's just the usual non-deterministic cases

---

_Comment by @AlexWaygood on 2025-12-19 12:27_

> Not sure how to tell whether there are any new mypy primer hits or if it's just the usual non-deterministic cases

It might be worth seeing if you can reproduce the prefect errors locally. I don't recall seeing those ones before.

---

_Label `performance` added by @AlexWaygood on 2025-12-19 12:27_

---

_Comment by @MichaReiser on 2025-12-19 13:09_

Yeah, they look suspicious. 

---

_Converted to draft by @MichaReiser on 2025-12-22 14:06_

---
