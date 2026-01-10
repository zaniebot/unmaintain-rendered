```yaml
number: 17589
title: "[red-knot] Stricter parsing for `type[]` type expressions"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/protocol-type-expression
created_at: 2025-04-23T17:43:48Z
updated_at: 2026-01-07T13:55:47Z
url: https://github.com/astral-sh/ruff/pull/17589
synced_at: 2026-01-10T16:30:32Z
```

# [red-knot] Stricter parsing for `type[]` type expressions

---

_Pull request opened by @AlexWaygood on 2025-04-23 17:43_

## Summary

This PR:
- Adds stricter parsing of `type[]` type expressions, so that invalid type forms such as `type[Protocol]` and `type[Callable]` are flagged
- Adds a helpful subdiagnostic to some `invalid-type-form` diagnostics that links to the typing spec grammar for type expressions and annotation expressions

## Test Plan

Mdtests extended and updated


---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 17:43_

---

_Comment by @github-actions[bot] on 2025-04-23 17:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- error[invalid-return-type] bidict/_base.py:519:16: Return type does not match returned value: expected `BT`, found `BidictBase[Any, Any]`
+ error[unresolved-attribute] bidict/_base.py:140:16: Type `BT'meta` has no attribute `_inv_cls_dict_diff`
+ error[call-non-callable] bidict/_base.py:489:16: Object of type `BT'meta` is not callable
+ error[call-non-callable] bidict/_base.py:517:15: Object of type `BT'meta` is not callable
- Found 15 diagnostics
+ Found 17 diagnostics

dacite (https://github.com/konradhalas/dacite)
+ error[call-non-callable] dacite/core.py:87:16: Object of type `T'meta` is not callable
- Found 24 diagnostics
+ Found 25 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ error[call-non-callable] aioredis/connection.py:1302:16: Object of type `_CP'meta` is not callable
- Found 28 diagnostics
+ Found 29 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[call-non-callable] chess/__init__.py:1528:16: Object of type `BaseBoardT'meta` is not callable
+ error[unresolved-attribute] chess/__init__.py:1540:17: Type `BaseBoardT'meta` has no attribute `empty`
+ error[call-non-callable] chess/__init__.py:3910:16: Object of type `BoardT'meta` is not callable
+ error[unresolved-attribute] chess/__init__.py:3920:17: Type `BoardT'meta` has no attribute `empty`
+ error[unresolved-attribute] chess/__init__.py:3925:17: Type `BoardT'meta` has no attribute `empty`
+ error[invalid-argument-type] chess/engine.py:1171:65: Argument to bound method `subprocess_exec` is incorrect: Argument type `() -> _ProtocolT` does not satisfy upper bound of type variable `ProtocolT'meta`
+ error[call-non-callable] chess/pgn.py:940:16: Object of type `GameT'meta` is not callable
+ error[call-non-callable] chess/pgn.py:954:16: Object of type `GameT'meta` is not callable
- Found 37 diagnostics
+ Found 45 diagnostics

beartype (https://github.com/beartype/beartype)
- warning[unused-ignore-comment] beartype/_util/cache/utilcachemeta.py:79:41: Unused blanket `type: ignore` directive
- error[invalid-return-type] beartype/_util/utilobjmake.py:194:12: Return type does not match returned value: expected `T`, found `object`
+ error[call-non-callable] beartype/_util/utilobjmake.py:191:23: Object of type `T'meta` is not callable
+ error[invalid-super-argument] beartype/typing/_typingpep544.py:192:15: `_TT'meta` is not an instance or subclass of `<class '_CachingProtocolMeta'>` in `super(<class '_CachingProtocolMeta'>, _TT'meta)` call

python-htmlgen (https://github.com/srittau/python-htmlgen)
- warning[unused-ignore-comment] test_htmlgen/attribute.py:271:55: Unused blanket `type: ignore` directive
- Found 26 diagnostics
+ Found 25 diagnostics

kornia (https://github.com/kornia/kornia)
+ error[call-non-callable] kornia/constants.py:56:16: Object of type `T'meta` is not callable
- error[call-non-callable] kornia/contrib/models/efficient_vit/nn/act.py:43:19: Method `__getitem__` of type `bound method dict[str, @Todo(unsupported type[X] special form)].__getitem__(key: str, /) -> @Todo(unsupported type[X] special form)` is not callable on object of type `dict[str, @Todo(unsupported type[X] special form)]`
+ error[call-non-callable] kornia/contrib/models/efficient_vit/nn/act.py:43:19: Method `__getitem__` of type `bound method dict[str, type[Unknown]].__getitem__(key: str, /) -> type[Unknown]` is not callable on object of type `dict[str, type[Unknown]]`
- warning[unused-ignore-comment] kornia/utils/helpers.py:387:48: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] kornia/utils/helpers.py:379:67: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `T'meta`
- Found 969 diagnostics
+ Found 970 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ warning[redundant-cast] koda_validate/dataclasses.py:117:25: Value is already of type `_DCT'meta`
- Found 50 diagnostics
+ Found 51 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ error[invalid-argument-type] strawberry/experimental/pydantic/error_type.py:118:73: Argument to function `_wrap_dataclass` is incorrect: Argument type `type` does not satisfy upper bound of type variable `T'meta`
+ error[call-non-callable] strawberry/experimental/pydantic/object_type.py:298:20: Object of type `PydanticModel'meta` is not callable
- warning[unused-ignore-comment] strawberry/federation/schema_directive.py:41:37: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] strawberry/schema_directive.py:56:37: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] strawberry/types/object_type.py:300:51: Argument to function `_inject_default_for_maybe_annotations` is incorrect: Argument type `T` does not satisfy upper bound of type variable `T'meta`
+ error[invalid-argument-type] strawberry/types/object_type.py:301:35: Argument to function `_wrap_dataclass` is incorrect: Argument type `T` does not satisfy upper bound of type variable `T'meta`
- Found 436 diagnostics
+ Found 438 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ error[call-non-callable] src/aiortc/codecs/h264.py:66:19: Object of type `DESCRIPTOR_T'meta` is not callable
+ error[call-non-callable] src/aiortc/codecs/h264.py:79:19: Object of type `DESCRIPTOR_T'meta` is not callable
+ error[call-non-callable] src/aiortc/codecs/h264.py:100:19: Object of type `DESCRIPTOR_T'meta` is not callable
+ error[call-non-callable] src/aiortc/codecs/vpx.py:143:15: Object of type `DESCRIPTOR_T'meta` is not callable
+ error[call-non-callable] src/aiortc/rtcdtlstransport.py:192:16: Object of type `CERTIFICATE_T'meta` is not callable
- Found 129 diagnostics
+ Found 134 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-return-type] src/trio/_path.py:67:16: Return type does not match returned value: expected `PathT`, found `Path`
+ error[call-non-callable] src/trio/_path.py:67:16: Object of type `PathT'meta` is not callable
+ error[no-matching-overload] src/trio/_path.py:77:16: No overload of function `__new__` matches arguments
- warning[unused-ignore-comment] src/trio/_tests/test_testing_raisesgroup.py:997:26: Unused blanket `type: ignore` directive
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:17:5: Object of type `Unknown | (@Todo(unsupported type[X] special form) & None) | None | (@Todo(unsupported type[X] special form) & type[BaseException])` is not assignable to `type[BaseException]`
+ error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:17:5: Object of type `Unknown | None | MatchE'meta` is not assignable to `type[BaseException]`
- warning[unused-ignore-comment] src/trio/_util.py:289:65: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_util.py:327:51: Unused blanket `type: ignore` directive
- error[invalid-argument-type] src/trio/testing/_raises_group.py:593:25: Argument to function `issubclass` is incorrect: Expected `type`, found `@Todo(unsupported type[X] special form) | (@Todo(unsupported type[X] special form) & None) | (@Todo(unsupported type[X] special form) & type[BaseException]) | None`
+ error[invalid-argument-type] src/trio/testing/_raises_group.py:593:25: Argument to function `issubclass` is incorrect: Expected `type`, found `Unknown | None | MatchE'meta`
- Found 1092 diagnostics
+ Found 1090 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ error[call-non-callable] porcupine/settings.py:612:21: Object of type `_StrOrInt'meta` is not callable
+ error[call-non-callable] porcupine/tabs.py:1013:15: Object of type `_FileTabT'meta` is not callable
+ error[unresolved-attribute] porcupine/utils.py:429:44: Type `_T'meta` has no attribute `__name__`
+ error[unresolved-attribute] porcupine/utils.py:430:70: Type `_T'meta` has no attribute `__name__`
- Found 81 diagnostics
+ Found 85 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ error[invalid-return-type] pydantic/dataclasses.py:319:12: Return type does not match returned value: expected `((_T'meta, /) -> type[PydanticDataclass]) | type[PydanticDataclass]`, found `(def create_dataclass(cls: type[Any]) -> type[PydanticDataclass]) | type[PydanticDataclass]`
+ error[invalid-argument-type] pydantic/dataclasses.py:319:67: Argument to function `create_dataclass` is incorrect: Expected `type[Any]`, found `_T'meta & ~None`
+ error[missing-argument] pydantic/deprecated/copy_internals.py:114:9: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[invalid-argument-type] pydantic/deprecated/copy_internals.py:114:21: Argument to bound method `__new__` is incorrect: Expected `str`, found `Model'meta`
+ error[invalid-return-type] pydantic/deprecated/copy_internals.py:120:12: Return type does not match returned value: expected `Model`, found `type`
+ error[invalid-argument-type] pydantic/experimental/pipeline.py:162:69: Argument is incorrect: Expected `type[Any]`, found `_NewOutT'meta | Unknown | ellipsis`
- error[invalid-argument-type] pydantic/experimental/pipeline.py:162:69: Argument is incorrect: Expected `type[Any]`, found `@Todo(unsupported type[X] special form) | ellipsis`
+ error[invalid-argument-type] pydantic/experimental/pipeline.py:170:74: Argument is incorrect: Expected `() -> type[Any]`, found `() -> _NewOutT'meta`
+ error[invalid-assignment] pydantic/main.py:1724:9: Object of type `tuple[(ModelT'meta & ~tuple[Unknown, ...]) | (tuple[ModelT'meta, ...] & ~tuple[Unknown, ...])]` is not assignable to `ModelT'meta | tuple[ModelT'meta, ...] | None`
+ error[invalid-argument-type] pydantic/main.py:1757:42: Argument to function `resolve_bases` is incorrect: Expected `Iterable[object]`, found `ModelT'meta | tuple[ModelT'meta, ...] | None`
+ error[invalid-type-form] pydantic/types.py:852:26: Variable of type `HashableItemType'meta` is not allowed in a type expression
+ error[invalid-type-form] pydantic/types.py:868:32: Variable of type `HashableItemType'meta` is not allowed in a type expression
+ error[invalid-type-form] pydantic/types.py:903:27: Variable of type `AnyItemType'meta` is not allowed in a type expression
+ error[invalid-argument-type] pydantic/v1/_hypothesis_plugin.py:202:23: Argument to function `issubclass` is incorrect: Expected `type`, found `T'meta | Unknown`
+ error[invalid-argument-type] pydantic/v1/dataclasses.py:238:17: Argument to function `wrap` is incorrect: Expected `type[Any]`, found `_T'meta & ~None`
+ warning[unused-ignore-comment] pydantic/v1/fields.py:1049:49: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] pydantic/v1/generics.py:97:12: Type `GenericModelT'meta` has no attribute `__concrete__`
+ error[unresolved-attribute] pydantic/v1/generics.py:108:63: Type `GenericModelT'meta` has no attribute `__parameters__`
+ error[unresolved-attribute] pydantic/v1/generics.py:113:22: Type `GenericModelT'meta` has no attribute `__concrete_name__`
+ error[unresolved-attribute] pydantic/v1/generics.py:119:39: Type `GenericModelT'meta` has no attribute `__fields__`
+ error[unresolved-attribute] pydantic/v1/generics.py:119:106: Type `GenericModelT'meta` has no attribute `__fields__`
+ error[unresolved-attribute] pydantic/v1/generics.py:127:41: Type `GenericModelT'meta` has no attribute `__parameterized_bases__`
+ error[unresolved-attribute] pydantic/v1/generics.py:145:32: Type `GenericModelT'meta` has no attribute `Config`
+ error[invalid-argument-type] pydantic/v1/generics.py:214:63: Argument to function `__class_getitem__` is incorrect: Argument type `tuple[Unknown, ...]` does not satisfy upper bound of type variable `GenericModelT'meta`
+ error[missing-argument] pydantic/v1/main.py:621:13: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[invalid-argument-type] pydantic/v1/types.py:153:24: Argument to bound method `add` is incorrect: Expected `type`, found `T'meta | ConstrainedNumberMeta`
+ error[invalid-return-type] pydantic/v1/types.py:318:12: Return type does not match returned value: expected `type[int] | type[float]`, found `type`
- error[invalid-return-type] pydantic/v1/types.py:318:12: Return type does not match returned value: expected `type[float]`, found `type`
+ error[invalid-return-type] pydantic/v1/types.py:391:12: Return type does not match returned value: expected `type[bytes]`, found `type`
+ error[invalid-return-type] pydantic/v1/types.py:471:12: Return type does not match returned value: expected `type[str]`, found `type`
+ error[invalid-return-type] pydantic/v1/types.py:833:16: Return type does not match returned value: expected `type[JsonWrapper]`, found `type`
- warning[unused-ignore-comment] pydantic/v1/validators.py:605:58: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] pydantic/v1/validators.py:554:41: Argument to bound method `__init__` is incorrect: Expected `type[Any]`, found `T'meta`
+ error[invalid-argument-type] pydantic/v1/validators.py:561:34: Argument to function `lenient_issubclass` is incorrect: Expected `type[Any] | tuple[type[Any], ...] | None`, found `T'meta`
+ error[invalid-argument-type] pydantic/v1/validators.py:563:36: Argument to bound method `__init__` is incorrect: Expected `type[Any]`, found `T'meta`
+ error[call-non-callable] pydantic/v1/validators.py:615:16: Object of type `NamedTupleT'meta` is not callable
- Found 762 diagnostics
+ Found 792 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ error[unresolved-attribute] comtypes/_comobject.py:390:39: Type `_T_IUnknown'meta` has no attribute `_iid_`
+ warning[unused-ignore-comment] comtypes/_comobject.py:400:24: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] comtypes/_post_coinit/misc.py:74:53: Type `_T_IUnknown'meta` has no attribute `_iid_`
+ warning[unused-ignore-comment] comtypes/_post_coinit/misc.py:75:19: Unused blanket `type: ignore` directive
+ error[invalid-parameter-default] comtypes/_post_coinit/misc.py:157:9: Default value of type `<class 'IClassFactory'>` is not assignable to annotated parameter type `_T_IUnknown'meta`
+ error[unresolved-attribute] comtypes/_post_coinit/unknwn.py:403:19: Type `_T_IUnknown'meta` has no attribute `_iid_`
+ warning[unused-ignore-comment] comtypes/_post_coinit/unknwn.py:408:19: Unused blanket `type: ignore` directive
+ error[invalid-parameter-default] comtypes/client/_create.py:36:9: Default value of type `<class 'IClassFactory'>` is not assignable to annotated parameter type `_T_IUnknown'meta`
+ error[no-matching-overload] comtypes/client/_create.py:144:12: No overload of function `CoGetObject` matches arguments
- warning[unused-ignore-comment] comtypes/typeinfo.py:434:73: Unused blanket `type: ignore` directive
+ error[invalid-parameter-default] comtypes/typeinfo.py:429:9: Default value of type `<class 'IUnknown'>` is not assignable to annotated parameter type `_T_IUnknown'meta`
+ error[unresolved-attribute] comtypes/typeinfo.py:433:19: Type `_T_IUnknown'meta` has no attribute `_iid_`
- Found 599 diagnostics
+ Found 609 diagnostics

black (https://github.com/psf/black)
- error[invalid-return-type] src/blib2to3/pgen2/grammar.py:151:16: Return type does not match returned value: expected `_P`, found `Grammar`
+ error[call-non-callable] src/blib2to3/pgen2/grammar.py:135:15: Object of type `_P'meta` is not callable

rich (https://github.com/Textualize/rich)
+ error[unresolved-attribute] rich/repr.py:64:28: Type `T'meta` has no attribute `__name__`
+ error[unresolved-attribute] rich/repr.py:66:27: Type `T'meta` has no attribute `__name__`
- warning[unused-ignore-comment] rich/repr.py:90:49: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rich/repr.py:93:35: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rich/repr.py:95:50: Unused blanket `type: ignore` directive
- Found 391 diagnostics
+ Found 390 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ error[call-non-callable] src/werkzeug/http.py:605:16: Object of type `_TAnyAccept'meta` is not callable
+ error[call-non-callable] src/werkzeug/http.py:634:12: Object of type `_TAnyAccept'meta` is not callable
+ error[call-non-callable] src/werkzeug/http.py:680:16: Object of type `_TAnyCC'meta` is not callable
+ error[call-non-callable] src/werkzeug/http.py:682:12: Object of type `_TAnyCC'meta` is not callable
+ error[call-non-callable] src/werkzeug/http.py:724:16: Object of type `_TAnyCSP'meta` is not callable
+ error[call-non-callable] src/werkzeug/http.py:736:12: Object of type `_TAnyCSP'meta` is not callable
+ error[call-non-callable] src/werkzeug/test.py:543:18: Object of type `_TAnyMultiDict'meta` is not callable
- Found 454 diagnostics
+ Found 461 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ error[call-non-callable] mkosi/config.py:1070:20: Object of type `SE'meta` is not callable
+ error[unresolved-attribute] mkosi/config.py:1089:21: Type `SE'meta` has no attribute `values`
+ error[call-non-callable] mkosi/config.py:1090:20: Object of type `SE'meta` is not callable
+ error[call-non-callable] mkosi/config.py:1896:16: Object of type `D'meta` is not callable
+ error[invalid-argument-type] mkosi/config.py:3076:67: Argument to function `make_simple_config_parser` is incorrect: Argument type `<class 'UKIProfile'>` does not satisfy upper bound of type variable `D'meta`
+ error[call-non-callable] mkosi/config.py:5648:16: Object of type `E'meta` is not callable
- Found 247 diagnostics
+ Found 253 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ error[unresolved-attribute] tornado/web.py:1992:5: Unresolved attribute `_stream_request_body` on type `_RequestHandlerType'meta`.
- Found 344 diagnostics
+ Found 345 diagnostics

vision (https://github.com/pytorch/vision)
+ error[unresolved-attribute] torchvision/prototype/datasets/utils/_encoded.py:35:16: Type `D'meta` has no attribute `_wrap`
+ error[call-non-callable] torchvision/prototype/datasets/utils/_encoded.py:39:24: Object of type `D'meta` is not callable
+ error[unresolved-attribute] torchvision/prototype/datasets/utils/_encoded.py:46:20: Type `D'meta` has no attribute `from_file`
+ error[unresolved-attribute] torchvision/prototype/tv_tensors/_label.py:34:18: Type `L'meta` has no attribute `_to_tensor`
+ error[unresolved-attribute] torchvision/prototype/tv_tensors/_label.py:35:16: Type `L'meta` has no attribute `_wrap`
+ error[call-non-callable] torchvision/prototype/tv_tensors/_label.py:45:16: Object of type `L'meta` is not callable
- Found 1843 diagnostics
+ Found 1849 diagnostics

jinja (https://github.com/pallets/jinja)
+ error[call-non-callable] src/jinja2/environment.py:77:11: Object of type `_env_bound'meta` is not callable
- Found 234 diagnostics
+ Found 235 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/structured_configs/_implementations.py:1310:20: Return type does not match returned value: expected `_T`, found `object`
+ error[call-non-callable] src/hydra_zen/structured_configs/_implementations.py:1310:20: Object of type `_T'meta` is not callable
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2251:31: Type `Self'meta` has no attribute `_get_obj_path`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2291:47: Type `Self'meta` has no attribute `_get_obj_path`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2511:30: Type `Self'meta` has no attribute `_sanitize_collection`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2532:29: Type `Self'meta` has no attribute `_make_hydra_compatible`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2544:23: Type `Self'meta` has no attribute `_get_sig_obj`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2681:21: Type `Self'meta` has no attribute `_make_hydra_compatible`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2896:21: Type `Self'meta` has no attribute `_make_hydra_compatible`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2914:53: Type `Self'meta` has no attribute `_sanitized_type`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2920:30: Type `Self'meta` has no attribute `_sanitized_field`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:2933:21: Type `Self'meta` has no attribute `_sanitized_type`
- error[invalid-assignment] src/hydra_zen/structured_configs/_implementations.py:3474:9: Object of type `@Todo(unsupported type[X] special form) | dict[Unknown, Unknown]` is not assignable to `DataclassOptions`
+ error[invalid-assignment] src/hydra_zen/structured_configs/_implementations.py:3474:9: Object of type `Unknown | dict[Unknown, Unknown]` is not assignable to `DataclassOptions`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:3475:13: Type `Self'meta` has no attribute `_default_dataclass_options_for_kwargs_of`
+ error[unresolved-attribute] src/hydra_zen/structured_configs/_implementations.py:3476:16: Type `Self'meta` has no attribute `_default_dataclass_options_for_kwargs_of`
- warning[unused-ignore-comment] src/hydra_zen/structured_configs/_implementations.py:3491:29: Unused blanket `type: ignore` directive
+ error[no-matching-overload] src/hydra_zen/structured_configs/_make_custom_builds.py:318:9: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] src/hydra_zen/wrapper/_implementations.py:935:20: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] src/hydra_zen/wrapper/_implementations.py:951:39: No overload of bound method `builds` matches arguments
+ error[call-non-callable] src/hydra_zen/wrapper/_implementations.py:1557:18: Object of type `Self'meta` is not callable
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:1578:20: Return type does not match returned value: expected `F | Self`, found `ZenStore`
+ error[no-matching-overload] tests/annotations/declarations.py:1392:5: No overload of bound method `builds` matches arguments
- warning[unused-ignore-comment] tests/annotations/declarations.py:1430:17: Unused blanket `type: ignore` directive
+ error[no-matching-overload] tests/annotations/declarations.py:1427:17: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] tests/annotations/declarations.py:1428:17: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] tests/annotations/declarations.py:1429:17: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] tests/annotations/declarations.py:1510:5: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] tests/annotations/declarations.py:1511:5: No overload of bound method `builds` matches arguments
- warning[unused-ignore-comment] tests/annotations/declarations.py:1512:49: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:1513:49: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:1514:39: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:1515:39: Unused blanket `type: ignore` directive
- Found 638 diagnostics
+ Found 653 diagnostics

paasta (https://github.com/yelp/paasta)
+ error[call-non-callable] paasta_tools/frameworks/task_store.py:75:16: Object of type `_SelfT'meta` is not callable
+ error[unresolved-attribute] paasta_tools/paasta_service_config_loader.py:130:19: Type `InstanceConfig_T'meta` has no attribute `config_filename_prefix`
+ error[unresolved-attribute] paasta_tools/paasta_service_config_loader.py:137:27: Type `InstanceConfig_T'meta` has no attribute `config_filename_prefix`
+ error[call-non-callable] paasta_tools/paasta_service_config_loader.py:183:32: Object of type `InstanceConfig_T'meta` is not callable
+ error[call-non-callable] paasta_tools/paasta_service_config_loader.py:194:16: Object of type `InstanceConfig_T'meta` is not callable
- Found 935 diagnostics
+ Found 940 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ error[unresolved-attribute] psycopg/psycopg/_adapters_map.py:282:56: Type `RV'meta` has no attribute `__name__`
+ error[unresolved-attribute] psycopg/psycopg/_typeinfo.py:86:20: Type `T'meta` has no attribute `_fetch`
+ error[unresolved-attribute] psycopg/psycopg/_typeinfo.py:88:20: Type `T'meta` has no attribute `_fetch_async`
+ error[unresolved-attribute] psycopg/psycopg/_typeinfo.py:105:29: Type `T'meta` has no attribute `_get_info_query`
+ error[unresolved-attribute] psycopg/psycopg/_typeinfo.py:110:16: Type `T'meta` has no attribute `_from_records`
+ error[unresolved-attribute] psycopg/psycopg/_typeinfo.py:123:39: Type `T'meta` has no attribute `_get_info_query`
+ error[unresolved-attribute] psycopg/psycopg/_typeinfo.py:128:16: Type `T'meta` has no attribute `_from_records`
+ error[call-non-callable] psycopg/psycopg/_typeinfo.py:135:20: Object of type `T'meta` is not callable
+ error[call-non-callable] psycopg/psycopg/rows.py:162:24: Object of type `T'meta` is not callable
- Found 1118 diagnostics
+ Found 1127 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- warning[unused-ignore-comment] mkdocs/plugins.py:65:52: Unused blanket `type: ignore` directive
+ error[call-non-callable] mkdocs/tests/config/config_options_tests.py:57:18: Object of type `SomeConfig'meta` is not callable

scrapy (https://github.com/scrapy/scrapy)
- warning[unused-ignore-comment] scrapy/middleware.py:57:56: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] scrapy/middleware.py:63:32: Type `_T'meta` has no attribute `__qualname__`
- warning[unused-ignore-comment] scrapy/utils/misc.py:187:67: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] scrapy/utils/misc.py:204:28: Type `T'meta` has no attribute `__qualname__`

colour (https://github.com/colour-science/colour)
+ error[invalid-argument-type] colour/geometry/primitives.py:218:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/geometry/primitives.py:418:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/diagrams.py:299:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/diagrams.py:357:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/models.py:320:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/models.py:345:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/temperature.py:155:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/temperature.py:341:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/plotting/temperature.py:375:9: Argument to function `zeros` is incorrect: Expected `type[Unknown] | None`, found `list[Unknown]`
+ error[invalid-argument-type] colour/volume/rgb.py:142:28: Argument to function `random_triplet_generator` is incorrect: Expected `int`, found `signedinteger[_32Bit] | signedinteger[_64Bit]`
+ error[invalid-argument-type] colour/volume/rgb.py:333:28: Argument to function `random_triplet_generator` is incorrect: Expected `int`, found `signedinteger[_32Bit] | signedinteger[_64Bit]`
- Found 536 diagnostics
+ Found 547 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[unused-ignore-comment] mitmproxy/coretypes/serializable.py:94:34: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] mitmproxy/coretypes/serializable.py:91:22: Type `U'meta` has no attribute `__fields`
+ error[call-non-callable] mitmproxy/tools/main.py:68:18: Object of type `T'meta` is not callable
+ error[call-non-callable] test/mitmproxy/addons/test_proxyserver.py:668:17: Object of type `T'meta` is not callable
+ error[unresolved-attribute] test/mitmproxy/proxy/tutils.py:392:59: Type `T'meta` has no attribute `__name__`
+ error[invalid-parameter-default] test/mitmproxy/proxy/tutils.py:405:17: Default value of type `typing.Any` is not assignable to annotated parameter type `T'meta`
- Found 2112 diagnostics
+ Found 2116 diagnostics

mypy (https://github.com/python/mypy)
+ error[invalid-type-form] mypy/typeshed/stdlib/multiprocessing/context.pyi:32:33: Variable of type `type[Unknown]` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/multiprocessing/context.pyi:33:35: Variable of type `type[Unknown]` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/multiprocessing/context.pyi:34:33: Variable of type `type[Unknown]` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/multiprocessing/context.pyi:35:40: Variable of type `type[Unknown]` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/multiprocessing/context.pyi:164:28: Variable of type `type[Unknown]` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:265:33: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:267:37: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:269:29: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:271:38: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:273:38: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:275:37: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:277:41: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:279:40: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:281:40: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/sqlite3/__init__.pyi:283:31: Variable of type `property` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:892:20: Variable of type `TypeVar` is not allowed in a type expression
- Found 3316 diagnostics
+ Found 3332 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
- error[invalid-return-type] src/split_parser.py:47:16: Return type does not match returned value: expected `T`, found `str | int | float`
+ error[call-non-callable] src/split_parser.py:43:17: Object of type `T'meta` is not callable

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ error[invalid-parameter-default] bson/codec_options.py:259:13: Default value of type `ellipsis` is not assignable to annotated parameter type `_DocumentType'meta | None`
- Found 554 diagnostics
+ Found 555 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ error[call-non-callable] cwltool/mpi.py:61:20: Object of type `MpiConfigT'meta` is not callable
- Found 302 diagnostics
+ Found 303 diagnostics

meson (https://github.com/mesonbuild/meson)
+ error[invalid-argument-type] mesonbuild/build.py:3175:49: Argument to function `pickle_load` is incorrect: Argument type `<class 'Build'>` does not satisfy upper bound of type variable `_PL'meta`
+ error[invalid-type-form] mesonbuild/compilers/detect.py:882:26: Variable of type `Never` is not allowed in a type expression
+ error[invalid-type-form] mesonbuild/compilers/detect.py:882:53: Variable of type `Never` is not allowed in a type expression
+ error[invalid-type-form] mesonbuild/compilers/detect.py:968:21: Variable of type `Never` is not allowed in a type expression
+ error[invalid-type-form] mesonbuild/compilers/detect.py:1176:17: Variable of type `Never` is not allowed in a type expression
+ error[invalid-argument-type] mesonbuild/coredata.py:682:46: Argument to function `pickle_load` is incorrect: Argument type `<class 'CoreData'>` does not satisfy upper bound of type variable `_PL'meta`
- error[invalid-return-type] mesonbuild/interpreter/interpreter.py:2268:16: Return type does not match returned value: expected `TestClass`, found `@Todo(unsupported type[X] special form) | Test`
+ error[invalid-parameter-default] mesonbuild/interpreter/interpreter.py:2238:19: Default value of type `<class 'Test'>` is not assignable to annotated parameter type `TestClass'meta`
+ error[call-non-callable] mesonbuild/interpreter/interpreter.py:2268:16: Object of type `TestClass'meta` is not callable
- error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:526:45: Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
- error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:527:54: Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:526:45: Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:527:54: Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo]`
- error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:546:45: Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
- error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:546:197: Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | tuple[@Todo(unsupported type[X] special form) | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:546:45: Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/interpreterbase/decorators.py:546:197: Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo | tuple[Unknown | _T'meta | tuple[_T'meta | ContainerTypeInfo, ...] | ContainerTypeInfo]`
+ error[invalid-argument-type] mesonbuild/minstall.py:129:46: Argument to function `pickle_load` is incorrect: Argument type `<class 'InstallData'>` does not satisfy upper bound of type variable `_PL'meta`
+ error[call-non-callable] mesonbuild/mparser.py:701:16: Object of type `BaseNodeT'meta` is not callable
- Found 1367 diagnostics
+ Found 1376 diagnostics

altair (https://github.com/vega/altair)
+ error[unresolved-attribute] altair/utils/schemapi.py:1307:13: Type `TSchemaBase'meta` has no attribute `validate`
+ error[unresolved-attribute] altair/utils/schemapi.py:1308:31: Type `TSchemaBase'meta` has no attribute `_default_wrapper_classes`
+ error[unresolved-attribute] altair/utils/schemapi.py:1584:30: Type `TSchemaBase'meta` has no attribute `_schema`
+ error[unresolved-attribute] altair/utils/schemapi.py:1585:57: Type `TSchemaBase'meta` has no attribute `_rootschema`
+ error[call-non-callable] altair/utils/schemapi.py:1619:20: Object of type `TSchemaBase'meta` is not callable
- error[invalid-return-type] altair/utils/schemapi.py:1619:20: Return type does not match returned value: expected `SchemaBase`, found `@Todo(Type::Intersection.call()) | dict[str, Any]`
+ error[invalid-return-type] altair/utils/schemapi.py:1619:20: Return type does not match returned value: expected `SchemaBase`, found `Unknown | dict[str, Any]`
+ error[call-non-callable] altair/utils/schemapi.py:1622:20: Object of type `TSchemaBase'meta` is not callable
- error[invalid-return-type] altair/utils/schemapi.py:1622:20: Return type does not match returned value: expected `SchemaBase`, found `@Todo(Type::Intersection.call()) | dict[str, Any]`
+ error[invalid-return-type] altair/utils/schemapi.py:1622:20: Return type does not match returned value: expected `SchemaBase`, found `Unknown | dict[str, Any]`
+ error[call-non-callable] altair/utils/schemapi.py:1625:20: Object of type `TSchemaBase'meta` is not callable
- error[invalid-return-type] altair/utils/schemapi.py:1625:20: Return type does not match returned value: expected `SchemaBase`, found `@Todo(Type::Intersection.call()) | dict[str, Any]`
+ error[invalid-return-type] altair/utils/schemapi.py:1625:20: Return type does not match returned value: expected `SchemaBase`, found `Unknown | dict[str, Any]`
+ error[unresolved-attribute] altair/utils/schemapi.py:1679:14: Type `TSchemaBase'meta` has no attribute `resolve_references`
+ error[invalid-super-argument] altair/vegalite/v5/api.py:3977:19: `_TSchemaBase'meta` is not an instance or subclass of `<class 'Chart'>` in `super(<class 'Chart'>, _TSchemaBase'meta)` call
+ error[invalid-argument-type] tests/expr/test_expr.py:49:35: Argument to function `classify_class_attrs` is incorrect: Expected `type`, found `T'meta`
- Found 1300 diagnostics
+ Found 1310 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ error[call-non-callable] static_frame/core/frame.py:524:20: Object of type `Self'meta` is not callable
+ error[unresolved-attribute] static_frame/core/frame.py:542:29: Type `Self'meta` has no attribute `_COLUMNS_CONSTRUCTOR`
+ error[unresolved-attribute] static_frame/core/frame.py:589:25: Type `Self'meta` has no attribute `_COLUMNS_CONSTRUCTOR`
+ error[call-non-callable] static_frame/core/frame.py:633:16: Object of type `Self'meta` is not callable
+ error[call-non-callable] static_frame/core/index.py:258:16: Object of type `I'meta` is not callable
+ error[unresolved-attribute] static_frame/core/index.py:398:9: Unresolved attribute `_map` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:399:9: Unresolved attribute `_labels` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:400:9: Unresolved attribute `_positions` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:401:9: Unresolved attribute `_recache` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:402:9: Unresolved attribute `_name` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:403:9: Unresolved attribute `_argsort_cache` on type `type`.
- error[invalid-return-type] static_frame/core/index.py:423:16: Return type does not match returned value: expected `I`, found `Index[Any]`
+ error[invalid-return-type] static_frame/core/index.py:406:16: Return type does not match returned value: expected `I`, found `type`
- error[invalid-return-type] static_frame/core/index.py:441:16: Return type does not match returned value: expected `I`, found `Index[Any]`
+ error[call-non-callable] static_frame/core/index.py:423:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index.py:441:16: Object of type `I'meta` is not callable
+ error[unresolved-attribute] static_frame/core/index.py:1529:9: Unresolved attribute `_map` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:1530:9: Unresolved attribute `_labels` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:1531:9: Unresolved attribute `_positions` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:1532:9: Unresolved attribute `_recache` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:1533:9: Unresolved attribute `_name` on type `type`.
+ error[unresolved-attribute] static_frame/core/index.py:1537:9: Unresolved attribute `_argsort_cache` on type `type`.
+ error[invalid-return-type] static_frame/core/index.py:1540:16: Return type does not match returned value: expected `I`, found `type`
+ error[call-non-callable] static_frame/core/index_datetime.py:251:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:271:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:291:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:364:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:384:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:404:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:445:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:463:16: Object of type `I'meta` is not callable
+ error[call-non-callable] static_frame/core/index_datetime.py:482:16: Object of type `I'meta` is not callable
+ error[invalid-assignment] static_frame/core/loc_map.py:259:9: Object of type `type` is not assignable to `_HLMap`
+ error[call-non-callable] static_frame/core/series.py:3429:16: Object of type `FrameType'meta` is not callable
- warning[unused-ignore-comment] static_frame/test/unit/test_store_zip.py:57:60: Unused blanket `type: ignore` directive
- Found 2063 diagnostics
+ Found 2092 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ error[invalid-argument-type] src/_pytest/deprecated.py:42:5: Argument is incorrect: Expected `_W'meta`, found `<class 'PytestRemovedIn9Warning'>`
+ error[invalid-argument-type] src/_pytest/deprecated.py:49:5: Argument is incorrect: Expected `_W'meta`, found `<class 'PytestRemovedIn9Warning'>`
+ error[invalid-argument-type] src/_pytest/deprecated.py:57:5: Argument is incorrect: Expected `_W'meta`, found `<class 'PytestDeprecationWarning'>`
- warning[unused-ignore-comment] src/_pytest/nodes.py:111:48: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/_pytest/nodes.py:126:54: Unused blanket `type: ignore` directive
+ error[invalid-return-type] src/_pytest/raises.py:443:20: Return type does not match returned value: expected `BaseExcT_1'meta`, found `BaseExcT_1'meta | (GenericAlias & type[BaseException])`
- Found 882 diagnostics
+ Found 884 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ error[call-non-callable] github/GithubObject.py:369:27: Object of type `T_gh'meta` is not callable
+ error[call-non-callable] github/GithubObject.py:392:38: Object of type `T_gh'meta` is not callable
+ error[call-non-callable] github/GithubObject.py:408:23: Object of type `T_gh'meta` is not callable
+ error[unresolved-attribute] github/Requester.py:696:12: Type `T_gh'meta` has no attribute `is_rest`
+ error[call-non-callable] github/Requester.py:698:16: Object of type `T_gh'meta` is not callable
+ error[invalid-argument-type] tests/PaginatedList.py:321:13: Argument to bound method `__init__` is incorrect: Argument type `<class 'CommitComment'>` does not satisfy upper bound of type variable `T'meta`
- Found 316 diagnostics
+ Found 322 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ warning[call-possibly-unbound-method] ext/tests/test_monkeypatching.py:69:20: Method `__getitem__` of type `Unknown | _T'meta` is possibly unbound
+ warning[call-possibly-unbound-method] ext/tests/test_monkeypatching.py:111:20: Method `__getitem__` of type `Unknown | _T'meta` is possibly unbound
- Found 471 diagnostics
+ Found 473 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ error[call-non-callable] lib/streamlit/runtime/connection_factory.py:81:16: Object of type `ConnectionClass'meta` is not callable
- Found 3257 diagnostics
+ Found 3258 diagnostics

rotki (https://github.com/rotki/rotki)
+ error[call-non-callable] rotkehlchen/accounting/structures/processed_event.py:256:21: Object of type `T'meta` is not callable
- warning[unused-ignore-comment] rotkehlchen/utils/mixins/enums.py:13:64: Unused blanket `type: ignore` directive

zulip (https://github.com/zulip/zulip)
+ error[invalid-assignment] zerver/lib/event_schema.py:259:1: Object of type `dict[str, <class 'PersonAvatarFields'> | <class 'PersonBotOwnerId'> | <class 'PersonCustomProfileField'> | <class 'PersonDeliveryEmail'> | <class 'PersonEmail'> | <class 'PersonFullName'> | <class 'PersonRole'> | <class 'PersonTimezone'> | <class 'PersonIsActive'>]` is not assignable to `dict[str, type[Unknown]]`
+ error[invalid-argument-type] zerver/lib/typed_endpoint.py:221:46: Argument to function `is_optional` is incorrect: Expected `type`, found `T'meta`
+ error[invalid-argument-type] zerver/lib/typed_endpoint.py:238:21: Argument to function `is_annotated` is incorrect: Expected `type`, found `T'meta | Any`
+ error[unresolved-attribute] zerver/worker/base.py:60:9: Unresolved attribute `queue_name` on type `ConcreteQueueWorker'meta`.
- warning[unused-ignore-comment] zilencer/views.py:984:51: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] zilencer/views.py:981:26: Type `ModelT'meta` has no attribute `_meta`
+ error[unresolved-attribute] zilencer/views.py:987:31: Type `ModelT'meta` has no attribute `_meta`
+ error[unresolved-attribute] zilencer/views.py:996:13: Type `ModelT'meta` has no attribute `_meta`
- Found 6977 diagnostics
+ Found 6983 diagnostics

sympy (https://github.com/sympy/sympy)
- error[not-iterable] sympy/functions/elementary/piecewise.py:1095:72: Object of type `property` is not iterable
- error[no-matching-overload] sympy/logic/boolalg.py:2850:19: No overload of function `__new__` matches arguments
- error[not-iterable] sympy/printing/smtlib.py:525:23: Object of type `property` is not iterable
- error[no-matching-overload] sympy/printing/smtlib.py:534:30: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/printing/smtlib.py:534:30: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/printing/smtlib.py:534:30: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/printing/smtlib.py:543:30: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/printing/smtlib.py:543:30: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/printing/smtlib.py:543:30: No overload of function `__new__` matches arguments
- error[unresolved-attribute] sympy/printing/smtlib.py:551:12: Attribute `is_integer` can only be accessed on instances, not on the class object `<class 'Symbol'>` itself.
- error[unresolved-attribute] sympy/printing/smtlib.py:558:12: Attribute `is_real` can only be accessed on instances, not on the class object `<class 'Symbol'>` itself.
- error[unresolved-attribute] sympy/printing/smtlib.py:558:35: Attribute `is_integer` can only be accessed on instances, not on the class object `<class 'Symbol'>` itself.
- error[non-subscriptable] sympy/solvers/ode/ode.py:2402:8: Cannot subscript object of type `property` with no `__getitem__` method
- error[non-subscriptable] sympy/solvers/ode/ode.py:2405:22: Cannot subscript object of type `property` with no `__getitem__` method
- error[no-matching-overload] sympy/solvers/ode/ode.py:2416:16: No overload of function `subs` matches arguments
- error[unresolved-attribute] sympy/utilities/codegen.py:700:27: Type `property` has no attribute `label`
- error[unresolved-attribute] sympy/utilities/codegen.py:1374:27: Type `property` has no attribute `label`
- error[unresolved-attribute] sympy/utilities/codegen.py:1582:27: Type `property` has no attribute `label`
- error[unresolved-attribute] sympy/utilities/codegen.py:1813:27: Type `property` has no attribute `label`
- Found 18757 diagnostics
+ Found 18738 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:38:16: Type `T'meta` has no attribute `parse`
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:39:16: Type `T'meta` has no attribute `get_prefix`
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:46:22: Type `T'meta` has no attribute `get_prefix`
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:47:16: Type `T'meta` has no attribute `parse`
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:52:27: Type `T'meta` has no attribute `get_prefix`
+ error[invalid-super-argument] misc/python/materialize/mz_version.py:67:16: `T'meta` is not an instance or subclass of `<class 'TypedVersionBase'>` in `super(<class 'TypedVersionBase'>, T'meta)` call
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:75:20: Type `T'meta` has no attribute `parse`
+ error[unresolved-attribute] misc/python/materialize/mz_version.py:103:16: Type `T'meta` has no attribute `parse`
+ error[call-non-callable] misc/python/materialize/scalability/df/df_totals.py:136:26: Object of type `SCALABILITY_CHANGE_TYPE'meta` is not callable
+ error[unresolved-attribute] misc/python/materialize/util.py:45:10: Type `T'meta` has no attribute `__subclasses__`
+ error[unresolved-attribute] misc/python/materialize/zippy/framework.py:101:13: Type `T'meta` has no attribute `format_str`
- Found 3217 diagnostics
+ Found 3228 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-04-23 18:24_

Might need to park this for now; it's not urgent and there's quite a few false positives in the mypy_primer report there.

---

_Comment by @AlexWaygood on 2026-01-07 13:55_

Hopelessly merge-conflicted, and I'm not sure this is the way we want to do this either

---

_Closed by @AlexWaygood on 2026-01-07 13:55_

---

_Branch deleted on 2026-01-07 13:55_

---
