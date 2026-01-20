```yaml
number: 22776
title: "[ty] show final search path instead of \"and 1 more paths\""
type: pull_request
state: merged
author: mswart
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: show-one-more-path
created_at: 2026-01-20T20:45:11Z
updated_at: 2026-01-20T23:38:53Z
url: https://github.com/astral-sh/ruff/pull/22776
synced_at: 2026-01-20T23:51:43Z
```

# [ty] show final search path instead of "and 1 more paths"

---

_@mswart_

## Summary

If there are six search paths, five are attached as subdiagnostic of unresolved imports but the sixth is by default hidden and replaced by "... and 1 more paths. Run with `-v` to see all paths."

```
info: Searched in the following paths during module resolution:
info:   1. <temp_dir>/extra1 (extra search path specified on the CLI or in your config file)
info:   2. <temp_dir>/extra2 (extra search path specified on the CLI or in your config file)
info:   3. <temp_dir>/extra3 (extra search path specified on the CLI or in your config file)
info:   4. <temp_dir>/extra4 (extra search path specified on the CLI or in your config file)
info:   5. <temp_dir>/ (first-party code)
info:   ... and 1 more paths. Run with `-v` to see all paths.
```

By hiding a single path this truncation does not shorten the output but still requires the user to rerun ty. We can just include the final search path instead.
The subdiagnostic "and 1 more paths" isn't helpful - we can show the hidden path instead (and still have the same number of subdiagnistics).

```
info: Searched in the following paths during module resolution:
info:   1. <temp_dir>/extra1 (extra search path specified on the CLI or in your config file)
info:   2. <temp_dir>/extra2 (extra search path specified on the CLI or in your config file)
info:   3. <temp_dir>/extra3 (extra search path specified on the CLI or in your config file)
info:   4. <temp_dir>/extra4 (extra search path specified on the CLI or in your config file)
info:   5. <temp_dir>/ (first-party code)
info:   6. vendored://stdlib (stdlib typeshed stubs vendored by ty)
```

## Test Plan

* cargo test extended
* manual invocation (with the configuration that prompted me to propose this change).

---

_Review requested from @carljm by @mswart on 2026-01-20 20:45_

---

_Review requested from @AlexWaygood by @mswart on 2026-01-20 20:45_

---

_Review requested from @sharkdp by @mswart on 2026-01-20 20:45_

---

_Review requested from @dcreager by @mswart on 2026-01-20 20:45_

---

_Review requested from @MichaReiser by @mswart on 2026-01-20 20:45_

---

_@mswart reviewed on 2026-01-20 21:09_

---

_Review comment by @mswart on `crates/ty_python_semantic/src/types/infer/builder.rs`:8137 on 2026-01-20 21:09_

This could be implemented with `.next()`, too: we only count the items afterwards, but that feels a bit magic.

---

_Label `ty` added by @AlexWaygood on 2026-01-20 23:05_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 23:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 23:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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
- Found 5407 diagnostics
+ Found 5412 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1821 diagnostics
+ Found 1825 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2026-01-20 23:38_

Thank you!

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-20 23:38_

---

_Merged by @AlexWaygood on 2026-01-20 23:38_

---

_Closed by @AlexWaygood on 2026-01-20 23:38_

---
