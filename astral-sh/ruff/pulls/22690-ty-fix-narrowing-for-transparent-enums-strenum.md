```yaml
number: 22690
title: "[ty] Fix narrowing for transparent enums (StrEnum, IntEnum)"
type: pull_request
state: open
author: bxff
labels:
  - ty
assignees: []
base: main
head: fix-enum-custom-eq-narrowing
created_at: 2026-01-18T21:14:30Z
updated_at: 2026-01-20T00:02:59Z
url: https://github.com/astral-sh/ruff/pull/22690
synced_at: 2026-01-20T00:49:15Z
```

# [ty] Fix narrowing for transparent enums (StrEnum, IntEnum)

---

_@bxff_

## Summary

This PR fixes incorrect type narrowing in `match` statements for enums with "transparent" equality (`StrEnum`, `IntEnum`, etc.) where enum members compare equal to their underlying primitive values.

**Problem**: When matching a primitive literal (e.g., `Literal["g"]`) against a transparent enum member (e.g., `Color.GREEN`), the type checker incorrectly narrowed the type to `Never` because it treated them as disjoint types. This was reported in issue https://github.com/astral-sh/ty/issues/1454.

**Root Cause**: 
1. `is_disjoint_from` treated enum literals and primitive literals as disjoint based solely on type differences
2. `IntersectionBuilder` couldn't simplify intersections like `Literal["g"] & ~Color.GREEN` correctly

**Solution**:
1. Added `has_transparent_equality()` in `enums.rs` to explicitly detect enums inheriting from both `Enum` and primitive types (`str`, `int`, `bytes`)
2. Modified `is_disjoint_from_impl()` in `relation.rs` to compare underlying values when dealing with transparent enums instead of just checking type identity
3. Updated `add_negative()` in `builder.rs` to correctly simplify `PrimitiveLiteral & ~TransparentEnumLiteral` to `Never`

## Test Plan

- **New regression tests**: Created `match_enums.md` with targeted tests for transparent enum narrowing in match statements
- All tests pass successfully.

---

_Review requested from @carljm by @bxff on 2026-01-18 21:14_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-18 21:14_

---

_Review requested from @sharkdp by @bxff on 2026-01-18 21:14_

---

_Review requested from @dcreager by @bxff on 2026-01-18 21:14_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 21:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 21:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1821 diagnostics
+ Found 1825 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14492 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-18 21:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/relation.rs`:1868 on 2026-01-18 21:22_

this isn't correct: an enum-literal type _should_ always be considered disjoint from an int-literal type, even if the enum class inherits from `int`.

The underlying problem is that we shouldn't be using concepts like disjointness or single-valuedness to implement our equality-based narrowing in `narrow.rs` at all. A value `x` of type `X` can still compare equal to a value `y` of type `Y` even if `X` and `Y` are both single-valued types that both have well-behaved `__eq__` methods and are both mutually disjoint.

---

_@AlexWaygood reviewed on 2026-01-18 21:23_

thanks! Unfortunately, I'm not sure your understanding of the problem here is correct

---

_Comment by @codspeed-hq[bot] on 2026-01-18 21:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/bxff%3Afix-enum-custom-eq-narrowing?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **not alter performance**

<sub>Comparing <code>bxff:fix-enum-custom-eq-narrowing</code> (ee4dccd) with <code>main</code> (d4a0150)</sub>



### Summary

`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/bxff%3Afix-enum-custom-eq-narrowing?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@bxff reviewed on 2026-01-19 23:07_

---

_Review comment by @bxff on `crates/ty_python_semantic/src/types/relation.rs`:1868 on 2026-01-19 23:07_

I've updated the implementation to address this:

- Removed all changes to relation.rs — enum literals remain disjoint from primitive literals as they should be
- The fix is now purely in the narrowing logic (narrow.rs and reachability_constraints.rs), where we check if a transparent enum's underlying value equals the match subject

This keeps type relations correct while still handling the runtime equality behavior of StrEnum/IntEnum in match statement narrowing.

---
