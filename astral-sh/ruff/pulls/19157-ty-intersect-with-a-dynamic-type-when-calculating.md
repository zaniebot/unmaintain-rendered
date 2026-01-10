```yaml
number: 19157
title: "[ty] Intersect with a dynamic type when calculating the metaclass of a class if that class has a dynamic type in its MRO"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/dynamic-mro-metaclass
created_at: 2025-07-05T20:56:37Z
updated_at: 2025-07-10T08:34:36Z
url: https://github.com/astral-sh/ruff/pull/19157
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Intersect with a dynamic type when calculating the metaclass of a class if that class has a dynamic type in its MRO

---

_Pull request opened by @AlexWaygood on 2025-07-05 20:56_

## Summary

I believe this is necessary for fixing https://github.com/astral-sh/ty/issues/764, though it doesn't seem to be sufficient.

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-07-05 20:56_

---

_Comment by @github-actions[bot] on 2025-07-05 21:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
-     memo fields = ~49MB
+     memo fields = ~54MB

pyinstrument (https://github.com/joerick/pyinstrument)
- error[invalid-assignment] pyinstrument/vendor/decorator.py:295:5: Implicit shadowing of function `__init__`
- error[invalid-assignment] pyinstrument/vendor/decorator.py:301:5: Implicit shadowing of function `__init__`
+ error[invalid-assignment] pyinstrument/vendor/decorator.py:295:5: Object of type `def __init__(self, g, *a, **k) -> Unknown` is not assignable to attribute `__init__` of type `(Overload[(self, o: object, /) -> None, (self, name: str, bases: tuple[type, ...], dict: dict[str, Any], /, **kwds: Any) -> None]) & Unknown`
+ error[invalid-assignment] pyinstrument/vendor/decorator.py:301:5: Object of type `def __init__(self, g, *a, **k) -> Unknown` is not assignable to attribute `__init__` of type `(Overload[(self, o: object, /) -> None, (self, name: str, bases: tuple[type, ...], dict: dict[str, Any], /, **kwds: Any) -> None]) & Unknown`

Expression (https://github.com/cognitedata/Expression)
+ error[non-subscriptable] expression/core/fn.py:26:29: Cannot subscript object of type `<class 'TailCall'>` with no `__getitem__` method
- Found 232 diagnostics
+ Found 233 diagnostics

kornia (https://github.com/kornia/kornia)
-     struct metadata = ~6MB
+     struct metadata = ~7MB
-     memo metadata = ~23MB
+     memo metadata = ~25MB

dulwich (https://github.com/dulwich/dulwich)
-     memo metadata = ~15MB
+     memo metadata = ~17MB

discord.py (https://github.com/Rapptz/discord.py)
+ error[non-subscriptable] discord/app_commands/errors.py:60:26: Cannot subscript object of type `<class 'Command'>` with no `__getitem__` method
+ error[non-subscriptable] discord/ext/commands/core.py:1586:31: Cannot subscript object of type `<class 'Command'>` with no `__getitem__` method
+ error[non-subscriptable] discord/ext/commands/hybrid.py:305:24: Cannot subscript object of type `<class 'Command'>` with no `__getitem__` method
+ error[non-subscriptable] discord/ext/commands/hybrid.py:481:21: Cannot subscript object of type `<class 'Command'>` with no `__getitem__` method
- Found 548 diagnostics
+ Found 552 diagnostics
-     memo fields = ~189MB
+     memo fields = ~207MB

pydantic (https://github.com/pydantic/pydantic)
+ error[non-subscriptable] pydantic/deprecated/class_validators.py:58:50: Cannot subscript object of type `<class 'classmethod'>` with no `__getitem__` method
+ error[non-subscriptable] pydantic/deprecated/class_validators.py:58:78: Cannot subscript object of type `<class 'staticmethod'>` with no `__getitem__` method
+ error[non-subscriptable] pydantic/deprecated/copy_internals.py:20:22: Cannot subscript object of type `<class 'classmethod'>` with no `__getitem__` method
+ error[non-subscriptable] pydantic/functional_validators.py:366:50: Cannot subscript object of type `<class 'classmethod'>` with no `__getitem__` method
+ error[non-subscriptable] pydantic/functional_validators.py:366:78: Cannot subscript object of type `<class 'staticmethod'>` with no `__getitem__` method
+ error[non-subscriptable] pydantic/v1/typing.py:284:26: Cannot subscript object of type `<class 'classmethod'>` with no `__getitem__` method
+ error[non-subscriptable] pydantic/v1/typing.py:287:26: Cannot subscript object of type `<class 'classmethod'>` with no `__getitem__` method
- Found 765 diagnostics
+ Found 772 diagnostics

optuna (https://github.com/optuna/optuna)
- error[invalid-argument-type] optuna/storages/_rdb/alembic/versions/v1.3.0.a.py:65:38: Argument to bound method `bulk_update_mappings` is incorrect: Expected `Mapper[Any]`, found `<class 'TrialModel'>`
- error[invalid-argument-type] optuna/storages/_rdb/alembic/versions/v3.0.0.c.py:140:38: Argument to bound method `bulk_update_mappings` is incorrect: Expected `Mapper[Any]`, found `<class 'IntermediateValueModel'>`
- error[invalid-argument-type] optuna/storages/_rdb/alembic/versions/v3.0.0.c.py:180:38: Argument to bound method `bulk_update_mappings` is incorrect: Expected `Mapper[Any]`, found `<class 'IntermediateValueModel'>`
- error[invalid-argument-type] optuna/storages/_rdb/alembic/versions/v3.0.0.d.py:145:38: Argument to bound method `bulk_update_mappings` is incorrect: Expected `Mapper[Any]`, found `<class 'TrialValueModel'>`
- error[invalid-argument-type] optuna/storages/_rdb/alembic/versions/v3.0.0.d.py:177:38: Argument to bound method `bulk_update_mappings` is incorrect: Expected `Mapper[Any]`, found `<class 'TrialValueModel'>`
- Found 580 diagnostics
+ Found 575 diagnostics

asynq (https://github.com/quora/asynq)
+ error[non-subscriptable] asynq/decorators.pyi:78:22: Cannot subscript object of type `<class 'PureAsyncDecorator'>` with no `__getitem__` method
+ error[non-subscriptable] asynq/decorators.pyi:101:39: Cannot subscript object of type `<class 'AsyncDecoratorBinder'>` with no `__getitem__` method
+ error[non-subscriptable] asynq/decorators.pyi:103:33: Cannot subscript object of type `<class 'AsyncDecorator'>` with no `__getitem__` method
+ error[non-subscriptable] asynq/decorators.pyi:115:27: Cannot subscript object of type `<class 'AsyncDecorator'>` with no `__getitem__` method
+ error[non-subscriptable] asynq/decorators.pyi:122:38: Cannot subscript object of type `<class 'AsyncProxyDecorator'>` with no `__getitem__` method
- Found 188 diagnostics
+ Found 193 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
-     memo fields = ~72MB
+     memo fields = ~80MB

pwndbg (https://github.com/pwndbg/pwndbg)
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:297:23: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `int`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:300:18: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `Literal[254]`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:359:23: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `int`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:362:18: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `Literal[254]`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:427:23: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `int`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:430:18: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `Literal[254]`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:546:21: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `Literal[64]`
- error[unsupported-operator] pwndbg/aglib/heap/structs.py:565:21: Operator `*` is unsupported between objects of type `<class 'c_pvoid'>` and `Literal[64]`
- Found 2263 diagnostics
+ Found 2255 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ error[non-subscriptable] src/bandersnatch/simple.py:64:16: Cannot subscript object of type `<class 'SimpleDigest'>` with no `__getitem__` method
- Found 128 diagnostics
+ Found 129 diagnostics

scrapy (https://github.com/scrapy/scrapy)
-     memo fields = ~207MB
+     memo fields = ~189MB

django-stubs (https://github.com/typeddjango/django-stubs)
-     struct metadata = ~6MB
+     struct metadata = ~7MB

static-frame (https://github.com/static-frame/static-frame)
+ error[non-subscriptable] static_frame/core/batch.py:80:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/container.py:42:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/container_util.py:91:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/frame.py:10005:15: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/frame.py:10151:15: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/frame.py:10525:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/frame.py:10526:15: Cannot subscript object of type `<class 'FrameHE'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/frame.py:10527:15: Cannot subscript object of type `<class 'FrameGO'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/generic_aliases.py:12:22: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/generic_aliases.py:19:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/generic_aliases.py:20:15: Cannot subscript object of type `<class 'FrameGO'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/generic_aliases.py:21:15: Cannot subscript object of type `<class 'FrameHE'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/index_hierarchy.py:3084:24: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/interface.py:106:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/interface_meta.py:10:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/memory_measure.py:18:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/node_fill_value.py:35:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/node_iter.py:45:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/node_selector.py:56:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/node_transpose.py:21:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/pivot.py:40:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/protocol_dfi.py:25:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/quilt.py:85:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/store.py:28:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/store_config.py:16:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/store_sqlite.py:23:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/store_xlsx.py:51:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/store_zip.py:29:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/type_clinic.py:28:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/core/yarn.py:91:15: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/test_case.py:33:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:172:20: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:204:21: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:206:21: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:209:21: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:251:21: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:253:21: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:256:21: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_frame.py:263:23: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_index.py:7:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_index_hierarchy.py:11:11: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_index_hierarchy.py:14:11: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_index_hierarchy.py:29:11: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_index_hierarchy.py:30:11: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/typing/test_index_hierarchy.py:32:11: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1081:9: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1086:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1095:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1096:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1108:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1109:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1113:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1121:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1161:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1181:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1190:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1200:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1238:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1269:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1289:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1309:10: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1333:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1355:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1372:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1404:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1456:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1534:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1589:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1628:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1662:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1709:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1735:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:1991:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2026:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2154:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2158:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2170:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2182:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2200:13: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2647:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2662:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2703:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2705:9: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2741:12: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2758:17: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2877:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2901:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2921:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit/test_type_clinic.py:2946:10: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit_forward/test_type_clinic.py:19:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit_forward/test_type_clinic.py:28:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit_forward/test_type_clinic.py:37:9: Cannot subscript object of type `<class 'Frame'>` with no `__getitem__` method
+ error[non-subscriptable] static_frame/test/unit_forward/test_type_clinic.py:38:13: Cannot subscript object of type `<class 'IndexHierarchy'>` with no `__getitem__` method
- Found 1803 diagnostics
+ Found 1896 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~276MB
+     memo fields = ~304MB

sphinx (https://github.com/sphinx-doc/sphinx)
+ error[non-subscriptable] sphinx/util/inspect.py:61:30: Cannot subscript object of type `<class 'staticmethod'>` with no `__getitem__` method
+ error[non-subscriptable] sphinx/util/inspect.py:61:55: Cannot subscript object of type `<class 'classmethod'>` with no `__getitem__` method
- Found 607 diagnostics
+ Found 609 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- warning[possibly-unbound-attribute] openlibrary/accounts/model.py:940:9: Attribute `save_s3_keys` on type `OpenLibraryAccount | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/accounts/model.py:950:33: Attribute `generate_login_code` on type `OpenLibraryAccount | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/accounts/model.py:951:5: Attribute `update_last_login` on type `OpenLibraryAccount | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/accounts/model.py:956:21: Attribute `email` on type `OpenLibraryAccount | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/accounts/model.py:958:24: Attribute `username` on type `OpenLibraryAccount | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/accounts/model.py:959:17: Attribute `itemname` on type `OpenLibraryAccount | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/lending.py:634:26: Attribute `username` on type `None | OpenLibraryAccount` is possibly unbound
+ warning[possibly-unbound-attribute] openlibrary/core/lending.py:634:26: Attribute `username` on type `None | @Todo(Type::Intersection.call())` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/lending.py:635:26: Attribute `itemname` on type `None | OpenLibraryAccount` is possibly unbound
+ warning[possibly-unbound-attribute] openlibrary/core/lending.py:635:26: Attribute `itemname` on type `None | @Todo(Type::Intersection.call())` is possibly unbound
- error[invalid-argument-type] openlibrary/core/lending.py:641:42: Argument to function `_get_ia_loan` is incorrect: Expected `str`, found `None | (OpenLibraryAccount & ~AlwaysTruthy) | Unknown`
+ error[invalid-argument-type] openlibrary/core/lending.py:641:42: Argument to function `_get_ia_loan` is incorrect: Expected `str`, found `None | (@Todo(Type::Intersection.call()) & ~AlwaysTruthy) | Unknown`
- error[invalid-argument-type] openlibrary/core/lending.py:646:42: Argument to function `_get_ia_loan` is incorrect: Expected `str`, found `None | (OpenLibraryAccount & ~AlwaysTruthy) | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[invalid-argument-type] openlibrary/core/lending.py:646:42: Argument to function `_get_ia_loan` is incorrect: Expected `str`, found `None | (@Todo(Type::Intersection.call()) & ~AlwaysTruthy) | @Todo(map_with_boundness: intersections with negative contributions)`
- warning[possibly-unbound-attribute] openlibrary/core/lending.py:669:66: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/lending.py:696:20: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/lending.py:955:16: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/lending.py:956:59: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/waitinglist.py:52:24: Attribute `username` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/waitinglist.py:119:24: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/waitinglist.py:131:24: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/waitinglist.py:180:8: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/core/waitinglist.py:181:50: Attribute `itemname` on type `OpenLibraryAccount | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/plugins/upstream/borrow.py:173:46: Attribute `_key` on type `OpenLibraryAccount | None` is possibly unbound
- Found 698 diagnostics
+ Found 682 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ error[non-subscriptable] lib/streamlit/elements/widgets/button_group.py:403:17: Cannot subscript object of type `<class '_SingleSelectSerde'>` with no `__getitem__` method
+ error[non-subscriptable] lib/streamlit/elements/widgets/button_group.py:918:38: Cannot subscript object of type `<class 'ButtonGroupSerde'>` with no `__getitem__` method
- Found 3301 diagnostics
+ Found 3303 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[non-subscriptable] src/bokeh/_specs.pyi:72:49: Cannot subscript object of type `UnionType` with no `__getitem__` method
- Found 857 diagnostics
+ Found 856 diagnostics
-     struct metadata = ~9MB
+     struct metadata = ~10MB
-     struct fields = ~13MB
+     struct fields = ~14MB

prefect (https://github.com/PrefectHQ/prefect)
+ error[non-subscriptable] src/prefect/flow_engine.py:230:21: Cannot subscript object of type `<class 'BaseFlowRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flow_engine.py:790:26: Cannot subscript object of type `<class 'BaseFlowRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flow_engine.py:1362:14: Cannot subscript object of type `<class 'FlowRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flow_engine.py:1386:14: Cannot subscript object of type `<class 'AsyncFlowRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flow_engine.py:1413:14: Cannot subscript object of type `<class 'FlowRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flow_engine.py:1454:14: Cannot subscript object of type `<class 'AsyncFlowRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flows.py:2026:31: Cannot subscript object of type `<class 'Flow'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/flows.py:2249:11: Cannot subscript object of type `<class 'InfrastructureBoundFlow'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/runner/submit.py:44:31: Cannot subscript object of type `<class 'Flow'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/runner/submit.py:44:47: Cannot subscript object of type `<class 'Task'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/server/database/dependencies.py:256:5: Cannot subscript object of type `<class '_FuncWrapper'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/server/database/dependencies.py:325:25: Cannot subscript object of type `<class '_FuncWrapper'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/task_engine.py:308:25: Cannot subscript object of type `<class 'BaseTaskRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/task_engine.py:880:26: Cannot subscript object of type `<class 'BaseTaskRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/task_engine.py:1471:14: Cannot subscript object of type `<class 'SyncTaskRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/task_engine.py:1502:14: Cannot subscript object of type `<class 'AsyncTaskRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/task_engine.py:1536:14: Cannot subscript object of type `<class 'SyncTaskRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/task_engine.py:1594:14: Cannot subscript object of type `<class 'AsyncTaskRunEngine'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/tasks.py:2084:25: Cannot subscript object of type `<class 'Task'>` with no `__getitem__` method
+ error[non-subscriptable] src/prefect/workers/base.py:824:27: Cannot subscript object of type `<class 'Task'>` with no `__getitem__` method
- Found 3752 diagnostics
+ Found 3772 diagnostics

```
</details>


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 07:06_

---

_Comment by @github-actions[bot] on 2025-07-10 07:15_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `non-subscriptable` | 142 | 1 | 0 |
| `possibly-unbound-attribute` | 0 | 16 | 2 |
| `unsupported-operator` | 0 | 8 | 0 |
| `invalid-argument-type` | 0 | 5 | 2 |
| `invalid-assignment` | 0 | 0 | 2 |
| **Total** | **142** | **30** | **6** |

**[Full report with detailed diff](https://alex-dynamic-mro-metaclass.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-10 08:26_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 08:26_

---
