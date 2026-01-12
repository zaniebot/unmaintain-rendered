```yaml
number: 22091
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
created_at: 2025-12-19T18:04:28Z
updated_at: 2025-12-19T18:23:11Z
url: https://github.com/astral-sh/ruff/pull/22091
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2025-12-19 18:04_

---

_Review requested from @carljm by @github-actions[bot] on 2025-12-19 18:04_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-12-19 18:04_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-12-19 18:04_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-12-19 18:04_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-12-19 18:04_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-19 18:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_bidict.py:126:9: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableMapping.pop`
- Found 15 diagnostics
+ Found 14 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/http/headers.py:76:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- scrapy/utils/datatypes.py:82:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- Found 1794 diagnostics
+ Found 1792 diagnostics

starlette (https://github.com/encode/starlette)
+ tests/test_datastructures.py:171:12: error[no-matching-overload] No overload of bound method `get` matches arguments
+ tests/test_datastructures.py:287:12: error[no-matching-overload] No overload of bound method `get` matches arguments
+ tests/test_datastructures.py:371:12: error[no-matching-overload] No overload of bound method `get` matches arguments
+ tests/test_datastructures.py:406:12: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 210 diagnostics
+ Found 214 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/_contextvars.pyi:57:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- mypy/typeshed/stdlib/builtins.pyi:1129:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- mypy/typeshed/stdlib/builtins.pyi:1135:9: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableMapping.pop`
- mypy/typeshed/stdlib/configparser.pyi:366:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- mypy/typeshed/stdlib/multiprocessing/managers.pyi:115:13: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- mypy/typeshed/stdlib/multiprocessing/managers.pyi:121:13: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableMapping.pop`
+ mypy/typeshed/stdlib/types.pyi:328:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1785 diagnostics
+ Found 1780 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/mesondata.py:28:24: error[unsupported-operator] Operator `/` is not supported between objects of type `Traversable` and `Unknown | PurePosixPath`
- Found 1946 diagnostics
+ Found 1945 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

AutoSplit (https://github.com/Toufool/AutoSplit)
- src/capture_method/__init__.py:108:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- Found 70 diagnostics
+ Found 69 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/http/request.pyi:205:9: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableMapping.pop`
- Found 444 diagnostics
+ Found 443 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/common/tests/test_egraph.py:59:12: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 4574 diagnostics
+ Found 4575 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5085 diagnostics
+ Found 5086 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14425 diagnostics
+ Found 14426 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-12-19 18:23_

---

_Closed by @AlexWaygood on 2025-12-19 18:23_

---

_Branch deleted on 2025-12-19 18:23_

---
