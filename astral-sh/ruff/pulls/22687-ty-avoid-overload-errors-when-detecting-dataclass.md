```yaml
number: 22687
title: "[ty] Avoid overload errors when detecting dataclass-on-tuple"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: charlie/data-decorator
created_at: 2026-01-18T19:34:12Z
updated_at: 2026-01-18T20:13:12Z
url: https://github.com/astral-sh/ruff/pull/22687
synced_at: 2026-01-18T20:18:40Z
```

# [ty] Avoid overload errors when detecting dataclass-on-tuple

---

_@charliermarsh_

## Summary

Fixes some TODOs introduced in #22672 around cases like the following:


```python
from collections import namedtuple
from dataclasses import dataclass

NT = namedtuple("NT", "x y")

# error: [invalid-dataclass] "Cannot use `dataclass()` on a `NamedTuple` class"
dataclass(NT)
```

On main, `dataclass(NT)` emits `# error: [no-matching-overload]` instead, which is wrong -- the overload does match! On main, the logic proceeds as follows:

1. `dataclass` has two overloads:
  - `dataclass(cls: type[_T], ...) -> type[_T]`
  - `dataclass(cls: None = None, ...) -> Callable[[type[_T]], type[_T]]`
2. When `dataclass(NT)` is called:
  - Arity check: Both overloads accept one positional argument, so both pass.
  - Type checking on first overload: `NT` matches `type[_T]`... but then `invalid_dataclass_target()` runs and adds `InvalidDataclassApplication` error
  - Type checking on second overload: `NT` doesn't match `None`, so we have a type error.
3. After type checking, both overloads have errors.
4. `matching_overload_index()` filters by `overload.as_result().is_ok()`, which checks if `errors.is_empty()`. Since both overloads have errors, neither matches...
5. We emit the "No overload matches arguments" error.

Instead, we now differentiate between non-matching errors, and errors that occur when we _do_ match, but the call has some other semantic failure.


---

_Review requested from @dhruvmanila by @charliermarsh on 2026-01-18 19:34_

---

_Label `ty` added by @charliermarsh on 2026-01-18 19:34_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 19:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 19:37_


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

pybind11 (https://github.com/pybind/pybind11)
- tests/test_pytypes.py:457:29: error[no-matching-overload] No overload of class `str` matches arguments
- Found 216 diagnostics
+ Found 215 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/routing.py:582:17: error[no-matching-overload] No overload of bound method `match` matches arguments
- Found 328 diagnostics
+ Found 327 diagnostics

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

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/checker.py:121:53: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:121:53: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:124:60: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:124:60: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/command_line_tool.py:615:50: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/command_line_tool.py:620:32: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 252 diagnostics
+ Found 246 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/exceptions.py:219:23: error[no-matching-overload] No overload of class `str` matches arguments
- Found 422 diagnostics
+ Found 421 diagnostics

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
- Found 5406 diagnostics
+ Found 5411 diagnostics

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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

altair (https://github.com/vega/altair)
- altair/vegalite/v6/api.py:913:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 1056 diagnostics
+ Found 1055 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

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
- pandas/tests/series/test_cumulative.py:48:20: error[no-matching-overload] No overload matches arguments
- Found 3755 diagnostics
+ Found 3746 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/linalg/_isolve/iterative.py:1003:19: error[no-matching-overload] No overload of function `vdot` matches arguments
- scipy/stats/_mstats_basic.py:667:16: error[no-matching-overload] No overload of bound method `any` matches arguments
- Found 8106 diagnostics
+ Found 8104 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@charliermarsh reviewed on 2026-01-18 19:39_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/call/bind.rs`:2446 on 2026-01-18 19:39_

I... _think_ this was impossible on main? Because if we had exactly one matching overload without errors, we'd never call `report_diagnostics`? But now, we _can_ have one overload that matches but still emits an error.

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-18 19:40_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 19:46_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 5 | 5 |
| `no-matching-overload` | 0 | 9 | 0 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 4 |
| **Total** | **0** | **14** | **14** |


**[Full report with detailed diff](https://b1fb9560.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://b1fb9560.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @charliermarsh on 2026-01-18 19:57_

So in `aioredis-py`, we now show this:

```
error[invalid-argument-type]: Argument to bound method `split` is incorrect
    --> aioredis-py/aioredis/client.py:3898:53
     |
3896 |         if isinstance(response, bytes):
3897 |             response = self.connection.encoder.decode(response, force=True)
3898 |         command_time, command_data = response.split(" ", 1)
     |                                                     ^^^ Expected `Buffer | None`, found `Literal[" "]`
3899 |         m = self.monitor_re.match(command_data)
3900 |         if m is None:
     |
info: Method defined here
    --> stdlib/builtins.pyi:1761:9
     |
1759 |         """
1760 |
1761 |     def split(self, sep: ReadableBuffer | None = None, maxsplit: SupportsIndex = -1) -> list[bytes]:
     |         ^^^^^       --------------------------------- Parameter declared here
1762 |         """Return a list of the sections in the bytes, using sep as the delimiter.
```

But _not_ the following:

```
error[no-matching-overload]: No overload of bound method `split` matches arguments
    --> aioredis-py/aioredis/client.py:3898:38
     |
3896 |         if isinstance(response, bytes):
3897 |             response = self.connection.encoder.decode(response, force=True)
3898 |         command_time, command_data = response.split(" ", 1)
     |                                      ^^^^^^^^^^^^^^^^^^^^^^
3899 |         m = self.monitor_re.match(command_data)
3900 |         if m is None:
     |
info: First overload defined here
    --> stdlib/builtins.pyi:1276:9
     |
1274 |     def rstrip(self, chars: str | None = None, /) -> str: ...  # type: ignore[misc]
1275 |     @overload
1276 |     def split(self: LiteralString, sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString]:
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1277 |         """Return a list of the substrings in the string, using sep as the separator string.
     |
info: Possible overloads for bound method `split`:
info:   (self: LiteralString, sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString]
info:   (self, sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]
info: Union variant `Overload[(sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString], (sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]]` is incompatible with this call site
info: Attempted to call union type `Unknown | (bound method bytes.split(sep: Buffer | None = None, maxsplit: SupportsIndex = -1) -> list[bytes]) | (Overload[(sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString], (sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]])`
info: rule `no-matching-overload` is enabled by default
```

On main, we show both.


---

_Comment by @charliermarsh on 2026-01-18 20:05_

I think that's actually... _correct_? On main, we'd add an `CalledTopCallable` error because there's an `| Unknown`, but we now consider that a non-matching error?

---
