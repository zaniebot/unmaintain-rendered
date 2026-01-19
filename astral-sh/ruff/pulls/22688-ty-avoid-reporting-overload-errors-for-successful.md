```yaml
number: 22688
title: "[ty] Avoid reporting overload errors for successful union variants"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/success
created_at: 2026-01-18T20:28:22Z
updated_at: 2026-01-19T08:09:15Z
url: https://github.com/astral-sh/ruff/pull/22688
synced_at: 2026-01-19T08:25:13Z
```

# [ty] Avoid reporting overload errors for successful union variants

---

_@charliermarsh_

## Summary

Consider `x: str | bytes` and then `x.split(" ")`. Because we have a union, and at least one variant errors (`bytes` expects a `Buffer`, not a `str`), we call `binding.report_diagnostics` for each variant. For the `str` variant, it has two overloads that both match arity, but only one actually matches the signature... So `matching_overload_before_type_checking` is `None` (because they both match arity), but we don't actually have an error, and we fall through to `NO_MATCHING_OVERLOAD`.

If one variant succeeds, we should avoid reporting errors for it, even if not _all_ variants matched.


---

_Label `ty` added by @charliermarsh on 2026-01-18 20:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 20:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 20:31_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3898:38: error[no-matching-overload] No overload of bound method `split` matches arguments
- Found 30 diagnostics
+ Found 29 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/test/cmd/develop.py:265:18: error[no-matching-overload] No overload matches arguments
- lib/spack/spack/util/gpg.py:353:9: error[no-matching-overload] No overload matches arguments
- lib/spack/spack/vendor/jinja2/environment.py:1612:17: error[no-matching-overload] No overload of bound method `writelines` matches arguments
- Found 4344 diagnostics
+ Found 4341 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_kits/hierarchies.py:44:24: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:82:24: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:125:30: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:139:17: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:181:25: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:185:25: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:232:30: error[no-matching-overload] No overload of bound method `get` matches arguments
- kopf/_kits/hierarchies.py:233:21: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- Found 268 diagnostics
+ Found 260 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/config.py:2490:9: error[no-matching-overload] No overload of bound method `pop` matches arguments
- Found 76 diagnostics
+ Found 75 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/routing.py:582:17: error[no-matching-overload] No overload of bound method `match` matches arguments
- Found 328 diagnostics
+ Found 327 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_pytypes.py:457:29: error[no-matching-overload] No overload of class `str` matches arguments
- Found 216 diagnostics
+ Found 215 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/config.py:133:13: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- pydantic/v1/config.py:133:13: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- Found 3157 diagnostics
+ Found 3155 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/metaschema.py:1037:24: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/metaschema.py:1038:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/metaschema.py:1039:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/python_codegen_support.py:1034:24: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/python_codegen_support.py:1035:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/python_codegen_support.py:1036:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 243 diagnostics
+ Found 237 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/exceptions.py:219:23: error[no-matching-overload] No overload of class `str` matches arguments
- Found 422 diagnostics
+ Found 421 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/config/cc_rh_subscription.py:253:29: error[no-matching-overload] No overload of bound method `split` matches arguments
- Found 1169 diagnostics
+ Found 1168 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/conventions.py:349:12: error[no-matching-overload] No overload of bound method `get` matches arguments
- xarray/tests/test_backends_file_manager.py:173:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:176:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:179:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:198:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:200:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:202:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:214:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:271:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:287:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- Found 1762 diagnostics
+ Found 1752 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/checker.py:121:53: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:121:53: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:124:60: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:124:60: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/command_line_tool.py:615:50: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/command_line_tool.py:620:32: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/process.py:1221:37: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 252 diagnostics
+ Found 245 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/contrib/integration_registry/registry_update_helpers/integration_registry_manager.py:161:24: error[no-matching-overload] No overload of function `getattr` matches arguments
- Found 8415 diagnostics
+ Found 8414 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

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
- Found 5411 diagnostics
+ Found 5406 diagnostics

altair (https://github.com/vega/altair)
- altair/vegalite/v6/api.py:913:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 1056 diagnostics
+ Found 1055 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/_stochastic_gradient.py:785:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- sklearn/linear_model/_stochastic_gradient.py:785:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- sklearn/linear_model/_stochastic_gradient.py:785:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- Found 2423 diagnostics
+ Found 2420 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/algorithms.py:552:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:552:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:583:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:583:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/arrays/string_.py:804:22: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/dtypes/cast.py:1545:26: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/nanops.py:1557:30: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/nanops.py:1559:30: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/io/pytables.py:4837:26: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- pandas/tests/series/test_cumulative.py:48:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:48:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:54:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:54:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:154:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:154:20: error[no-matching-overload] No overload matches arguments
- Found 3755 diagnostics
+ Found 3740 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14496 diagnostics
+ Found 14497 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/linalg/_isolve/iterative.py:1003:19: error[no-matching-overload] No overload of function `vdot` matches arguments
- scipy/stats/_mstats_basic.py:667:16: error[no-matching-overload] No overload of bound method `any` matches arguments
- Found 8106 diagnostics
+ Found 8104 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 20:56_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 20:56_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 20:56_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 20:56_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 20:56_

---

_Review requested from @dhruvmanila by @charliermarsh on 2026-01-18 20:56_

---

_@dhruvmanila reviewed on 2026-01-19 05:13_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:332 on 2026-01-19 05:13_

Should this be moved inside `CallableBinding::report_diagnostics` so that the other call (right above this) can also benefit?

---

_@dhruvmanila reviewed on 2026-01-19 05:14_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:332 on 2026-01-19 05:14_

Hmm, actually, that's not required because for a single binding (non-union type), the `Bindings::report_diagnostics` will only be called when there's actually an error. So, for a successful overload matching, this method won't be called in the first place.

---

_@dhruvmanila approved on 2026-01-19 05:14_

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2026-01-19 07:56_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 08:01_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `no-matching-overload` | 0 | 16 | 0 |
| `invalid-argument-type` | 2 | 1 | 5 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 0 | 0 | 4 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **2** | **19** | **21** |


**[Full report with detailed diff](https://28e81978.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://28e81978.ty-ecosystem-ext.pages.dev/timing))



---
