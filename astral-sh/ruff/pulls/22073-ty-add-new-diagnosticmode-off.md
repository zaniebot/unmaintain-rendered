```yaml
number: 22073
title: "[ty] Add new `diagnosticMode: off`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/diagnostic-mode-off
created_at: 2025-12-19T10:04:23Z
updated_at: 2025-12-22T15:46:04Z
url: https://github.com/astral-sh/ruff/pull/22073
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Add new `diagnosticMode: off`

---

_Pull request opened by @MichaReiser on 2025-12-19 10:04_

## Summary

This PR adds a new `diagnosticMode: off`, for users that want to use ty's LSP functionality only (go to def, ...) but don't want to see any type errors.

I took the easy way here where the LSP returns an empty response for pull diagnostics rather than changing the registered capabilities. Happy to pursue that if people think it's worth the time.

We may want to add a separate `disableSyntaxErrors` option for users that want to use ty alongside other Python LSPs. I created a new issue for that.

Closes https://github.com/astral-sh/ty/issues/1755

## Test Plan

Added tests

https://github.com/user-attachments/assets/3041db43-e6f5-401b-9b83-4ebe278e0db1



---

_Review requested from @carljm by @MichaReiser on 2025-12-19 10:04_

---

_Label `configuration` added by @MichaReiser on 2025-12-19 10:04_

---

_Label `server` added by @MichaReiser on 2025-12-19 10:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-19 10:04_

---

_Label `ty` added by @MichaReiser on 2025-12-19 10:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-19 10:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 10:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 10:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2799 diagnostics
+ Found 2802 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14414 diagnostics
+ Found 14415 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@charliermarsh approved on 2025-12-20 19:05_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-22 11:12_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:466 on 2025-12-22 11:23_

```suggestion
            tracing::debug!("Initializing workspace `{url}`: {global:#?}");
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/diagnostic.rs`:40 on 2025-12-22 11:28_

I think I'd prefer if we take the route of updating the capabilities which doesn't seem too difficult (?) given that we already do that when switching between `openFilesOnly` and `workspace`: https://github.com/astral-sh/ruff/blob/fee4e2d72a2f56e819251b6297efd263af7dc158/crates/ty_server/src/session.rs#L616-L626

The client won't need to keep track of diagnostics and send any request then.

---

_@dhruvmanila reviewed on 2025-12-22 11:32_

This looks good but I'd prefer the approach to change the capabilities instead (the one that you've mentioned in the description). I'm also fine if you want to do it as a follow-up.

I was wondering if we need to change anything for `db.set_check_mode` here: https://github.com/astral-sh/ruff/blob/fee4e2d72a2f56e819251b6297efd263af7dc158/crates/ty_server/src/session.rs#L550-L558

but I think it's fine now given that changing the configuration value requires a server restart.

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-22 13:37_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/diagnostic.rs`:36 on 2025-12-22 15:24_

Is this still required?

---

_@dhruvmanila approved on 2025-12-22 15:24_

---

_@MichaReiser reviewed on 2025-12-22 15:25_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/diagnostic.rs`:36 on 2025-12-22 15:25_

Yes, for clients that don't support dynamic registration

---

_Merged by @MichaReiser on 2025-12-22 15:46_

---

_Closed by @MichaReiser on 2025-12-22 15:46_

---

_Branch deleted on 2025-12-22 15:46_

---
