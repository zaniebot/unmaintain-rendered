```yaml
number: 22067
title: "[ty] Add support for descriptor checks with unions"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/descriptor-union
created_at: 2025-12-19T02:44:13Z
updated_at: 2025-12-31T00:16:18Z
url: https://github.com/astral-sh/ruff/pull/22067
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Add support for descriptor checks with unions

---

_@charliermarsh_

## Summary

This PR attempts to support cases like the following ([playground](https://play.ty.dev/cf79b56e-4350-49e2-ad3f-795e68613401)):

```python
from typing import Any

class DescriptorWithSet:
    def __set__(self, instance: object, value: Any) -> None: ...

class NoSet:
    pass

class MyModel:
    state: DescriptorWithSet | NoSet

m = MyModel()
m.state = 1
```

At present, `m.state = 1` fails with `Invalid assignment to data descriptor attribute state on type MyModel with custom __set__ method (invalid-assignment)`, because we attempt to reconcile the `DescriptorWithSet | NoSet` against the `__set__` call. With this change, we instead partition the union into those members that implement `__set__` and those that don't.


---

_Comment by @astral-sh-bot[bot] on 2025-12-19 02:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 02:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_orderedbase.py:71:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Self@__init__` with custom `__set__` method
- bidict/_orderedbase.py:82:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbase.py:110:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:86:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:87:24: error[invalid-assignment] Object of type `Node` is not assignable to attribute `nxt` on type `Unknown | Node`
- bidict/_orderedbidict.py:91:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- Found 15 diagnostics
+ Found 9 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_pqueues.py:101:9: error[invalid-assignment] Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
+ tests/test_scheduler.py:98:9: error[invalid-assignment] Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
- Found 1790 diagnostics
+ Found 1792 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/test_terminal.py:2029:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 433 diagnostics
+ Found 432 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/storages/_rdb/storage.py:325:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_json` on type `StudyUserAttributeModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:337:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_json` on type `StudySystemAttributeModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:572:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `state` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:574:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:668:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `state` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:671:21: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_start` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:674:21: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_complete` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:699:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `value` on type `TrialValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:700:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_type` on type `TrialValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:738:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `intermediate_value` on type `TrialIntermediateValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:739:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `intermediate_value_type` on type `TrialIntermediateValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:805:17: error[invalid-assignment] Object of type `str` is not assignable to attribute `value_json` on type `TrialUserAttributeModel | TrialSystemAttributeModel`
- optuna/storages/_rdb/storage.py:1035:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `heartbeat` on type `TrialHeartbeatModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:1193:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `schema_version` on type `VersionInfoModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:1194:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `library_version` on type `VersionInfoModel` with custom `__set__` method
- tests/storages_tests/rdb_tests/test_storage.py:160:9: error[invalid-assignment] Object of type `Literal[11]` is not assignable to attribute `schema_version` on type `Unknown | VersionInfoModel`
- tests/storages_tests/rdb_tests/test_storage.py:361:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `TrialModel` with custom `__set__` method
- Found 577 diagnostics
+ Found 560 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/interpreter.py:1186:9: error[invalid-assignment] Object of type `list[str]` is not assignable to attribute `project_default_options` of type `dict[OptionKey, str | int | list[str]]`
- Found 1944 diagnostics
+ Found 1945 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ tests/unittests/sources/test_azure.py:2849:9: error[invalid-assignment] Object of type `Literal["md"]` is not assignable to attribute `metadata` of type `dict[Unknown, Unknown]`
- Found 1188 diagnostics
+ Found 1189 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2805 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1228:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5125 diagnostics
+ Found 5126 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/wallbox/coordinator.py:244:9: error[invalid-assignment] Object of type `timedelta` is not assignable to attribute `update_interval` of type `property`
- Found 14440 diagnostics
+ Found 14441 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "Add support for descriptor checks with unions" to "[ty] Add support for descriptor checks with unions" by @AlexWaygood on 2025-12-19 14:07_

---

_Label `ty` added by @AlexWaygood on 2025-12-19 14:07_

---

_Marked ready for review by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-19 14:15_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-19 14:19_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 14:24_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 5 | 23 | 0 |
| `invalid-argument-type` | 0 | 3 | 1 |
| `invalid-return-type` | 1 | 0 | 3 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **6** | **27** | **4** |


**[Full report with detailed diff](https://a4bfcb31.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://a4bfcb31.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @carljm on 2025-12-24 01:37_

Where did this case come from? Was it user-reported (there's no issue linked)? Did we see it in the ecosystem somewhere?

Just curious since it's kind of an odd case.

---

_Comment by @charliermarsh on 2025-12-24 04:05_

@carljm -- I believe this was an attempt to fix some of the new diagnostics that fell out of https://github.com/astral-sh/ruff/pull/22014, specifically the new `optuna` diagnostics in the ecosystem report (https://charlie-set.ecosystem-663.pages.dev/diff).

---
