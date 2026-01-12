```yaml
number: 22025
title: "[ty] Unwrap `enum.nonmember` values"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/non
created_at: 2025-12-17T13:48:10Z
updated_at: 2025-12-19T00:59:51Z
url: https://github.com/astral-sh/ruff/pull/22025
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Unwrap `enum.nonmember` values

---

_@charliermarsh_

## Summary

This PR unwraps the `enum.nonmember` type to match runtime behavior.

Closes https://github.com/astral-sh/ty/issues/1974.


---

_Comment by @astral-sh-bot[bot] on 2025-12-17 13:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-19 00:07:35.660229877 +0000
+++ new-output.txt	2025-12-19 00:07:37.828248076 +0000
@@ -420,7 +420,7 @@
 enums_members.py:84:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:85:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `def speak(self) -> None`
 enums_members.py:85:33: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:116:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | nonmember[int]`
+enums_members.py:116:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `int`
 enums_members.py:116:32: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
 enums_members.py:129:9: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | Literal[2]`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-17 13:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5086 diagnostics
+ Found 5085 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-17 14:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fnon?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22025 will **not alter performance**

<sub>Comparing <code>charlie/non</code> (ab51c81) with <code>main</code> (5a2d3cd)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fnon?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ty` added by @ntBre on 2025-12-17 14:06_

---

_Renamed from "Unwrap `enum.nonmember` values" to "[ty] Unwrap `enum.nonmember` values" by @charliermarsh on 2025-12-17 14:06_

---

_Marked ready for review by @charliermarsh on 2025-12-17 14:31_

---

_Review requested from @carljm by @charliermarsh on 2025-12-17 14:31_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-17 14:31_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-17 14:31_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-17 14:31_

---

_Converted to draft by @charliermarsh on 2025-12-17 14:45_

---

_Marked ready for review by @charliermarsh on 2025-12-17 15:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2370 on 2025-12-17 22:36_

This is a pretty subtle pair of conditions that need testing, and it looks like we already have the same check (with sense reversed) inside `enum_metadata`. Can we extract it as a reusable function?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:922 on 2025-12-17 22:37_

Do we have a test for the case where this union handling is necessary?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:6012 on 2025-12-17 22:42_

Maybe this belongs better in `types/enums.rs`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2373 on 2025-12-17 22:46_

The `member` variable above already has the `Nonmember` type. Looking at the implementation of `extract_nonmember_value`, it seems like we go through a lot of work just to get that same `Nonmember` instance type back again, and then we get its `value` attribute. So why can't we just work directly from the type we already have in hand, without going back to the place table etc?

---

_@carljm reviewed on 2025-12-17 22:46_

---

_Review requested from @carljm by @charliermarsh on 2025-12-18 00:59_

---

_Comment by @AlexWaygood on 2025-12-18 15:41_

I'll leave this to @carljm (since he's already reviewed) and/or @sharkdp (who originally implemented enums and `member`/`nonmember` for us)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 15:41_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/enums.md`:467 on 2025-12-18 19:34_

What happens if the wrapped types in the two branches are different? Should we union the wrapped types? Right now it seems we just arbitrarily pick the first one?

And what happens if some branches of the union are `nonmember(...)` and others are not?

I'm actually wondering why you added the union handling for this in the first place? (I should have asked that question originally, instead of just asking if we had a test for it.) It looks like `enum_metadata` currently won't respect this case as a non-member, and we should probably keep this logic in sync with `enum_metadata` -- so maybe we shouldn't handle unions at all in `try_unwrap_nonmember_value`? Unless you saw an important reason somewhere why we need to do so.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/enums.rs`:339 on 2025-12-18 19:44_

As commented above, I'm wondering if we should even have this branch at all, given I don't think the nonmember handling in `enum_metadata` handles unions. If we do need it, I think it should be more principled than just arbitrarily grabbing the first nonmember type it sees (more like: union all nonmember types found -- and decide what we should do with mixed types, which is really not clear).

---

_@carljm reviewed on 2025-12-18 19:45_

Looks good!

---

_@charliermarsh reviewed on 2025-12-18 20:30_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/enums.rs`:339 on 2025-12-18 20:30_

I was using this as a crutch to avoid `Unknown | int` which is obviously bad (and not entirely intended -- I only just noticed that when I tried to remove). I removed this and changed the non-member inference to use only the inferred type from the binding rather than combining the declaration (which would add `Unknown`), which I _think_ is consistent with `enum_metadata`?

---

_@carljm reviewed on 2025-12-18 21:36_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/enums.rs`:339 on 2025-12-18 21:36_

So I think your latest approach is right in principle, but sadly it looks like a significant regression, because now we're doing a bunch of extra work to re-lookup the attribute, for any attribute lookup on any enum class.

The ideal solution here would be to move the code in `place_by_id` that does the actual "union with Unknown" out into `Place`, so that we'd have a way to get the type of a `Place` without the `Unknown`, without having to reverse-engineer a union type. That seems pretty doable but it won't be trivial, probably better as a separate PR.

To get this landed probably the best approach is to go back to handling unions in `try_unwrap_...`, but maybe in a more targeted way, so we'll ignore `Unknown` and otherwise expect there to be exactly one `nonmember` type in the union, otherwise return `None`. And caption this with a TODO comment that it's a hack and what we should really do is pull the Unknown unioning up into Place instead.



---

_@charliermarsh reviewed on 2025-12-19 00:06_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/enums.rs`:339 on 2025-12-19 00:06_

(Reverted back for now.)

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 00:21_

---

_@carljm approved on 2025-12-19 00:46_

ship it

---

_Merged by @charliermarsh on 2025-12-19 00:59_

---

_Closed by @charliermarsh on 2025-12-19 00:59_

---

_Branch deleted on 2025-12-19 00:59_

---
