```yaml
number: 22590
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2026-01-15T00:39:51Z
updated_at: 2026-01-15T12:37:32Z
url: https://github.com/astral-sh/ruff/pull/22590
synced_at: 2026-01-15T12:53:10Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2026-01-15 00:39_

---

_Review requested from @carljm by @github-actions[bot] on 2026-01-15 00:39_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2026-01-15 00:39_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2026-01-15 00:39_

---

_Review requested from @sharkdp by @github-actions[bot] on 2026-01-15 00:39_

---

_Review requested from @dcreager by @github-actions[bot] on 2026-01-15 00:39_

---

_Closed by @AlexWaygood on 2026-01-15 05:05_

---

_Reopened by @AlexWaygood on 2026-01-15 05:05_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 05:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Typing conformance

No changes



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-15 05:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
+ src/pip/_vendor/distlib/util.py:1473:41: error[invalid-argument-type] Argument to bound method `load_cert_chain` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `str | bytes | PathLike[str] | PathLike[bytes] | None`
- src/pip/_vendor/distlib/util.py:1473:41: error[unresolved-attribute] Object of type `Self@connect` has no attribute `cert_file`
- src/pip/_vendor/distlib/util.py:1473:57: error[unresolved-attribute] Object of type `Self@connect` has no attribute `key_file`
+ src/pip/_vendor/distlib/util.py:1572:42: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/pip/_vendor/distlib/util.py:1572:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `str | Unknown`
- src/pip/_vendor/distlib/util.py:1572:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[str, int] | None`, found `str | Unknown`
- src/pip/_vendor/distlib/util.py:1572:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `str | Unknown`
- src/pip/_vendor/distlib/util.py:1572:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | None`, found `str | Unknown`
- src/pip/_vendor/distlib/util.py:1572:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `str | Unknown`
- Found 595 diagnostics
+ Found 590 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/oci/opener.py:432:61: warning[deprecated] The function `info` is deprecated: Deprecated since Python 3.9. Use `HTTPResponse.headers` attribute instead.
- Found 4340 diagnostics
+ Found 4341 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/claw/_importlib/_clawimpload.py:379:9: error[invalid-assignment] Object of type `def cache_from_source_beartype(...) -> str` is not assignable to attribute `cache_from_source` of type `def cache_from_source(path: str | PathLike[str], debug_override: bool | None = None, *, optimization: Any | None = None) -> str`
+ beartype/claw/_importlib/_clawimpload.py:379:9: error[invalid-assignment] Object of type `def cache_from_source_beartype(...) -> str` is not assignable to attribute `cache_from_source` of type `Overload[(path: str | PathLike[str], debug_override: bool, *, optimization: None = None) -> str, (path: str | PathLike[str], debug_override: None = None, *, optimization: Any | None = None) -> str]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/addons/save.py:101:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BinaryIO`, found `IO[Any]`
- Found 2146 diagnostics
+ Found 2145 diagnostics

vision (https://github.com/pytorch/vision)
- test/builtin_dataset_mocks.py:1245:18: error[parameter-already-assigned] Multiple values provided for parameter `cls` of function `__new__`
- test/builtin_dataset_mocks.py:1246:18: error[parameter-already-assigned] Multiple values provided for parameter `cls` of function `__new__`
- test/builtin_dataset_mocks.py:1247:18: error[parameter-already-assigned] Multiple values provided for parameter `cls` of function `__new__`
- test/test_datasets.py:2693:18: error[parameter-already-assigned] Multiple values provided for parameter `cls` of function `__new__`
- test/test_datasets.py:2694:18: error[parameter-already-assigned] Multiple values provided for parameter `cls` of function `__new__`
- test/test_datasets.py:2695:18: error[parameter-already-assigned] Multiple values provided for parameter `cls` of function `__new__`
- Found 1409 diagnostics
+ Found 1403 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_asyncgen.py:309:38: warning[possibly-missing-attribute] Attribute `f_locals` may be missing on object of type `FrameType | None`
+ src/trio/_core/_tests/test_asyncgen.py:313:38: warning[possibly-missing-attribute] Attribute `f_locals` may be missing on object of type `FrameType | None`
+ src/trio/_util.py:301:18: warning[possibly-missing-attribute] Attribute `f_globals` may be missing on object of type `FrameType | None`
- Found 486 diagnostics
+ Found 489 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
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
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1823 diagnostics
+ Found 1824 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14509 diagnostics
+ Found 14508 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Review requested from @Gankra by @AlexWaygood on 2026-01-15 12:33_

---

_Merged by @AlexWaygood on 2026-01-15 12:37_

---

_Closed by @AlexWaygood on 2026-01-15 12:37_

---

_Branch deleted on 2026-01-15 12:37_

---
