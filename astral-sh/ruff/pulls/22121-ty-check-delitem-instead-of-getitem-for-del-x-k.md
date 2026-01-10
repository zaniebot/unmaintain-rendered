```yaml
number: 22121
title: "[ty] Check `__delitem__` instead of `__getitem__` for `del x[k]`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/del
created_at: 2025-12-21T01:00:36Z
updated_at: 2025-12-24T03:34:22Z
url: https://github.com/astral-sh/ruff/pull/22121
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Check `__delitem__` instead of `__getitem__` for `del x[k]`

---

_Pull request opened by @charliermarsh on 2025-12-21 01:00_

## Summary

Previously, `del x[k]` incorrectly required the object to have a `__getitem__` method. This was wrong because deletion only needs `__delitem__`, which is independent of `__getitem__`.

Closes https://github.com/astral-sh/ty/issues/1799.


---

_Label `ty` added by @charliermarsh on 2025-12-21 01:00_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 01:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-21 01:02:12.586405320 +0000
+++ new-output.txt	2025-12-21 01:02:14.709411235 +0000
@@ -759,6 +759,7 @@
 namedtuples_usage.py:35:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:40:1: error[invalid-assignment] Cannot assign to read-only property `x` on object of type `Point`
 namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to a subscript on an object of type `Point`
+namedtuples_usage.py:43:5: error[non-subscriptable] Cannot subscript object of type `Point` with no `__delitem__` method
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Type `tuple[str, str]` does not match asserted type `tuple[str, ...]`
@@ -967,7 +968,8 @@
 typeddicts_extra_items.py:29:23: error[invalid-key] Unknown key "novel_adaptation" for TypedDict `Movie`: Unknown key "novel_adaptation"
 typeddicts_extra_items.py:39:54: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `InheritedMovie`: value of type `None`
 typeddicts_extra_items.py:43:5: error[invalid-key] Unknown key "other_extra_key" for TypedDict `InheritedMovie`: Unknown key "other_extra_key"
-typeddicts_extra_items.py:129:15: error[invalid-key] Unknown key "year" for TypedDict `MovieEI`: Unknown key "year"
+typeddicts_extra_items.py:128:9: error[invalid-argument-type] Method `__delitem__` of type `bound method MovieEI.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["name"]` on object of type `MovieEI`
+typeddicts_extra_items.py:129:9: error[invalid-argument-type] Method `__delitem__` of type `bound method MovieEI.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["year"]` on object of type `MovieEI`
 typeddicts_extra_items.py:254:63: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
 typeddicts_extra_items.py:255:63: error[invalid-key] Unknown key "description" for TypedDict `MovieExtraStr`: Unknown key "description"
 typeddicts_extra_items.py:266:64: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
@@ -989,7 +991,7 @@
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `Unknown`
 typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
 typeddicts_extra_items.py:342:27: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with a string literal key, got key of type `str`.
-typeddicts_extra_items.py:343:31: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with a string literal key, got key of type `str`
+typeddicts_extra_items.py:343:9: error[invalid-argument-type] Method `__delitem__` of type `bound method IntDictWithNum.__delitem__(k: Never) -> None` cannot be called with key of type `str` on object of type `IntDictWithNum`
 typeddicts_extra_items.py:352:25: error[invalid-assignment] Object of type `dict[str, int]` is not assignable to `IntDict`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`
@@ -1000,7 +1002,9 @@
 typeddicts_operations.py:32:36: error[invalid-key] Unknown key "other" for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:47:1: error[unresolved-attribute] Object of type `Movie` has no attribute `clear`
+typeddicts_operations.py:49:5: error[invalid-argument-type] Method `__delitem__` of type `bound method Movie.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["name"]` on object of type `Movie`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
+typeddicts_operations.py:64:5: error[invalid-argument-type] Method `__delitem__` of type `bound method MovieOptional.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["name"]` on object of type `MovieOptional`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
 typeddicts_readonly.py:51:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie1`: key is marked read-only
@@ -1033,4 +1037,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1035 diagnostics
+Found 1039 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-21 01:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/streams/stapled.py:122:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 101 diagnostics
+ Found 100 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/utils.py:111:17: error[non-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
+ src/werkzeug/utils.py:111:17: error[non-subscriptable] Cannot subscript object of type `object` with no `__delitem__` method

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/utilities/test_build_client_schema.py:738:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:775:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:814:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:837:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:930:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:951:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:972:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 430 diagnostics
+ Found 423 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/monkeypatch.py:306:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/monkeypatch.py:417:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 438 diagnostics
+ Found 436 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/configs/conventions.py:281:17: error[invalid-argument-type] Method `__delitem__` of type `bound method MetaEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["annotations"]` on object of type `MetaEssence`
+ kopf/_cogs/configs/conventions.py:283:17: error[invalid-argument-type] Method `__delitem__` of type `bound method MetaEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["labels"]` on object of type `MetaEssence`
+ kopf/_cogs/configs/conventions.py:285:17: error[invalid-argument-type] Method `__delitem__` of type `bound method BodyEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["metadata"]` on object of type `BodyEssence`
+ kopf/_cogs/configs/conventions.py:287:17: error[invalid-argument-type] Method `__delitem__` of type `bound method BodyEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["status"]` on object of type `BodyEssence`
- Found 263 diagnostics
+ Found 267 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/layers/http/test_http2.py:197:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ test/mitmproxy/proxy/layers/http/test_http2.py:197:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- test/mitmproxy/proxy/layers/http/test_http3.py:532:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ test/mitmproxy/proxy/layers/http/test_http3.py:532:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method

artigraph (https://github.com/artigraph/artigraph)
- tests/arti/internal/test_mappings.py:32:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 149 diagnostics
+ Found 148 diagnostics

vision (https://github.com/pytorch/vision)
+ torchvision/models/densenet.py:236:17: error[non-subscriptable] Cannot subscript object of type `Mapping[str, Any]` with no `__delitem__` method
- Found 1413 diagnostics
+ Found 1414 diagnostics

antidote (https://github.com/Finistere/antidote)
- tests/core/test_catalog_test.py:158:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:170:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:208:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:219:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:224:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:225:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:252:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:253:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:254:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:354:17: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:355:17: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:356:17: error[non-subscriptable] Cannot subscript object of type `CatalogOverrides` with no `__getitem__` method
- tests/core/test_catalog_test.py:464:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverride` with no `__getitem__` method
- tests/core/test_catalog_test.py:468:13: error[non-subscriptable] Cannot subscript object of type `CatalogOverride` with no `__getitem__` method
- Found 272 diagnostics
+ Found 258 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/clients.py:2668:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, ModuleType].__getitem__(key: str, /) -> ModuleType` cannot be called with key of type `str | Path` on object of type `dict[str, ModuleType]`
+ tanjun/clients.py:2668:17: error[invalid-argument-type] Method `__delitem__` of type `bound method dict[str, ModuleType].__delitem__(key: str, /) -> None` cannot be called with key of type `str | Path` on object of type `dict[str, ModuleType]`
- tanjun/clients.py:2668:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Path, ModuleType].__getitem__(key: Path, /) -> ModuleType` cannot be called with key of type `str | Path` on object of type `dict[Path, ModuleType]`
+ tanjun/clients.py:2668:17: error[invalid-argument-type] Method `__delitem__` of type `bound method dict[Path, ModuleType].__delitem__(key: Path, /) -> None` cannot be called with key of type `str | Path` on object of type `dict[Path, ModuleType]`
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/sources/test_cloudsigma.py:115:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["cloudinit"]` on object of type `str`
+ tests/unittests/sources/test_cloudsigma.py:115:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["cloudinit"]` on object of type `list[Unknown]`
- tests/unittests/sources/test_cloudsigma.py:115:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["cloudinit"]` on object of type `list[Unknown]`
- tests/unittests/sources/test_cloudsigma.py:115:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["cloudinit"]` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_cloudsigma.py:115:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["cloudinit"]` on object of type `list[Unknown | str]`
- tests/unittests/sources/test_cloudsigma.py:115:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/unittests/sources/test_cloudsigma.py:115:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__delitem__` method
+ tests/unittests/sources/test_cloudsigma.py:115:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
+ tests/unittests/test_ds_identify.py:1112:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["sys/class/dmi/id/product_name"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1112:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["sys/class/dmi/id/product_name"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1112:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["sys/class/dmi/id/product_name"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1112:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
- tests/unittests/test_ds_identify.py:1112:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["sys/class/dmi/id/product_name"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1112:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["sys/class/dmi/id/product_name"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1243:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1243:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1243:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1243:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
- tests/unittests/test_ds_identify.py:1243:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1243:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1251:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1251:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1251:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1251:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1251:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["/native/.zonecontrol/metadata.sock"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1251:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
+ tests/unittests/test_ds_identify.py:1285:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1285:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1285:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1285:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
- tests/unittests/test_ds_identify.py:1285:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1285:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1300:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1300:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1300:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1300:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
- tests/unittests/test_ds_identify.py:1300:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1300:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1315:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1315:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1315:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1315:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
- tests/unittests/test_ds_identify.py:1315:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1315:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1330:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | dict[Unknown | str, Unknown | str | int]].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1330:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1330:13: error[invalid-argument-type] Method `__delitem__` of type `bound method list[Unknown | str].__delitem__(key: SupportsIndex | slice[Any, Any, Any], /) -> None` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1330:13: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
- tests/unittests/test_ds_identify.py:1330:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[Unknown | str, Unknown | str | int], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[Unknown | str, Unknown | str | int]]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1330:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["usr/lib/vmware-tools/plugins/vmsvc/libdeployPkgPlugin.so"]` on object of type `list[Unknown | str]`

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/tests/test_adapter.py:70:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/zope/interface/tests/test_adapter.py:70:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method

apprise (https://github.com/caronc/apprise)
- apprise/manager.py:335:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ apprise/manager.py:335:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- apprise/manager.py:701:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ apprise/manager.py:701:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- apprise/manager.py:705:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ apprise/manager.py:705:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- apprise/persistent_store.py:882:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ apprise/persistent_store.py:882:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- apprise/persistent_store.py:919:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ apprise/persistent_store.py:919:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- apprise/persistent_store.py:1646:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ apprise/persistent_store.py:1646:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2603:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2603:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2630:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2630:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2631:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2631:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2655:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2655:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2699:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2699:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2700:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2700:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- tests/test_apprise_utils.py:2701:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_apprise_utils.py:2701:9: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/modules/rust.py:183:13: error[invalid-argument-type] Method `__delitem__` of type `bound method ExecutableKeywordArguments.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["rust_crate_type"]` on object of type `ExecutableKeywordArguments`
- unittests/cargotests.py:501:13: error[non-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
+ unittests/cargotests.py:501:13: error[non-subscriptable] Cannot subscript object of type `object` with no `__delitem__` method
- Found 1945 diagnostics
+ Found 1946 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/native/state.py:136:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[int | None, set[(StateBase, /) -> None]].__getitem__(key: int | None, /) -> set[(StateBase, /) -> None]` cannot be called with key of type `int | str | None | (Any & ~str)` on object of type `dict[int | None, set[(StateBase, /) -> None]]`
+ manticore/native/state.py:136:17: error[invalid-argument-type] Method `__delitem__` of type `bound method dict[int | None, set[(StateBase, /) -> None]].__delitem__(key: int | None, /) -> None` cannot be called with key of type `int | str | None | (Any & ~str)` on object of type `dict[int | None, set[(StateBase, /) -> None]]`

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/cwlprov/ro.py:669:25: error[invalid-argument-type] Method `__delitem__` of type `Overload[(index: int) -> None, (index: slice[Any, Any, Any]) -> None]` cannot be called with key of type `Literal["path"]` on object of type `MutableSequence[Divergent]`
+ cwltool/cwlprov/ro.py:669:25: error[invalid-argument-type] Method `__delitem__` of type `Overload[(index: int) -> None, (index: slice[Any, Any, Any]) -> None]` cannot be called with key of type `Literal["path"]` on object of type `MutableSequence[MutableMapping[str, None | int | str | ... omitted 3 union elements]]`
+ cwltool/cwlprov/ro.py:669:25: error[non-subscriptable] Cannot subscript object of type `int` with no `__delitem__` method
+ cwltool/cwlprov/ro.py:669:25: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
+ cwltool/cwlprov/ro.py:669:25: error[non-subscriptable] Cannot subscript object of type `float` with no `__delitem__` method
+ cwltool/cwlprov/ro.py:674:21: error[invalid-argument-type] Method `__delitem__` of type `Overload[(index: int) -> None, (index: slice[Any, Any, Any]) -> None]` cannot be called with key of type `Literal["location"]` on object of type `MutableSequence[Divergent]`
+ cwltool/cwlprov/ro.py:674:21: error[invalid-argument-type] Method `__delitem__` of type `Overload[(index: int) -> None, (index: slice[Any, Any, Any]) -> None]` cannot be called with key of type `Literal["location"]` on object of type `MutableSequence[MutableMapping[str, None | int | str | ... omitted 3 union elements]]`
+ cwltool/cwlprov/ro.py:674:21: error[non-subscriptable] Cannot subscript object of type `int` with no `__delitem__` method
+ cwltool/cwlprov/ro.py:674:21: error[non-subscriptable] Cannot subscript object of type `str` with no `__delitem__` method
+ cwltool/cwlprov/ro.py:674:21: error[non-subscriptable] Cannot subscript object of type `float` with no `__delitem__` method
- cwltool/main.py:527:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ cwltool/main.py:527:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- cwltool/main.py:529:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ cwltool/main.py:529:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- Found 245 diagnostics
+ Found 255 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/web_response.py:409:21: error[non-subscriptable] Cannot subscript object of type `MultiMapping[str]` with no `__delitem__` method
+ aiohttp/web_response.py:413:21: error[non-subscriptable] Cannot subscript object of type `MultiMapping[str]` with no `__delitem__` method
+ aiohttp/web_response.py:695:21: error[non-subscriptable] Cannot subscript object of type `MultiMapping[str]` with no `__delitem__` method
- Found 176 diagnostics
+ Found 179 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/tests/test_treenode.py:270:13: error[non-subscriptable] Cannot subscript object of type `TreeNode` with no `__getitem__` method
- xarray/tests/test_treenode.py:275:17: error[non-subscriptable] Cannot subscript object of type `TreeNode` with no `__getitem__` method
- xarray/tests/test_utils.py:144:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1771 diagnostics
+ Found 1768 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/sources.py:457:17: error[non-subscriptable] Cannot subscript object of type `Property[Any]` with no `__getitem__` method
+ src/bokeh/models/sources.py:457:17: error[non-subscriptable] Cannot subscript object of type `Property[Any]` with no `__delitem__` method

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2805 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Yarn[Any], generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1843 diagnostics
+ Found 1844 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/lib/message.py:244:21: error[invalid-argument-type] Method `__delitem__` of type `bound method FormattedEditHistoryEvent.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["prev_content"]` on object of type `FormattedEditHistoryEvent`
+ zerver/lib/message.py:245:21: error[invalid-argument-type] Method `__delitem__` of type `bound method FormattedEditHistoryEvent.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["prev_rendered_content"]` on object of type `FormattedEditHistoryEvent`
+ zerver/lib/message.py:246:21: error[invalid-argument-type] Method `__delitem__` of type `bound method FormattedEditHistoryEvent.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["content_html_diff"]` on object of type `FormattedEditHistoryEvent`
+ zerver/lib/users.py:637:13: error[invalid-argument-type] Method `__delitem__` of type `bound method APIUserDict.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["timezone"]` on object of type `APIUserDict`
- Found 3652 diagnostics
+ Found 3656 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/deconz/light.py:319:17: error[invalid-argument-type] Method `__delitem__` of type `bound method SetStateAttributes.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["on"]` on object of type `SetStateAttributes`
+ homeassistant/components/deconz/light.py:339:17: error[invalid-argument-type] Method `__delitem__` of type `bound method SetStateAttributes.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["on"]` on object of type `SetStateAttributes`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14423 diagnostics
+ Found 14426 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/optimize/_nonlin.py:753:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ scipy/optimize/_nonlin.py:753:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- scipy/optimize/_nonlin.py:754:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ scipy/optimize/_nonlin.py:754:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- scipy/optimize/_nonlin.py:764:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ scipy/optimize/_nonlin.py:764:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- scipy/optimize/_nonlin.py:765:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ scipy/optimize/_nonlin.py:765:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- scipy/optimize/_nonlin.py:829:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ scipy/optimize/_nonlin.py:829:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method
- scipy/optimize/_nonlin.py:830:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ scipy/optimize/_nonlin.py:830:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__delitem__` method


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~20MB
+     struct metadata = ~21MB


```

</details>




---

_Comment by @charliermarsh on 2025-12-21 01:06_

I think the conformance tests are correct except that they don't handle non-required keys for `TypedDict`, which I'm already working on as a follow-up PR.

---

_Marked ready for review by @charliermarsh on 2025-12-21 01:09_

---

_Review requested from @carljm by @charliermarsh on 2025-12-21 01:09_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-21 01:09_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-21 01:09_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-21 01:09_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-21 08:56_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 09:02_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `non-subscriptable` | 18 | 16 | 28 |
| `invalid-argument-type` | 17 | 8 | 20 |
| `unused-ignore-comment` | 0 | 12 | 0 |
| `invalid-return-type` | 0 | 2 | 5 |
| **Total** | **35** | **38** | **53** |


**[Full report with detailed diff](https://3ab034df.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://3ab034df.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/del.md`:235 on 2025-12-24 01:46_

This should probably say something like "Cannot delete subscript on object of type ..."?

---

_@carljm approved on 2025-12-24 01:53_

love it

---

_Merged by @charliermarsh on 2025-12-24 03:34_

---

_Closed by @charliermarsh on 2025-12-24 03:34_

---

_Branch deleted on 2025-12-24 03:34_

---
