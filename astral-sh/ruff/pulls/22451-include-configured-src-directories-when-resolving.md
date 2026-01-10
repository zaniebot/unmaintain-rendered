```yaml
number: 22451
title: "Include configured `src` directories when resolving graphs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/src
created_at: 2026-01-08T02:31:55Z
updated_at: 2026-01-08T15:19:16Z
url: https://github.com/astral-sh/ruff/pull/22451
synced_at: 2026-01-10T16:30:32Z
```

# Include configured `src` directories when resolving graphs

---

_Pull request opened by @charliermarsh on 2026-01-08 02:31_

## Summary

This PR augments the detected source paths with the user-configured `src` when computing roots for `ruff analyze graph`.


---

_Renamed from "Include configured src directories when resolving graphs" to "Include configured `src` directories when resolving graphs" by @charliermarsh on 2026-01-08 02:33_

---

_Label `configuration` added by @charliermarsh on 2026-01-08 02:33_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 02:40_


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

_Review requested from @ntBre by @charliermarsh on 2026-01-08 02:48_

---

_Marked ready for review by @charliermarsh on 2026-01-08 02:48_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-08 02:53_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/analyze_graph.rs`:64 on 2026-01-08 07:31_

Should we use an `FxIndexSet` to avoid duplicate roots? I think I'd also expect that explicitly configured `src` roots take precedence over the automatically detected `package_roots` (which means, they have to come first).



---

_Review comment by @MichaReiser on `crates/ruff/src/commands/analyze_graph.rs`:80 on 2026-01-08 07:32_

> This field supports globs. For example, if you have a series of Python packages in a python_modules directory, src = ["python_modules/*"] would expand to incorporate all packages in that directory. User home directory and environment variables will also be expanded.

Are globs and environment variables already expanded at this point?

---

_@MichaReiser reviewed on 2026-01-08 07:33_

---

_@charliermarsh reviewed on 2026-01-08 14:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/commands/analyze_graph.rs`:80 on 2026-01-08 14:45_

Yeah I believe so, I think they're expanded by the time they reach `LinterSettings`.

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-08 14:45_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5366 diagnostics
+ Found 5361 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5160 diagnostics
+ Found 5161 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1838 diagnostics
+ Found 1837 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14490 diagnostics
+ Found 14491 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ruff/src/commands/analyze_graph.rs`:88 on 2026-01-08 14:57_

Nit:

```rust
src_roots.extend(
	pyproject_config.settings.linter.src.iter()
		.filter(|path| path.is_dir())
		.filter_map(|path| SystemPathBuf::from_path_buf(path.clone());
...
```

---

_@MichaReiser approved on 2026-01-08 14:58_

Let's add a test using a glob for `src` to confirm that they're expanded at this point.

---

_Comment by @charliermarsh on 2026-01-08 15:08_

🫡 

---

_Merged by @charliermarsh on 2026-01-08 15:19_

---

_Closed by @charliermarsh on 2026-01-08 15:19_

---

_Branch deleted on 2026-01-08 15:19_

---
