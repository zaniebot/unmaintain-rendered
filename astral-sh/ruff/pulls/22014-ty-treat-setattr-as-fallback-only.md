```yaml
number: 22014
title: "[ty] Treat `__setattr__` as fallback-only"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/set
created_at: 2025-12-17T02:40:35Z
updated_at: 2025-12-31T00:01:12Z
url: https://github.com/astral-sh/ruff/pull/22014
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Treat `__setattr__` as fallback-only

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1460.

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 02:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @charliermarsh on 2025-12-17 02:44_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 02:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
+ lib/spack/spack/vendor/typing_extensions.py:863:17: error[invalid-assignment] Object of type `def _no_init(self, *args, **kwargs) -> Unknown` is not assignable to attribute `__init__` of type `def __init__(cls, *args, **kwargs) -> Unknown`
- Found 4294 diagnostics
+ Found 4295 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/auth.py:205:9: error[unresolved-attribute] Cannot assign object of type `CallbackDict[str, str]` to attribute `_parameters` on type `Self@parameters` with custom `__setattr__` method.
- src/werkzeug/sansio/response.py:597:9: error[unresolved-attribute] Cannot assign object of type `def on_update(value: WWWAuthenticate) -> None` to attribute `_on_update` on type `WWWAuthenticate` with custom `__setattr__` method.
- src/werkzeug/sansio/response.py:620:13: error[invalid-assignment] Object of type `def on_update(value: WWWAuthenticate) -> None` is not assignable to attribute `_on_update` on type `WWWAuthenticate & ~AlwaysFalsy & ~Top[list[Unknown]]`
- Found 385 diagnostics
+ Found 382 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_extension_telnet.py:16:9: error[invalid-assignment] Object of type `<class 'dict'>` is not assignable to attribute `_get_telnet_vars` of type `def _get_telnet_vars(self) -> dict[str, Any]`
- Found 1789 diagnostics
+ Found 1790 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/cli/cmds/mark_for_deployment.py:849:17: error[invalid-assignment] Object of type `None` is not assignable to attribute `paasta_status_reminder_handle` of type `TimerHandle`
- Found 1106 diagnostics
+ Found 1107 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/contrib/upstream_pac.py:90:13: error[invalid-assignment] Object of type `tuple[str, tuple[str, int]]` is not assignable to attribute `via` of type `tuple[Literal["http", "https", "http3", "tls", "dtls", ... omitted 4 literals], tuple[str, int]] | None`
+ test/mitmproxy/addons/test_next_layer.py:374:17: error[invalid-assignment] Object of type `tuple[Literal["2001:db8::1"], Literal[443], Literal[0], Literal[0]] | tuple[Literal["192.0.2.1"], Literal[443]]` is not assignable to attribute `peername` of type `tuple[str, int] | None`
+ test/mitmproxy/addons/test_tlsconfig.py:439:17: error[invalid-assignment] Object of type `None` is not assignable to attribute `alpn_offers` of type `Sequence[bytes]`
+ test/mitmproxy/tools/console/test_flowview.py:92:5: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_content_view` of type `_lru_cache_wrapper[Unknown]`
- Found 2144 diagnostics
+ Found 2148 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

optuna (https://github.com/optuna/optuna)
+ optuna/storages/_rdb/storage.py:325:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_json` on type `StudyUserAttributeModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:337:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_json` on type `StudySystemAttributeModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:572:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `state` on type `TrialModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:574:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `TrialModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:668:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `state` on type `TrialModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:671:21: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_start` on type `TrialModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:674:21: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_complete` on type `TrialModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:699:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `value` on type `TrialValueModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:700:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_type` on type `TrialValueModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:738:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `intermediate_value` on type `TrialIntermediateValueModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:739:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `intermediate_value_type` on type `TrialIntermediateValueModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:805:17: error[invalid-assignment] Object of type `str` is not assignable to attribute `value_json` on type `TrialUserAttributeModel | TrialSystemAttributeModel`
+ optuna/storages/_rdb/storage.py:1035:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `heartbeat` on type `TrialHeartbeatModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:1193:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `schema_version` on type `VersionInfoModel` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:1194:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `library_version` on type `VersionInfoModel` with custom `__set__` method
+ tests/storages_tests/rdb_tests/test_storage.py:160:9: error[invalid-assignment] Object of type `Literal[11]` is not assignable to attribute `schema_version` on type `Unknown | VersionInfoModel`
+ tests/storages_tests/rdb_tests/test_storage.py:361:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `TrialModel` with custom `__set__` method
- Found 560 diagnostics
+ Found 577 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/_post_coinit/misc.py:378:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 396 diagnostics
+ Found 395 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_ssl.py:781:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 484 diagnostics
+ Found 483 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/core/dataarray.py:473:9: error[invalid-assignment] Object of type `(Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[tuple[Any, ...], dtype[Any]]] & Top[dict[Unknown, Unknown]]) | (Mapping[Unknown, Unknown] & Top[dict[Unknown, Unknown]]) | dict[Hashable, Variable | Unknown]` is not assignable to attribute `_coords` of type `dict[Any, Variable]`
- xarray/tests/test_dataarray.py:167:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_datatree.py:212:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1765 diagnostics
+ Found 1764 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/accounts/model.py:603:9: error[invalid-assignment] Cannot assign to read-only property `last_login` on object of type `Self@update_last_login`: Attempted assignment to `Self@update_last_login.last_login` here
- Found 1155 diagnostics
+ Found 1156 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/tracer/test_pin.py:76:13: error[invalid-assignment] Cannot assign to read-only property `service` on object of type `Pin`: Attempted assignment to `Pin.service` here
- Found 8424 diagnostics
+ Found 8425 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/common/tests/test_egraph.py:153:9: error[invalid-assignment] Object of type `Literal[2]` is not assignable to attribute `head` of type `type`
+ ibis/expr/rewrites.py:152:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `subs` of type `FrozenDict[Value[Unknown, Any], Field] | None`
+ ibis/expr/rewrites.py:153:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `ambigs` of type `FrozenDict[Value[Unknown, Any], tuple[Value[Unknown, Any], ...]] | None`
- Found 4608 diagnostics
+ Found 4611 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/parametertools.py:2679:9: error[unresolved-attribute] Cannot assign object of type `list[Unknown]` to attribute `_toy2values_unprotected` on type `Self@__init__` with custom `__setattr__` method.
- hydpy/core/parametertools.py:2696:13: error[unresolved-attribute] Cannot assign object of type `tuple[Literal[-1]]` to attribute `shape` on type `Self@__call__` with custom `__setattr__` method.
- hydpy/core/parametertools.py:2703:13: error[unresolved-attribute] Cannot assign object of type `list[Unknown | tuple[TOY, Unknown]]` to attribute `_toy2values_unprotected` on type `Self@__call__` with custom `__setattr__` method.
- hydpy/core/parametertools.py:2706:13: error[unresolved-attribute] Cannot assign object of type `list[Unknown]` to attribute `_toy2values_unprotected` on type `Self@__call__` with custom `__setattr__` method.
- hydpy/models/dam/dam_control.py:93:9: error[unresolved-attribute] Cannot assign object of type `tuple[Literal[-1]]` to attribute `shape` on type `Self@__call__` with custom `__setattr__` method.
- Found 684 diagnostics
+ Found 679 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/frame.py:3491:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:3543:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:3573:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/index_hierarchy.py:1145:13: error[invalid-assignment] Object of type `list[IndexBase | Unknown]` is not assignable to attribute `_indices` of type `list[Index[Any]]`
+ static_frame/core/index_hierarchy.py:1168:9: error[invalid-assignment] Object of type `list[IndexBase | Unknown]` is not assignable to attribute `_indices` of type `list[Index[Any]]`
- Found 1843 diagnostics
+ Found 1842 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/groupby/generic.py:2217:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3748 diagnostics
+ Found 3747 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_config.py:69:5: error[unresolved-attribute] Cannot assign object of type `float` to attribute `chop_threshold` on type `Display` with custom `__setattr__` method.
- Found 5027 diagnostics
+ Found 5026 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/airvisual/__init__.py:220:5: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `runtime_data` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 02:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 33 | 1 | 5 |
| `invalid-return-type` | 0 | 0 | 8 |
| `unresolved-attribute` | 0 | 8 | 0 |
| `unused-ignore-comment` | 0 | 8 | 0 |
| `invalid-argument-type` | 3 | 0 | 3 |
| **Total** | **36** | **17** | **16** |


**[Full report with detailed diff](https://0be1947d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://0be1947d.ty-ecosystem-ext.pages.dev/timing))



---

_Label `ty` added by @AlexWaygood on 2025-12-17 07:11_

---

_Marked ready for review by @charliermarsh on 2025-12-17 14:58_

---

_Review requested from @carljm by @charliermarsh on 2025-12-17 14:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-17 14:58_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-17 14:58_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-17 14:58_

---

_@carljm reviewed on 2025-12-17 22:15_

Behavior looks good here but I find the implementation surprising. Is it not possible to check for explicit attributes before trying to call `__setattr__` at all?

---

_Renamed from "Treat `__setattr__` as fallback-only" to "[ty] Treat `__setattr__` as fallback-only" by @charliermarsh on 2025-12-17 22:21_

---

_Comment by @charliermarsh on 2025-12-17 22:24_

I did it that way initially but it ended up being a fairly large change (by LOC), I'll try it again though.

---

_Comment by @carljm on 2025-12-17 22:26_

I'm not worried about a big change by LOC, I would care if it ended up being a lot more net code or more confusing code.

It just seems odd to check `__setattr__` at all if we might not care about the result.

---

_Comment by @charliermarsh on 2025-12-17 22:47_

Okay, reordered the operations.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:4687 on 2025-12-17 23:06_

Oops, the existence of this special case invalidates my previous comment. If we have to try `__setattr__` first regardless, then we should go back to your previous version where we just do it once. Sorry!

---

_@carljm reviewed on 2025-12-17 23:06_

---

_Review requested from @carljm by @charliermarsh on 2025-12-18 01:02_

---

_@charliermarsh reviewed on 2025-12-18 01:03_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/attributes.md`:2056 on 2025-12-18 01:03_

I _think_ this is the desired behavior.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 15:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:2056 on 2025-12-18 21:15_

I think it's somewhat arbitrary whether we prefer the "returns NoReturn" or the attribute type error here, but I agree this way is probably a bit better.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-18 21:43_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-18 21:43_

---

_Comment by @carljm on 2025-12-18 21:54_

We are now adding a lot more new diagnostics in the ecosystem than we were on a previous iteration of this PR. Looking into what changed...

---

_Comment by @charliermarsh on 2025-12-18 22:19_

(I will review.)

---

_Comment by @charliermarsh on 2025-12-18 22:28_

I think it's because we're now validating against `__set__` instead of `__setattr__`?

---

_@carljm approved on 2025-12-30 22:07_

Sorry, got midway through a review of this and then lost track of it in my notifications. Looks good to me! I spot-checked a number of the new diagnostics, and they all look correct (at least as far as this PR goes).

---

_Merged by @charliermarsh on 2025-12-31 00:01_

---

_Closed by @charliermarsh on 2025-12-31 00:01_

---

_Branch deleted on 2025-12-31 00:01_

---
