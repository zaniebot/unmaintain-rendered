```yaml
number: 19785
title: "[ty] [experiment] Remove `TupleType::find_legacy_typevars`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: alex/remove-tuples
head: alex/tuple-legacy-tvars
created_at: 2025-08-06T14:03:07Z
updated_at: 2025-08-07T10:20:43Z
url: https://github.com/astral-sh/ruff/pull/19785
synced_at: 2026-01-10T17:52:17Z
```

# [ty] [experiment] Remove `TupleType::find_legacy_typevars`

---

_Pull request opened by @AlexWaygood on 2025-08-06 14:03_

## Summary

(I can't figure out a test that fails if we delete all this code; curious if primer will show me something)

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-08-06 14:03_

---

_Comment by @github-actions[bot] on 2025-08-06 14:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-06 14:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `_ArgsortCache`, found `(Unknown & ~None) | _ArgsortCache | Unknown | None`
+ static_frame/core/quilt.py:866:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/reduce.py:460:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/reduce.py:484:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:41:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:47:27: warning[possibly-unbound-attribute] Attribute `sort` on type `None | @Todo(instance attribute on class with dynamic base)` is possibly unbound
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:48:18: warning[possibly-unbound-attribute] Attribute `rename` on type `None | @Todo(instance attribute on class with dynamic base)` is possibly unbound
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:63:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:70:18: warning[possibly-unbound-attribute] Attribute `rename` on type `None | @Todo(instance attribute on class with dynamic base)` is possibly unbound
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:95:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:102:18: warning[possibly-unbound-attribute] Attribute `rename` on type `None | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- static_frame/test/unit/test_type_clinic.py:2383:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `set[Unknown]`
- static_frame/test/unit/test_type_clinic.py:2395:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `set[Unknown]`
- static_frame/test/unit/test_type_clinic.py:2395:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `set[Unknown]`
- static_frame/test/unit/test_type_clinic.py:2475:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `set[Unknown]`
- static_frame/test/unit/test_type_clinic.py:2475:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `set[Unknown]`
- Found 1771 diagnostics
+ Found 1777 diagnostics

scipy (https://github.com/scipy/scipy)
- benchmarks/benchmarks/stats.py:186:15: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `tuple[Unknown, Unknown]` on object of type `list[Unknown]`
- scipy/_lib/_array_api_docs_tables.py:215:9: error[invalid-assignment] Object of type `Unknown | dict[Unknown, Unknown]` is not assignable to `list[str] | None`
- scipy/_lib/_array_api_docs_tables.py:263:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, tuple[dict[str, str], bool]]`, found `list[Unknown]`
- scipy/_lib/tests/test_ccallback.py:100:24: error[call-non-callable] Object of type `LowLevelCallable` is not callable
- scipy/_lib/tests/test_ccallback.py:128:24: error[call-non-callable] Object of type `LowLevelCallable` is not callable
+ scipy/linalg/_decomp_schur.py:247:16: error[index-out-of-bounds] Index 0 is out of bounds for tuple `tuple[()]` with length 0
- scipy/optimize/_shgo_lib/_complex.py:895:27: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | list[Unknown]` is possibly unbound
- scipy/optimize/tests/test_chandrupatla.py:687:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["xatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:687:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["xatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:691:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["xatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:691:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["xatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:699:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["xrtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:699:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["xrtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:702:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["xrtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:702:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["xrtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:710:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["fatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:710:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["fatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:713:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["fatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:713:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["fatol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:719:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["frtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:719:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["frtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
- scipy/optimize/tests/test_chandrupatla.py:724:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, int].__setitem__(key: str, value: int, /) -> None` cannot be called with a key of type `Literal["frtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_chandrupatla.py:724:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, int].__setitem__(key: str, value: int, /) -> None)` cannot be called with a key of type `Literal["frtol"]` and a value of type `float` on object of type `tuple[Unknown] | dict[str, int]`
+ scipy/optimize/tests/test_direct.py:104:30: error[invalid-argument-type] Argument to function `direct` is incorrect: Expected `Iterable[Unknown] | Bounds`, found `int`
+ scipy/optimize/tests/test_direct.py:110:30: error[invalid-argument-type] Argument to function `direct` is incorrect: Expected `Iterable[Unknown] | Bounds`, found `int`
+ scipy/optimize/tests/test_direct.py:117:35: error[invalid-argument-type] Argument to function `direct` is incorrect: Expected `Iterable[Unknown] | Bounds`, found `int`
+ scipy/optimize/tests/test_direct.py:131:35: error[invalid-argument-type] Argument to function `direct` is incorrect: Expected `Iterable[Unknown] | Bounds`, found `int`
+ scipy/optimize/tests/test_direct.py:147:35: error[invalid-argument-type] Argument to function `direct` is incorrect: Expected `Iterable[Unknown] | Bounds`, found `int`
+ scipy/optimize/tests/test_direct.py:164:45: error[invalid-argument-type] Argument to function `direct` is incorrect: Expected `Iterable[Unknown] | Bounds`, found `int`
+ scipy/sparse/_index.py:330:30: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Unknown]`, found `(int & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
+ scipy/sparse/_index.py:339:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Unknown] | int`
- scipy/special/tests/test_basic.py:3711:46: error[invalid-argument-type] Argument to function `assert_` is incorrect: Expected `str | (() -> str)`, found `tuple[Unknown, Unknown]`
- scipy/special/tests/test_basic.py:3713:39: error[invalid-argument-type] Argument to function `assert_` is incorrect: Expected `str | (() -> str)`, found `tuple[Unknown, Unknown]`
- scipy/stats/tests/test_sampling.py:862:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | tuple[int | float, int | float]`, found `list[Unknown]`
- scipy/stats/tests/test_sampling.py:870:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | tuple[int | float, int | float]`, found `list[Unknown]`
- Found 6798 diagnostics
+ Found 6797 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-06 15:53_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuple-legacy-tvars?runnerMode=Instrumentation)

### Merging #19785 will **degrade performances by 6.15%**

<sub>Comparing <code>alex/tuple-legacy-tvars</code> (abc76b2) with <code>main</code> (b96929e)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>alex/remove-tuples</code> (c7de41d) during the generation of this report, so <code>main</code> (b96929e) was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`❌ 1` regressions  
`✅ 41` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuple-legacy-tvars?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_micro[many_string_assignments] `` | 74.3 ms | 79.1 ms | -6.15% |


---

_Comment by @AlexWaygood on 2025-08-06 16:04_

...but I can't repro any of the scipy diagnostics locally...

---

_Closed by @AlexWaygood on 2025-08-06 21:44_

---

_Marked ready for review by @AlexWaygood on 2025-08-06 21:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-06 21:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-06 21:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-06 21:44_

---

_Branch deleted on 2025-08-07 10:20_

---
