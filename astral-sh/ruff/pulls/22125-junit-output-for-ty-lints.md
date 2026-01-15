```yaml
number: 22125
title: "JUnit output for `ty` lints"
type: pull_request
state: open
author: cetanu
labels:
  - ty
assignees: []
base: main
head: cetanu/junit_for_ty
created_at: 2025-12-21T04:49:52Z
updated_at: 2026-01-15T11:14:29Z
url: https://github.com/astral-sh/ruff/pull/22125
synced_at: 2026-01-15T11:54:41Z
```

# JUnit output for `ty` lints

---

_@cetanu_

## Summary

Adding JUnit XML output for `ty`

## Test Plan

Reusing existing tests, still working this out, will amend this description later when I figure out how to specifically test `ty` lints, it seems they already are but will verify


---

_Comment by @cetanu on 2025-12-21 04:51_

Slowly working on this one

Links back to https://github.com/astral-sh/ty/issues/1476

---

_Label `ty` added by @AlexWaygood on 2025-12-21 18:01_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 18:04_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-21 18:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5526 diagnostics
+ Found 5531 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2801 diagnostics
+ Found 2798 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | Top[Yarn[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1842 diagnostics
+ Found 1841 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-21 18:11_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@cetanu reviewed on 2025-12-22 10:04_

---

_Review comment by @cetanu on `crates/ruff/tests/cli/snapshots/cli__format__output_format_junit.snap`:21 on 2025-12-22 10:04_

I don't yet understand why(or how) incorrect XML is being produced

When I run either ruff or ty myself, this doesn't seem to be the case

Also, this would be a change against ruff, but I am only trying to change ty - so perhaps I need to separate this behavior somehow

---

_@MichaReiser reviewed on 2025-12-22 11:06_

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/snapshots/cli__format__output_format_junit.snap`:21 on 2025-12-22 11:06_

Our snapshot test infrastructure replaces content that are system dependent (e.g. the path to the test) before storing the snapshot. I'm not sure what the original message looks like but it seems that the replacement logic goes a bit overboard and replaces more than it should?

---

_Comment by @MichaReiser on 2025-12-22 11:16_

Thanks for working on this. For an initial version, I suggest focusing on just rendering the `concise` diagnostic message without any subdiagnostics. The main reason is that I don't want to duplicate the rendering logic with what we already have in `FullRenderer`. 

---

_Comment by @cetanu on 2026-01-04 13:09_

I removed the code for handling sub diagnostics and snippets with the full renderer and instead just print out the concise message

I changed how the output is formatted, making the XML a bit easier on human eyes for both ruff and ty junit formats, but it was done by specifying indents and newlines which may be brittle if the format is changed in future.

I can squash these commits if needed. I left them as-is in case some of the history is useful in future

---

_@MichaReiser reviewed on 2026-01-04 17:37_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:52 on 2026-01-04 17:37_

Can you say more about the motivation for adding the indent?

XML isn't a format optimized for human readability and the indent might lead to undesired rendering in tools showing the message.

---

_@cetanu reviewed on 2026-01-05 05:20_

---

_Review comment by @cetanu on `crates/ruff_db/src/diagnostic/render/junit.rs`:52 on 2026-01-05 05:20_

No worries, I don't feel strongly about it's addition, the motivation was mostly to help myself to see changes in the produced XML; I'll take this out

---

_Comment by @MichaReiser on 2026-01-12 08:27_

Is this PR ready for review ?

---

_Marked ready for review by @cetanu on 2026-01-15 11:13_

---

_Review requested from @carljm by @cetanu on 2026-01-15 11:13_

---

_Review requested from @sharkdp by @cetanu on 2026-01-15 11:13_

---

_Review requested from @dcreager by @cetanu on 2026-01-15 11:13_

---

_Review requested from @Gankra by @cetanu on 2026-01-15 11:13_

---

_Comment by @cetanu on 2026-01-15 11:14_

Yes!
Let me know if any changes or if commits need to be rebased

---
