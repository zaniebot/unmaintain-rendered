```yaml
number: 22031
title: "[ty] Classify docstrings in semantic tokens (syntax highlighting)"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/doctok
created_at: 2025-12-17T17:32:27Z
updated_at: 2025-12-19T18:36:03Z
url: https://github.com/astral-sh/ruff/pull/22031
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Classify docstrings in semantic tokens (syntax highlighting)

---

_@Gankra_

## Summary

* Related to, but does not handle https://github.com/astral-sh/ty/issues/2021

## Test Plan

I also added some snapshot tests for future work on non-standard attribute docstrings (didn't want to highlight them if we don't recognize them elsewhere).


---

_Review requested from @carljm by @Gankra on 2025-12-17 17:32_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-17 17:32_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-17 17:32_

---

_Review requested from @sharkdp by @Gankra on 2025-12-17 17:32_

---

_Review requested from @dcreager by @Gankra on 2025-12-17 17:32_

---

_Label `server` added by @Gankra on 2025-12-17 17:32_

---

_Label `ty` added by @Gankra on 2025-12-17 17:32_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 17:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 17:35_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

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
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 43 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/semantic_tokens.rs`:201 on 2025-12-17 17:49_

nit

```suggestion
#[expect(clippy::struct_excessive_bools)]
```

---

_@AlexWaygood approved on 2025-12-17 17:54_

Nice!

---

_Comment by @MichaReiser on 2025-12-17 17:59_

I don't think this fixes the linked issue. The issue is that we aren't correctly encoding multiline tokens for clients not supporting it. I'm working on a fix for that

---

_Comment by @AlexWaygood on 2025-12-17 18:00_

> I don't think this fixes the linked issue. The issue is that we aren't correctly encoding multiline tokens for clients not supporting it. I'm working on a fix for that

I think that's why @Gankra wrote that it's "related to, but does not handle" that issue :-)

---

_Comment by @Gankra on 2025-12-17 18:01_

(I stealth edited that in)

---

_Comment by @AlexWaygood on 2025-12-17 18:02_

ah too stealthy for me

---

_Review request for @carljm removed by @carljm on 2025-12-17 18:09_

---

_@MichaReiser reviewed on 2025-12-17 18:22_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:837 on 2025-12-17 18:22_

I think we still need to walk the string in case it's an implicitly concatenated string literal or we incorrectly classify anything between the string parts as string too

---

_@Gankra reviewed on 2025-12-17 19:59_

---

_Review comment by @Gankra on `crates/ty_ide/src/semantic_tokens.rs`:837 on 2025-12-17 19:59_

Curses, I ran afoul of Micha's Terror once more

---

_Comment by @MichaReiser on 2025-12-19 16:20_

@Gankra we might want to land this, see https://github.com/astral-sh/ty/issues/2021#issuecomment-3674956489

---

_@Gankra reviewed on 2025-12-19 17:13_

---

_Review comment by @Gankra on `crates/ty_ide/src/semantic_tokens.rs`:837 on 2025-12-19 17:13_

Hmm could you provide an example that would show the problem you expect?

---

_@MichaReiser reviewed on 2025-12-19 17:15_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:837 on 2025-12-19 17:15_

you'll be so annoyed by these examples 😆 

```py
def test():
	"docstring" \
	"more"

def other():
	(
		"docstring" 
		# comment
		"more"
	)
```



---

_@Gankra reviewed on 2025-12-19 18:01_

---

_Review comment by @Gankra on `crates/ty_ide/src/semantic_tokens.rs`:837 on 2025-12-19 18:01_

No these rule, thanks!

---

_Comment by @Gankra on 2025-12-19 18:30_

I also added a bunch of my own variations and equivalent tests for hovers -- I'm not sure which people expect to work in practice but at least we have snapshots for what we do, and the two implementations seem to agree now.

---

_Comment by @Gankra on 2025-12-19 18:31_

Well except for on the "spill" test which I think is so marginal I don't care to fix it X3

---

_Merged by @Gankra on 2025-12-19 18:36_

---

_Closed by @Gankra on 2025-12-19 18:36_

---

_Branch deleted on 2025-12-19 18:36_

---
