```yaml
number: 22053
title: "[ty] Add `ty.configuration` and `ty.configurationFile` options"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/ty-server-configuration
created_at: 2025-12-18T17:29:37Z
updated_at: 2025-12-22T15:13:21Z
url: https://github.com/astral-sh/ruff/pull/22053
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Add `ty.configuration` and `ty.configurationFile` options

---

_@MichaReiser_

## Summary

This PR adds two new LSP settings:

* `configuration`: Inline configuration similar to `ty check --config`. Allows setting configurations in addition to what's configured in the project
* `configurationFile`: A path to a `ty.toml` configuration file to use instead of any auto-discovered configuration

Fixes https://github.com/astral-sh/ty/issues/786

Related to https://github.com/astral-sh/ty/issues/1084, although I think we probably want to have dedicated editor toggles for some settings because editing the inline JSON is no fun.

## Known limitations

Relative paths in the `ty.toml` file will be relative to the workspace root, which might be surprising if the feature is used in a case as described in https://github.com/astral-sh/ty/issues/2034 where the project is in a sub-directory. 

I think the proper solution for https://github.com/astral-sh/ty/issues/2034 simply is better mono-repository support or supporting workspace-folders. 

## Test Plan

Added E2E tests


---

_Label `server` added by @MichaReiser on 2025-12-18 17:29_

---

_Label `ty` added by @MichaReiser on 2025-12-18 17:29_

---

_Label `server` added by @MichaReiser on 2025-12-18 17:29_

---

_Label `ty` added by @MichaReiser on 2025-12-18 17:29_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1843 diagnostics
+ Found 1844 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2805 diagnostics
+ Found 2802 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5097 diagnostics
+ Found 5098 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14425 diagnostics
+ Found 14424 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:46_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-12-18 17:54_

---

_Review requested from @carljm by @MichaReiser on 2025-12-18 17:54_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-18 17:54_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-18 17:54_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-18 17:54_

---

_Review request for @carljm removed by @carljm on 2025-12-18 17:56_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 18:01_

---

_Review requested from @Gankra by @MichaReiser on 2025-12-19 09:24_

---

_Review requested from @carljm by @MichaReiser on 2025-12-22 11:11_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-22 11:11_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:466 on 2025-12-22 11:47_

```suggestion
            tracing::debug!("Initializing workspace `{url}`: {workspace:#?}");
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:195 on 2025-12-22 11:53_

nit: we could split the message in multiple lines so that rustfmt can work here

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/configuration.rs`:1 on 2025-12-22 11:57_

Can we add another test case to see how it handles when a user provides invalid configuration values and an invalid configuration file?

---

_@dhruvmanila approved on 2025-12-22 11:59_

This looks great!

todo: add these options to the VS Code extension and update the documentation (ignore if there are PRs already)

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/configuration.rs`:1 on 2025-12-22 13:54_

I added a test for an invalid configuration file. Invalid overrides is rather annoying to test because the mock server doesn't allow me to pass arbitrary JSON as the config value

---

_@MichaReiser reviewed on 2025-12-22 13:54_

---

_Comment by @MichaReiser on 2025-12-22 14:45_

Ugh windows

---

_Merged by @MichaReiser on 2025-12-22 15:13_

---

_Closed by @MichaReiser on 2025-12-22 15:13_

---

_Branch deleted on 2025-12-22 15:13_

---
