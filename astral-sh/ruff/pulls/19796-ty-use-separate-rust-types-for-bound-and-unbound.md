```yaml
number: 19796
title: "[ty] Use separate Rust types for bound and unbound type variables"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/bound-typevar
created_at: 2025-08-07T01:00:54Z
updated_at: 2025-08-11T19:30:00Z
url: https://github.com/astral-sh/ruff/pull/19796
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Use separate Rust types for bound and unbound type variables

---

_Pull request opened by @dcreager on 2025-08-07 01:00_

This PR creates separate Rust types for bound and unbound type variables, as proposed in https://github.com/astral-sh/ty/issues/926.

Closes https://github.com/astral-sh/ty/issues/926

---

_Review requested from @carljm by @dcreager on 2025-08-07 01:00_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-07 01:00_

---

_Review requested from @sharkdp by @dcreager on 2025-08-07 01:00_

---

_Review requested from @MichaReiser by @dcreager on 2025-08-07 01:00_

---

_Comment by @dcreager on 2025-08-07 01:04_

@carljm and I concurrently realized that we didn't need a new `Type` variant for unbound typevars, since we can use `KnownInstanceType::TypeVar` for that. A wrinkle is that at first I thought that unbound typevars should not be allowed in type expressions.  But that breaks default values for legacy typevars:

```py
from typing import Generic, TypeVar

T = TypeVar("T")
U = TypeVar("U", default=T)
```

The use of `T` is (correctly) unbound, but also appears in a type form position, so we do need to allow `KnownInstanceType::TypeVar` in type expressions.

> I feel like perhaps this calls into question the strategy of binding typevars in `infer_place_load` in the first place; it seems like maybe we should really only ever be binding them in `in_type_expression`?

This was a good suggestion, which I've implemented.


---

_Comment by @github-actions[bot] on 2025-08-07 01:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_typing.py:40:39: error[non-subscriptable] Cannot subscript object of type `<class 'Iterable[tuple[KT, VT]]'>` with no `__class_getitem__` method
+ bidict/_typing.py:40:39: error[non-subscriptable] Cannot subscript object of type `<class 'Iterable[tuple[typing.TypeVar("KT"), typing.TypeVar("VT")]]'>` with no `__class_getitem__` method

Expression (https://github.com/cognitedata/Expression)
+ expression/collections/maptree.py:47:18: error[invalid-argument-type] Argument to class `MapTreeLeaf` is incorrect: Expected `SupportsLessThan`, found `typing.TypeVar("Key", bound=SupportsLessThan)`
- expression/extra/result/catch.py:50:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> _TSource@catch, /) -> ((...) -> Result[_TSource@catch, _TError@catch]) | Result[_TSource@catch, _TError@catch]`, found `(...) -> Result[_TSource@catch, _TError@catch]`
- expression/extra/result/catch.py:50:26: error[invalid-argument-type] Argument to function `decorator` is incorrect: Expected `(...) -> _TSource@catch`, found `(...) -> _TSource@catch`
- expression/extra/result/catch.py:52:12: error[invalid-return-type] Return type does not match returned value: expected `((...) -> _TSource@catch, /) -> ((...) -> Result[_TSource@catch, _TError@catch]) | Result[_TSource@catch, _TError@catch]`, found `def decorator(fn: (...) -> _TSource@catch) -> (...) -> Result[_TSource@catch, _TError@catch]`
- Found 228 diagnostics
+ Found 226 diagnostics

black (https://github.com/psf/black)
+ src/black/rusty.py:28:23: error[invalid-argument-type] Argument to class `Err` is incorrect: Expected `Exception`, found `typing.TypeVar("E", bound=Exception)`
- Found 63 diagnostics
+ Found 64 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/from_thread.py:291:16: error[invalid-return-type] Return type does not match returned value: expected `T_Retval@call`, found `T_Retval@call`
- Found 224 diagnostics
+ Found 223 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `UpdateDictMixin[Any, Any]`
+ src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `UpdateDictMixin[Any, Any]`
- src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `Self@setdefault`
+ src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `Self@setdefault`
- src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `Self@pop`
+ src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `Self@pop`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/commands.py:111:64: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:111:64: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:113:61: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:113:61: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:116:52: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:116:52: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:122:62: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:122:62: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:123:54: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:123:54: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:132:48: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:132:48: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:133:46: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:133:46: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:134:49: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:134:49: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:135:61: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:135:61: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/app_commands/commands.py:139:53: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:139:53: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:139:63: error[invalid-argument-type] Argument to class `Choice` is incorrect: Expected `str | int | float`, found `typing.TypeVar("ChoiceT", str, int, int | float, str | int | float)`
- discord/app_commands/commands.py:140:45: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:140:45: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
+ discord/app_commands/commands.py:140:55: error[invalid-argument-type] Argument to class `Choice` is incorrect: Expected `str | int | float`, found `typing.TypeVar("ChoiceT", str, int, int | float, str | int | float)`
- discord/app_commands/commands.py:2537:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_only | ((T@guild_only, /) -> T@guild_only)`, found `def inner(f: T@guild_only) -> T@guild_only`
- discord/app_commands/commands.py:2539:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_only | ((T@guild_only, /) -> T@guild_only)`, found `T@guild_only`
- discord/app_commands/commands.py:2539:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@guild_only`, found `T@guild_only & ~None`
- discord/app_commands/commands.py:2594:16: error[invalid-return-type] Return type does not match returned value: expected `T@private_channel_only | ((T@private_channel_only, /) -> T@private_channel_only)`, found `def inner(f: T@private_channel_only) -> T@private_channel_only`
- discord/app_commands/commands.py:2596:16: error[invalid-return-type] Return type does not match returned value: expected `T@private_channel_only | ((T@private_channel_only, /) -> T@private_channel_only)`, found `T@private_channel_only`
- discord/app_commands/commands.py:2596:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@private_channel_only`, found `T@private_channel_only & ~None`
- discord/app_commands/commands.py:2649:16: error[invalid-return-type] Return type does not match returned value: expected `T@dm_only | ((T@dm_only, /) -> T@dm_only)`, found `def inner(f: T@dm_only) -> T@dm_only`
- discord/app_commands/commands.py:2651:16: error[invalid-return-type] Return type does not match returned value: expected `T@dm_only | ((T@dm_only, /) -> T@dm_only)`, found `T@dm_only`
- discord/app_commands/commands.py:2651:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@dm_only`, found `T@dm_only & ~None`
- discord/app_commands/commands.py:2744:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_install | ((T@guild_install, /) -> T@guild_install)`, found `def inner(f: T@guild_install) -> T@guild_install`
- discord/app_commands/commands.py:2746:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_install | ((T@guild_install, /) -> T@guild_install)`, found `T@guild_install`
- discord/app_commands/commands.py:2746:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@guild_install`, found `T@guild_install & ~None`
- discord/app_commands/commands.py:2795:16: error[invalid-return-type] Return type does not match returned value: expected `T@user_install | ((T@user_install, /) -> T@user_install)`, found `def inner(f: T@user_install) -> T@user_install`
- discord/app_commands/commands.py:2797:16: error[invalid-return-type] Return type does not match returned value: expected `T@user_install | ((T@user_install, /) -> T@user_install)`, found `T@user_install`
- discord/app_commands/commands.py:2797:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@user_install`, found `T@user_install & ~None`
+ discord/app_commands/tree.py:76:10: error[invalid-argument-type] Argument to class `Interaction` is incorrect: Expected `Client`, found `typing.TypeVar("ClientT", bound=Client, default=Client)`
+ discord/client.py:308:27: error[invalid-argument-type] Argument to class `ConnectionState` is incorrect: Expected `Client`, found `typing.Self`
- discord/ext/commands/_types.py:47:26: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/_types.py:47:26: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/_types.py:48:22: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/_types.py:48:22: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/_types.py:53:45: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/_types.py:53:45: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/_types.py:53:80: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/_types.py:53:80: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/_types.py:54:62: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/_types.py:54:62: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/_types.py:54:113: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/_types.py:54:113: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/hybrid.py:85:50: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/hybrid.py:85:50: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
- discord/ext/commands/hybrid.py:86:44: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
+ discord/ext/commands/hybrid.py:86:44: error[non-subscriptable] Cannot subscript object of type `<class 'Coroutine[Any, Any, typing.TypeVar("T")]'>` with no `__class_getitem__` method
+ discord/ui/modal.py:102:50: error[invalid-argument-type] Argument to class `Item` is incorrect: Expected `View`, found `typing.Self`
+ discord/ui/view.py:191:30: error[invalid-argument-type] Argument to class `Item` is incorrect: Expected `View`, found `typing.Self`
+ discord/ui/view.py:587:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ui/view.py:606:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 530 diagnostics
+ Found 523 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/__init__.py:1097:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any] | _DocumentType@decode`, found `dict[str, Any] | _DocumentType@decode`
+ pymongo/asynchronous/collection.py:104:5: error[invalid-argument-type] Argument to class `InsertOne` is incorrect: Expected `Mapping[str, Any]`, found `typing.TypeVar("_DocumentType", bound=Mapping[str, Any])`
+ pymongo/asynchronous/collection.py:107:5: error[invalid-argument-type] Argument to class `ReplaceOne` is incorrect: Expected `Mapping[str, Any]`, found `typing.TypeVar("_DocumentType", bound=Mapping[str, Any])`
+ pymongo/synchronous/collection.py:103:5: error[invalid-argument-type] Argument to class `InsertOne` is incorrect: Expected `Mapping[str, Any]`, found `typing.TypeVar("_DocumentType", bound=Mapping[str, Any])`
+ pymongo/synchronous/collection.py:106:5: error[invalid-argument-type] Argument to class `ReplaceOne` is incorrect: Expected `Mapping[str, Any]`, found `typing.TypeVar("_DocumentType", bound=Mapping[str, Any])`
- Found 508 diagnostics
+ Found 511 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/types/enum.py:236:16: error[invalid-return-type] Return type does not match returned value: expected `EnumType@enum | ((EnumType@enum, /) -> EnumType@enum)`, found `def wrap(cls: EnumType@enum) -> EnumType@enum`
- strawberry/types/enum.py:238:12: error[invalid-return-type] Return type does not match returned value: expected `EnumType@enum | ((EnumType@enum, /) -> EnumType@enum)`, found `EnumType@enum`
- strawberry/types/enum.py:238:17: error[invalid-argument-type] Argument to function `wrap` is incorrect: Expected `EnumType@enum`, found `EnumType@enum & ~AlwaysFalsy`
- strawberry/types/object_type.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `T@type | ((T@type, /) -> T@type)`, found `def wrap(cls: T@type) -> T@type`
- Found 381 diagnostics
+ Found 377 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/config.py:1210:12: error[invalid-return-type] Return type does not match returned value: expected `(_TypeT@with_config, /) -> _TypeT@with_config`, found `def inner(class_: _TypeT@with_config, /) -> _TypeT@with_config`
- pydantic/validate_call_decorator.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `AnyCallableT@validate_call | ((AnyCallableT@validate_call, /) -> AnyCallableT@validate_call)`, found `def validate(function: AnyCallableT@validate_call) -> AnyCallableT@validate_call`
- Found 759 diagnostics
+ Found 757 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_make_custom_builds.py:351:12: error[invalid-return-type] Return type does not match returned value: expected `FullBuilds[T@make_custom_builds_fn] | PBuilds[T@make_custom_builds_fn] | StdBuilds[T@make_custom_builds_fn]`, found `FullBuilds[T@make_custom_builds_fn] | PBuilds[T@make_custom_builds_fn] | StdBuilds[T@make_custom_builds_fn]`
- src/hydra_zen/wrapper/_implementations.py:1638:20: error[invalid-return-type] Return type does not match returned value: expected `F@__call__ | Self@__call__`, found `F | Self@__call__`
- Found 565 diagnostics
+ Found 563 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/core/result.py:27:23: error[invalid-argument-type] Argument to class `Err` is incorrect: Expected `Exception`, found `typing.TypeVar("E", bound=Exception)`
- Found 278 diagnostics
+ Found 279 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/raises.py:1004:33: error[invalid-argument-type] Argument to class `RaisesExc` is incorrect: Expected `BaseException`, found `typing.TypeVar("BaseExcT_co", bound=BaseException)`
- Found 470 diagnostics
+ Found 471 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ django-stubs/contrib/admin/options.pyi:148:66: error[invalid-argument-type] Argument to class `QuerySet` is incorrect: Expected `Model`, found `typing.TypeVar("_ModelT", bound=Model)`
+ django-stubs/contrib/auth/models.pyi:106:23: error[invalid-argument-type] Argument to class `UserManager` is incorrect: Expected `AbstractBaseUser`, found `typing.Self`
+ django-stubs/contrib/gis/db/backends/oracle/models.pyi:12:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/contrib/gis/db/backends/oracle/models.pyi:26:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/contrib/gis/db/backends/postgis/models.pyi:15:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/contrib/gis/db/backends/postgis/models.pyi:28:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/contrib/gis/db/backends/spatialite/models.pyi:14:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/contrib/gis/db/backends/spatialite/models.pyi:28:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/db/migrations/recorder.pyi:14:27: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/db/models/base.pyi:45:23: error[invalid-argument-type] Argument to class `Manager` is incorrect: Expected `Model`, found `typing.Self`
+ django-stubs/db/models/base.pyi:47:21: error[invalid-argument-type] Argument to class `Options` is incorrect: Expected `Model`, found `typing.Self`
+ ext/django_stubs_ext/annotations.py:18:33: error[invalid-argument-type] Argument to class `Annotations` is incorrect: Expected `Mapping[str, Any]`, found `typing.TypeVar("_Annotations", bound=Mapping[str, Any])`
- mypy_django_plugin/django/context.py:419:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Field[Any, Any] | ForeignObjectRel, type[Model]]`, found `tuple[None | Field[Unknown, Unknown] | ForeignObjectRel | GenericForeignKey | Unknown, type[Model] | Unknown]`
+ mypy_django_plugin/django/context.py:419:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Field[Any, Any] | ForeignObjectRel, type[Model]]`, found `tuple[None | Unknown, type[Model] | Unknown]`
- Found 430 diagnostics
+ Found 442 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_type_clinic.py:2878:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T", bound=generic[Any])`
+ static_frame/test/unit/test_type_clinic.py:2879:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T", bound=generic[Any])`
+ static_frame/test/unit/test_type_clinic.py:2902:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T", signedinteger[_64Bit], float64)`
+ static_frame/test/unit/test_type_clinic.py:2903:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T", signedinteger[_64Bit], float64)`
+ static_frame/test/unit/test_type_clinic.py:2922:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T", signedinteger[_64Bit], float64)`
+ static_frame/test/unit/test_type_clinic.py:2923:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T", signedinteger[_64Bit], float64)`
- static_frame/test/unit/test_type_clinic.py:2947:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `T`
+ static_frame/test/unit/test_type_clinic.py:2947:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T")`
- static_frame/test/unit/test_type_clinic.py:2948:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `T`
+ static_frame/test/unit/test_type_clinic.py:2948:9: error[invalid-argument-type] Argument to class `Index` is incorrect: Expected `generic[Any]`, found `typing.TypeVar("T")`
- Found 1771 diagnostics
+ Found 1777 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/elements/dialog_decorator.py:293:16: error[invalid-return-type] Return type does not match returned value: expected `F@dialog_decorator | ((F@dialog_decorator, /) -> F@dialog_decorator)`, found `def wrapper(f: F@dialog_decorator) -> F@dialog_decorator`
- lib/streamlit/elements/dialog_decorator.py:295:5: error[invalid-assignment] Object of type `F@dialog_decorator & ~str` is not assignable to `F@dialog_decorator`
- lib/streamlit/elements/dialog_decorator.py:334:16: error[invalid-return-type] Return type does not match returned value: expected `F@experimental_dialog_decorator | ((F@experimental_dialog_decorator, /) -> F@experimental_dialog_decorator)`, found `def wrapper(f: F@experimental_dialog_decorator) -> F@experimental_dialog_decorator`
- lib/streamlit/elements/dialog_decorator.py:336:5: error[invalid-assignment] Object of type `F@experimental_dialog_decorator & ~str` is not assignable to `F@experimental_dialog_decorator`
- lib/streamlit/runtime/metrics_util.py:393:16: error[invalid-return-type] Return type does not match returned value: expected `((F@gather_metrics, /) -> F@gather_metrics) | F@gather_metrics`, found `def wrapper(f: F@gather_metrics) -> F@gather_metrics`
- Found 148 diagnostics
+ Found 143 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cargo/builder.py:33:72: error[invalid-argument-type] Argument is incorrect: Argument type `TV_TokenTypes` does not satisfy constraints of type variable `TV_TokenTypes`
+ mesonbuild/cargo/builder.py:33:72: error[invalid-argument-type] Argument is incorrect: Argument type `TV_TokenTypes@_token` does not satisfy constraints of type variable `TV_TokenTypes`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dask/prefect_dask/task_runners.py:448:16: error[invalid-return-type] Return type does not match returned value: expected `PrefectDaskFuture[R@submit]`, found `PrefectDaskFuture[R@submit]`
- src/prefect/utilities/asyncutils.py:188:24: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `R@run_coro_as_sync`
- src/prefect/utilities/collections.py:395:36: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, VT@visit_collection]`, found `dict[str, VT@visit_collection]`
- src/prefect/utilities/collections.py:463:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, VT@visit_collection].__setitem__(key: str, value: VT@visit_collection, /) -> None` cannot be called with a key of type `Literal["annotation"]` and a value of type `VT@visit_collection` on object of type `dict[str, VT@visit_collection]`
- src/prefect/utilities/collections.py:662:16: error[invalid-return-type] Return type does not match returned value: expected `VT@get_from_dict | R@get_from_dict | None`, found `VT@get_from_dict`
- src/prefect/utilities/templating.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `T@apply_values`
- src/prefect/utilities/templating.py:218:16: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `T@apply_values`
- src/prefect/utilities/templating.py:230:16: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `T@apply_values`
- Found 2965 diagnostics
+ Found 2957 diagnostics

zulip (https://github.com/zulip/zulip)
- analytics/lib/counts.py:83:12: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:84:20: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:85:16: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:164:10: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:165:56: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:166:40: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:167:28: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:168:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self`
- analytics/lib/counts.py:170:10: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:171:28: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/lib/counts.py:173:68: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/lib/counts.py:189:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self`
- analytics/lib/counts.py:191:30: error[invalid-argument-type] Argument to function `do_update_fill_state` is incorrect: Expected `FillState`, found `Self`
- analytics/management/commands/populate_analytics_db.py:124:54: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:125:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self`.
- analytics/management/commands/populate_analytics_db.py:131:13: error[invalid-argument-type] Argument to function `create_stream_subscription` is incorrect: Expected `Recipient`, found `Self`
- analytics/management/commands/populate_analytics_db.py:132:13: error[invalid-argument-type] Argument to function `create_stream_subscription` is incorrect: Expected `Stream`, found `Self`
- analytics/management/commands/populate_analytics_db.py:292:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:293:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:297:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:298:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:299:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:300:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:301:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:302:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:303:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:304:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:305:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:306:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:310:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:311:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:312:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:313:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:314:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:315:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:316:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:317:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:318:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/management/commands/populate_analytics_db.py:319:13: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:163:54: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:164:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self`.
- analytics/tests/test_counts.py:166:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Stream, Recipient]`, found `tuple[Self, Self]`
- analytics/tests/test_counts.py:177:21: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:179:9: error[unresolved-attribute] Unresolved attribute `recipient` on type `Self`.
- analytics/tests/test_counts.py:181:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DirectMessageGroup, Recipient]`, found `tuple[Self, Self]`
- analytics/tests/test_counts.py:199:16: error[invalid-return-type] Return type does not match returned value: expected `Message`, found `Self`
- analytics/tests/test_counts.py:209:16: error[invalid-return-type] Return type does not match returned value: expected `Attachment`, found `Self`
- analytics/tests/test_counts.py:287:26: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- analytics/tests/test_counts.py:288:26: error[unresolved-attribute] Type `Self` has no attribute `state`
- analytics/tests/test_counts.py:870:26: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:918:26: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_counts.py:1257:9: error[unresolved-attribute] Unresolved attribute `state` on type `Self`.
- analytics/tests/test_counts.py:1263:9: error[unresolved-attribute] Unresolved attribute `property` on type `Self`.
- analytics/tests/test_counts.py:1735:13: error[invalid-argument-type] Argument to function `do_revoke_user_invite` is incorrect: Expected `PreregistrationUser`, found `Self | None`
- analytics/tests/test_counts.py:1741:39: error[invalid-argument-type] Argument to function `do_send_user_invite_email` is incorrect: Expected `PreregistrationUser`, found `Self | None`
- analytics/tests/test_counts.py:2150:34: error[unresolved-attribute] Type `Self` has no attribute `remote_id`
- analytics/tests/test_stats_views.py:223:18: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:223:35: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:223:52: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:224:18: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:224:35: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/tests/test_stats_views.py:526:9: error[unresolved-attribute] Unresolved attribute `end_time` on type `Self`.
- analytics/views/stats.py:138:20: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/views/stats.py:140:52: error[unresolved-attribute] Type `Self` has no attribute `hostname`
- analytics/views/stats.py:243:20: error[unresolved-attribute] Type `Self` has no attribute `id`
- analytics/views/stats.py:245:38: error[unresolved-attribute] Type `Self` has no attribute `hostname`
- confirmation/models.py:114:8: error[unresolved-attribute] Type `Self` has no attribute `expiry_date`
- confirmation/models.py:114:66: error[unresolved-attribute] Type `Self` has no attribute `expiry_date`
- confirmation/models.py:117:11: error[unresolved-attribute] Type `Self` has no attribute `content_object`
- confirmation/models.py:132:16: error[unresolved-attribute] Type `Self` has no attribute `type`
- confirmation/models.py:172:12: error[invalid-return-type] Return type does not match returned value: expected `Confirmation`, found `Self`
- corporate/lib/decorator.py:118:40: error[unresolved-attribute] Type `Self` has no attribute `host`
- corporate/lib/remote_billing_util.py:129:9: error[unresolved-attribute] Type `Self` has no attribute `registration_deactivated`
- corporate/lib/remote_billing_util.py:130:12: error[unresolved-attribute] Type `Self` has no attribute `realm_deactivated`
- corporate/lib/remote_billing_util.py:131:12: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/lib/remote_billing_util.py:148:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteRealm, RemoteRealmBillingUser]`, found `tuple[Self, Self]`
- corporate/lib/remote_billing_util.py:168:8: error[unresolved-attribute] Type `Self` has no attribute `deactivated`
- corporate/lib/remote_billing_util.py:173:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteZulipServer, RemoteServerBillingUser | None]`, found `tuple[Self, None]`
- corporate/lib/remote_billing_util.py:182:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[RemoteZulipServer, RemoteServerBillingUser | None]`, found `tuple[Self, Self | None]`
- corporate/lib/stripe.py:391:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlanOffer | None`, found `Self | None`
- corporate/lib/stripe.py:1049:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self | None`
- corporate/lib/stripe.py:1074:16: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self`
- corporate/lib/stripe.py:1202:17: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/lib/stripe.py:1961:16: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/lib/stripe.py:1974:17: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:2031:29: error[unresolved-attribute] Type `Self` has no attribute `fixed_price`
- corporate/lib/stripe.py:2033:27: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:2044:20: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/lib/stripe.py:2054:33: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/lib/stripe.py:2055:33: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2192:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:2201:39: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2210:40: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2288:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self]`
- corporate/lib/stripe.py:2333:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self]`
- corporate/lib/stripe.py:2352:17: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/lib/stripe.py:2354:47: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:2355:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[None, Self]`
- corporate/lib/stripe.py:2403:43: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2407:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[Self, Self]`
- corporate/lib/stripe.py:2449:44: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/lib/stripe.py:2453:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[CustomerPlan | None, LicenseLedger | None]`, found `tuple[Self, Self]`
- corporate/lib/stripe.py:2469:20: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self | None`
- corporate/lib/stripe.py:2892:17: warning[possibly-unbound-attribute] Attribute `get_default` on type `Field[Unknown, Unknown] | ForeignObjectRel | GenericForeignKey` is possibly unbound
- corporate/lib/stripe.py:3188:16: error[unresolved-attribute] Type `Self` has no attribute `automanage_licenses`
- corporate/lib/stripe.py:3442:17: error[unresolved-attribute] Type `Self` has no attribute `type`
- corporate/lib/stripe.py:3446:32: error[unresolved-attribute] Type `Self` has no attribute `to_dict`
- corporate/lib/stripe.py:3457:39: error[unresolved-attribute] Type `Self` has no attribute `to_dict`
- corporate/lib/stripe.py:3859:27: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:3872:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:3914:81: error[unresolved-attribute] Type `Self` has no attribute `tier`
- corporate/lib/stripe.py:3927:9: error[unresolved-attribute] Unresolved attribute `invoiced_through` on type `Self`.
- corporate/lib/stripe.py:4130:20: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/stripe.py:4135:20: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/stripe.py:4534:21: error[unresolved-attribute] Type `Self` has no attribute `annual_discounted_price`
- corporate/lib/stripe.py:4535:21: error[unresolved-attribute] Type `Self` has no attribute `monthly_discounted_price`
- corporate/lib/stripe.py:4537:13: error[unresolved-attribute] Unresolved attribute `flat_discounted_months` on type `Self`.
- corporate/lib/stripe.py:4540:16: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/stripe.py:4969:21: error[unresolved-attribute] Type `Self` has no attribute `annual_discounted_price`
- corporate/lib/stripe.py:4970:21: error[unresolved-attribute] Type `Self` has no attribute `monthly_discounted_price`
- corporate/lib/stripe.py:4972:13: error[unresolved-attribute] Unresolved attribute `flat_discounted_months` on type `Self`.
- corporate/lib/stripe.py:4975:16: error[invalid-return-type] Return type does not match returned value: expected `Customer`, found `Self`
- corporate/lib/support.py:392:17: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- corporate/lib/support.py:435:17: error[unresolved-attribute] Type `Self` has no attribute `end_time`
- corporate/lib/support.py:460:24: error[unresolved-attribute] Type `Self` has no attribute `event_time`
- corporate/models/customers.py:81:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self | None`
- corporate/models/customers.py:85:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self | None`
- corporate/models/customers.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `Customer | None`, found `Self | None`
- corporate/models/plans.py:256:12: error[invalid-return-type] Return type does not match returned value: expected `CustomerPlan | None`, found `Self | None`
- corporate/models/stripe_state.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `Event | None`, found `Self | None`
- corporate/tests/test_activity_views.py:177:24: error[unresolved-attribute] Type `Self` has no attribute `last_updated`
- corporate/tests/test_activity_views.py:215:63: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_activity_views.py:219:70: error[unresolved-attribute] Type `Self` has no attribute `uuid`
- corporate/tests/test_activity_views.py:314:28: error[unresolved-attribute] Type `Self` has no attribute `last_updated`
- corporate/tests/test_activity_views.py:333:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:334:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:339:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:340:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:341:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:342:28: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteZulipServer`, found `Self`
- corporate/tests/test_activity_views.py:347:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:348:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:348:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_activity_views.py:353:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:354:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:354:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_activity_views.py:359:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:360:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:360:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_activity_views.py:362:25: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:376:18: error[invalid-argument-type] Argument to function `add_plan` is incorrect: Expected `Customer`, found `Self`
- corporate/tests/test_activity_views.py:377:28: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_activity_views.py:377:42: error[invalid-argument-type] Argument to function `add_audit_log_data` is incorrect: Expected `RemoteRealm | None`, found `Self`
- corporate/tests/test_remote_billing.py:126:30: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:127:30: error[unresolved-attribute] Type `Self` has no attribute `email`
- corporate/tests/test_remote_billing.py:130:30: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:131:30: error[unresolved-attribute] Type `Self` has no attribute `date_joined`
- corporate/tests/test_remote_billing.py:169:36: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:179:26: error[unresolved-attribute] Type `Self` has no attribute `last_login`
- corporate/tests/test_remote_billing.py:443:26: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:444:26: error[unresolved-attribute] Type `Self` has no attribute `tos_version`
- corporate/tests/test_remote_billing.py:463:26: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:464:26: error[unresolved-attribute] Type `Self` has no attribute `tos_version`
- corporate/tests/test_remote_billing.py:688:29: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:688:51: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:699:26: error[unresolved-attribute] Type `Self` has no attribute `user_uuid`
- corporate/tests/test_remote_billing.py:700:26: error[unresolved-attribute] Type `Self` has no attribute `email`
- corporate/tests/test_remote_billing.py:703:26: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:719:26: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:725:58: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:792:60: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:833:56: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:835:26: error[unresolved-attribute] Type `Self` has no attribute `host`
- corporate/tests/test_remote_billing.py:836:49: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:843:13: error[unresolved-attribute] Type `Self` has no attribute `plan_type`
- corporate/tests/test_remote_billing.py:850:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:887:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:898:9: error[unresolved-attribute] Unresolved attribute `plan_type` on type `Self`.
- corporate/tests/test_remote_billing.py:908:87: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_remote_billing.py:908:127: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:918:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/tests/test_remote_billing.py:923:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/tests/test_remote_billing.py:933:87: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_remote_billing.py:933:127: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:945:9: error[unresolved-attribute] Unresolved attribute `status` on type `Self`.
- corporate/tests/test_remote_billing.py:958:87: error[unresolved-attribute] Type `Self` has no attribute `server`
- corporate/tests/test_remote_billing.py:958:127: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:982:55: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:990:26: error[unresolved-attribute] Type `Self` has no attribute `plan_type`
- corporate/tests/test_remote_billing.py:1059:60: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1064:26: error[unresolved-attribute] Type `Self` has no attribute `customer`
- corporate/tests/test_remote_billing.py:1094:56: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1096:26: error[unresolved-attribute] Type `Self` has no attribute `host`
- corporate/tests/test_remote_billing.py:1097:49: error[invalid-argument-type] Argument to function `get_customer_by_remote_realm` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1103:26: error[unresolved-attribute] Type `Self` has no attribute `plan_type`
- corporate/tests/test_remote_billing.py:1108:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self`
- corporate/tests/test_remote_billing.py:1317:36: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_remote_billing.py:1324:26: error[unresolved-attribute] Type `Self` has no attribute `last_login`
- corporate/tests/test_remote_billing.py:1446:26: error[unresolved-attribute] Type `Self` has no attribute `email`
- corporate/tests/test_remote_billing.py:1449:26: error[unresolved-attribute] Type `Self` has no attribute `created_user`
- corporate/tests/test_remote_billing.py:1450:26: error[unresolved-attribute] Type `Self` has no attribute `date_joined`
- corporate/tests/test_stripe.py:694:13: error[unresolved-attribute] Type `Self` has no attribute `stripe_invoice_id`
- corporate/tests/test_stripe.py:700:13: error[unresolved-attribute] Type `Self` has no attribute `stripe_invoice_id`
- corporate/tests/test_stripe.py:706:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_invoice_id`
- corporate/tests/test_stripe.py:724:36: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1032:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1319:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1571:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1572:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1577:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1578:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1624:13: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self`.
- corporate/tests/test_stripe.py:1647:13: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self`.
- corporate/tests/test_stripe.py:1653:9: error[unresolved-attribute] Unresolved attribute `fixed_price` on type `Self`.
- corporate/tests/test_stripe.py:1654:9: error[unresolved-attribute] Unresolved attribute `price_per_license` on type `Self`.
- corporate/tests/test_stripe.py:1681:36: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1795:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1796:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1823:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1824:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:1827:30: error[unresolved-attribute] Type `Self` has no attribute `billing_cycle_anchor`
- corporate/tests/test_stripe.py:1856:36: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:1970:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:1971:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:2008:17: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:2013:17: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:2020:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:2021:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:2023:30: error[unresolved-attribute] Type `Self` has no attribute `billing_cycle_anchor`
- corporate/tests/test_stripe.py:2061:36: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:2173:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:2224:54: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:2243:26: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tests/test_stripe.py:2244:26: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:2276:54: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:2288:26: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tests/test_stripe.py:2289:26: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:2341:54: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:2353:26: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tests/test_stripe.py:2354:26: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:2372:16: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:2376:26: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tests/test_stripe.py:2377:26: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:2381:9: error[unresolved-attribute] Unresolved attribute `minimum_licenses` on type `Self`.
- corporate/tests/test_stripe.py:2387:66: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:2399:9: error[unresolved-attribute] Unresolved attribute `minimum_licenses` on type `Self`.
- corporate/tests/test_stripe.py:2405:70: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:2677:9: error[unresolved-attribute] Unresolved attribute `exempt_from_license_number_check` on type `Self`.
- corporate/tests/test_stripe.py:2872:26: error[unresolved-attribute] Type `Self` has no attribute `org_website`
- corporate/tests/test_stripe.py:2873:26: error[unresolved-attribute] Type `Self` has no attribute `org_description`
- corporate/tests/test_stripe.py:2875:13: error[unresolved-attribute] Type `Self` has no attribute `org_type`
- corporate/tests/test_stripe.py:2978:9: error[unresolved-attribute] Unresolved attribute `sponsorship_pending` on type `Self`.
- corporate/tests/test_stripe.py:3061:34: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:3062:90: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3062:122: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3234:30: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:3393:34: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3443:26: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:3451:26: error[unresolved-attribute] Type `Self` has no attribute `event_type`
- corporate/tests/test_stripe.py:3452:26: error[unresolved-attribute] Type `Self` has no attribute `acting_user`
- corporate/tests/test_stripe.py:3468:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:3480:27: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:3506:30: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3575:26: error[unresolved-attribute] Type `Self` has no attribute `realm`
- corporate/tests/test_stripe.py:3576:26: error[unresolved-attribute] Type `Self` has no attribute `extra_data`
- corporate/tests/test_stripe.py:3577:26: error[unresolved-attribute] Type `Self` has no attribute `extra_data`
- corporate/tests/test_stripe.py:3697:30: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3806:30: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3932:26: error[unresolved-attribute] Type `Self` has no attribute `realm`
- corporate/tests/test_stripe.py:3933:26: error[unresolved-attribute] Type `Self` has no attribute `extra_data`
- corporate/tests/test_stripe.py:3934:26: error[unresolved-attribute] Type `Self` has no attribute `extra_data`
- corporate/tests/test_stripe.py:3989:34: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:3997:26: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4011:26: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4029:34: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:4042:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4043:26: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4045:9: error[unresolved-attribute] Unresolved attribute `next_invoice_date` on type `Self`.
- corporate/tests/test_stripe.py:4050:27: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4051:26: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4062:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4064:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4078:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4079:31: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4101:30: error[unresolved-attribute] Type `Self` has no attribute `invoiced_through`
- corporate/tests/test_stripe.py:4117:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4119:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4132:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4133:31: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4155:30: error[unresolved-attribute] Type `Self` has no attribute `invoiced_through`
- corporate/tests/test_stripe.py:4173:30: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4175:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4184:30: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tests/test_stripe.py:4185:30: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:4197:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4198:30: error[unresolved-attribute] Type `Self` has no attribute `invoiced_through`
- corporate/tests/test_stripe.py:4199:31: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4242:38: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:4293:31: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4294:30: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4354:38: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:4376:38: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:4404:34: error[unresolved-attribute] Type `Self` has no attribute `id`
- corporate/tests/test_stripe.py:4506:32: error[unresolved-attribute] Type `Self` has no attribute `stripe_customer_id`
- corporate/tests/test_stripe.py:4614:9: error[unresolved-attribute] Unresolved attribute `exempt_from_license_number_check` on type `Self`.
- corporate/tests/test_stripe.py:4630:13: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:4646:9: error[unresolved-attribute] Unresolved attribute `exempt_from_license_number_check` on type `Self`.
- corporate/tests/test_stripe.py:4658:26: error[unresolved-attribute] Type `Self` has no attribute `licenses_at_next_renewal`
- corporate/tests/test_stripe.py:4659:26: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tests/test_stripe.py:4772:26: error[unresolved-attribute] Type `Self` has no attribute `next_invoice_date`
- corporate/tests/test_stripe.py:4774:26: error[unresolved-attribute] Type `Self` has no attribute `status`
- corporate/tests/test_stripe.py:4783:26: error[unresolved-attribute] Type `Self` has no attribute `licenses`
- corporate/tes...*[Comment body truncated]*

---

_Comment by @dcreager on 2025-08-07 01:07_

Doh! The size of `Type` has gone up from 16 bytes to 24 :disappointed: 

---

_Label `ty` added by @AlexWaygood on 2025-08-07 09:19_

---

_Converted to draft by @dcreager on 2025-08-07 12:05_

---

_Comment by @dcreager on 2025-08-07 12:33_

> Doh! The size of `Type` has gone up from 16 bytes to 24 😞

I made `BoundTypeVarInstance` an interned struct to bring the size of `Type` back down. That eliminates one of the proposed benefits of this PR (fewer interned structs), but I think there's still value in the additional type safety.

---

_Comment by @github-actions[bot] on 2025-08-07 12:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-11 17:44:43.096472962 +0000
+++ new-output.txt	2025-08-11 17:44:43.163473111 +0000
@@ -84,9 +84,9 @@
 aliases_typealiastype.py:63:42: error[invalid-type-form] Boolean operations are not allowed in type expressions
 aliases_typealiastype.py:64:42: error[invalid-type-form] F-strings are not allowed in type expressions
 aliases_typealiastype.py:66:47: error[unresolved-reference] Name `BadAlias21` used when not defined
-aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
-aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
-aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[T_co, T_contra]'>` with no `__class_getitem__` method
+aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar("T_co"), typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
 annotations_forward_refs.py:22:7: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:23:12: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
@@ -400,7 +400,7 @@
 generics_defaults_referential.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults_specialization.py:26:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, str]`
 generics_defaults_specialization.py:27:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, bool]`
-generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, DefaultStrT]'>` with no `__class_getitem__` method
+generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar("DefaultStrT", default=str)]'>` with no `__class_getitem__` method
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
@@ -436,16 +436,16 @@
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self]`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `Self | None`, found `LinkedList[int]`
-generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `Self | None`
-generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `Self | None`
+generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[typing.Self]`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
+generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
+generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[typing.Self]`
+generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
+generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
+generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
+generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
+generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
@@ -473,7 +473,7 @@
 generics_self_usage.py:121:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 generics_self_usage.py:125:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[Self@__mul__]`
 generics_syntax_compatibility.py:23:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `V@ClassC | K@method1`
-generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M@method2 | K`
+generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M@method2 | K@method2`
 generics_syntax_declarations.py:17:1: error[invalid-generic-class] Cannot both inherit from `typing.Generic` and use PEP 695 type variables
 generics_syntax_declarations.py:25:20: error[invalid-generic-class] Cannot both inherit from subscripted `Protocol` and use PEP 695 type variables
 generics_syntax_declarations.py:32:9: error[unresolved-attribute] Type `T@ClassD` has no attribute `is_integer`
@@ -580,20 +580,20 @@
 generics_variance.py:14:6: error[invalid-legacy-type-variable] A legacy `typing.TypeVar` cannot be both covariant and contravariant
 generics_variance.py:26:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T_co@ImmutableList]`
 generics_variance.py:57:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `B_co@func`
-generics_variance.py:175:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:175:35: error[non-subscriptable] Cannot subscript object of type `<class 'Co[T_co]'>` with no `__class_getitem__` method
-generics_variance.py:179:29: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:179:39: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:183:21: error[non-subscriptable] Cannot subscript object of type `<class 'Co[T_co]'>` with no `__class_getitem__` method
-generics_variance.py:183:27: error[non-subscriptable] Cannot subscript object of type `<class 'Co[T_co]'>` with no `__class_getitem__` method
-generics_variance.py:187:25: error[non-subscriptable] Cannot subscript object of type `<class 'Co[T_co]'>` with no `__class_getitem__` method
-generics_variance.py:187:31: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:191:33: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:191:43: error[non-subscriptable] Cannot subscript object of type `<class 'Co[T_co]'>` with no `__class_getitem__` method
-generics_variance.py:191:49: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:196:5: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:196:15: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance.py:196:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
+generics_variance.py:175:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:175:35: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+generics_variance.py:179:29: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:179:39: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:183:21: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+generics_variance.py:183:27: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+generics_variance.py:187:25: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+generics_variance.py:187:31: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:191:33: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:191:43: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
+generics_variance.py:191:49: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:196:5: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:196:15: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
+generics_variance.py:196:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
 generics_variance_inference.py:19:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T3@ClassA`
 generics_variance_inference.py:24:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int, int, int]`
 generics_variance_inference.py:25:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int | float, int | float, int]`
```
</details>


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-07 12:48_

---

_Comment by @github-actions[bot] on 2025-08-07 12:58_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 2,856 | 0 |
| `invalid-argument-type` | 37 | 746 | 21 |
| `invalid-return-type` | 0 | 166 | 1 |
| `possibly-unbound-attribute` | 5 | 43 | 0 |
| `non-subscriptable` | 0 | 0 | 20 |
| `invalid-assignment` | 0 | 14 | 0 |
| `no-matching-overload` | 3 | 0 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `not-iterable` | 0 | 1 | 0 |
| `unsupported-operator` | 0 | 1 | 0 |
| **Total** | **48** | **3,827** | **42** |

**[Full report with detailed diff](https://dcreager-bound-typevar.ecosystem-663.pages.dev/diff)**


---

_Comment by @dcreager on 2025-08-07 13:42_

The `strawberry` [ecosystem failure](https://github.com/strawberry-graphql/strawberry/blob/812dfd8a98e1a0f08b569bec904ae9535a12f87c/strawberry/federation/scalar.py#L167) is interesting:

```
[error] invalid-argument-type - :167:17 - Argument to function `wrap` is incorrect: Expected `_T@scalar`, found `_T@scalar`
```

with some extra debug statements, the two typevars actually have different binding contexts:

```
 -> lhs Definition(Definition { [salsa id]: Id(1461), file: File(System("/home/dcreager/git/code-nav/strawberry/strawberry/types/scalar.py")), file_scope: FileScopeId(0), place: Symbol(ScopedSymbolId(25)), kind: Function(AstNodeRef { kind: StmtFunctionDef, range: 4370..6323 }), is_reexported: true })
 -> rhs Definition(Definition { [salsa id]: Id(1455), file: File(System("/home/dcreager/git/code-nav/strawberry/strawberry/types/scalar.py")), file_scope: FileScopeId(0), place: Symbol(ScopedSymbolId(25)), kind: Function(AstNodeRef { kind: StmtFunctionDef, range: 3784..4118 }), is_reexported: true })
```

because the `scalar` function is overloaded.  The lhs has a binding context of [the implementation](https://github.com/strawberry-graphql/strawberry/blob/812dfd8a98e1a0f08b569bec904ae9535a12f87c/strawberry/types/scalar.py#L157), while the rhs has a binding context of [one of the overloads](https://github.com/strawberry-graphql/strawberry/blob/812dfd8a98e1a0f08b569bec904ae9535a12f87c/strawberry/types/scalar.py#L141).

Going to dig into this some more

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-08-07 17:12_

---

_Comment by @dcreager on 2025-08-07 17:21_

> The lhs has a binding context of [the implementation](https://github.com/strawberry-graphql/strawberry/blob/812dfd8a98e1a0f08b569bec904ae9535a12f87c/strawberry/types/scalar.py#L157), while the rhs has a binding context of [one of the overloads](https://github.com/strawberry-graphql/strawberry/blob/812dfd8a98e1a0f08b569bec904ae9535a12f87c/strawberry/types/scalar.py#L141).

The reason for this was interesting! Whenever we use a typevar in a type context, we need to determine if the typevar is bound at that point in the source. We walk through the enclosing scopes, to see if any of them are a generic class/function that binds the typevar in question. That requires knowing the generic context of said classes/functions. For a generic function, we find the infered type for the definition (which should be a `Type::FunctionType`), grab its signature (which is a `CallableSignature`, since the function might be overloaded), and then take the generic context of the final per-overload `Signature`.

This works because we create a `FunctionType`/`FunctionLiteral` wrapper around _every_ overload; each one only "sees" the earlier overloads when you ask e.g. for "all overloads and implementation". So the last overload in the `CallableSignature` should be the one defined by the enclosing scope that we're currently looking at.

EXCEPT! We don't include the implementation when producing the signature of a function! This is almost always what you want. But not here! For a typevar reference inside the _implementation body_ of an overloaded function, we would grab the signature of last non-implementation overload, and use that as the binding context.

The fix is to add an additional method on `FunctionType` that returns the `Signature` of the very specific overload that the type is pointing at, and to use that when walking the enclosing scopes.

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-07 17:22_

---

_Comment by @dcreager on 2025-08-07 17:23_

The remaining new ecosystem diagnostics look to all be from either:

- a `typing.Self` in an attribute annotation
- not introducing a new generic context for `Callable` types

---

_Marked ready for review by @dcreager on 2025-08-07 17:24_

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-08-07 17:47_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-08-07 17:47_

---

_Review comment by @carljm on `crates/ty_ide/src/hover.rs`:377 on 2025-08-08 21:28_

Does this display change indicate that we now consider `T` unbound here? Is that right? The type parameter to `list` is a type expression, so I would expect this use of `T` to be inferred as `Type::TypeVar`, and `T` should be bound to the type alias context.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/exception/basic.md`:105 on 2025-08-08 21:29_

Clearly it pre-exists this PR, but where does the `'instance` in this typevar's name come from?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:415 on 2025-08-08 21:31_

nit: as written, this is an invalid implementation for the above overloads, we just don't detect that yet
```suggestion
def overloaded_outer(t: T | None = None) -> None:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:23 on 2025-08-08 21:34_

Hmm, is this display for an unbound typevar potentially misleading? The runtime `TypeVar` class is not defined as a generic class; it doesn't have a type parameter. I see the value in showing the name/bound/constraints of the typevar (though pyright doesn't do that; it just shows `TypeVar` in this case).

Maybe this display is intuitive in practice even though it's kind of "wrong" in principle... I don't have a better option to suggest. Probably doesn't matter much, this value-expression use of a typevar is uncommon.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:422 on 2025-08-08 21:37_

```suggestion
def overloaded_outer[T](t: T | None = None) -> None:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/definition.rs`:26 on 2025-08-08 21:38_

Hmm, I have a vague recollection that we avoided adding such derives in the past where the ordering was "arbitrary" and there was no obvious semantic ordering. What in this PR requires this?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:486 on 2025-08-08 21:58_

This looks the same as `DisplayTypeVarInstance::fmt` -- couldn't we just replace all this with something like `f.write("{}", typevar.display())`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:46 on 2025-08-08 22:58_

So this ensures that any typevar bound by any overload of a generic function ends up bound to the last definition?

How do we ensure that we detect that an overloaded function binds a given typevar, even if, say, only some of the overloads are generic?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:57 on 2025-08-08 23:01_

So in this PR, we now create both legacy and PEP 695 typevars as unbound, and we bind them on-demand in the same way, by walking up scopes until we find the one that binds it?

That feels slightly inefficient for PEP 695 typevars, in that for them we know at creation time what scope binds them. But I guess in practice the scope-walking is quick, and in order to track their binding scope through the "KnownInstance" type, we would have to add a "this will be the binding context when we bind this typevar" to the unbound typevar struct, which is both confusing and inefficient in its own way.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1033 on 2025-08-08 23:05_

I'm pretty sure `scope` here will also always be `self.scope` on the `TypeInferenceBuilder`. (Which I guess raises the question why we have to pass it into `infer_region_scope`...) But I don't think we have to add it as an argument to these three methods.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2260 on 2025-08-08 23:29_

I think this is equivalent? Seems to pass all tests for me. We've already got the right AST node at hand; can get directly to the definition from that. And I'm not sure why we need to use `std::mem::replace` instead of `self.typevar_binding_context.replace`.
```suggestion
        let previous_typevar_binding_context = self
            .typevar_binding_context
            .replace(self.index.expect_single_definition(class));
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2303 on 2025-08-08 23:30_

```suggestion
        let previous_typevar_binding_context = self
            .typevar_binding_context
            .replace(self.index.expect_single_definition(function));

```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2334 on 2025-08-08 23:31_

```suggestion
        let previous_typevar_binding_context = self
            .typevar_binding_context
            .replace(self.index.expect_single_definition(type_alias));

```

---

_@carljm approved on 2025-08-08 23:33_

Love it!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:2260 on 2025-08-11 13:50_

Ah nice! I had a hunch there was a simpler way to do this. I'm not using the patch as-is because I used a local variable to hopefully document the intent a bit better.

> I'm not sure why we need to use `std::mem::replace` instead of `self.typevar_binding_context.replace`.

It depends on whether the value you're replacing with is an `Option` or not. (`std::mem::replace` lets you replace with `None`, `Option::replace` does not)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:2303 on 2025-08-11 13:50_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:2334 on 2025-08-11 13:50_

Done

---

_Review comment by @dcreager on `crates/ty_ide/src/hover.rs`:377 on 2025-08-11 14:07_

It always was unbound, we just couldn't see it before! You're right that it _should_ be bound, but we're not creating `GenericContext`s for type aliases yet [[playground](https://play.ty.dev/718d2782-5369-42f2-a51d-b241c35314f0)]:

```py
type Alias[T] = list[T]
reveal_type(Alias[int])  # revealed: @Todo
```

Once type aliases have a generic context, we can update `enclosing_generic_scopes` to return those, and then this will become bound as it should.  I'll add a TODO comment here.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/exception/basic.md`:105 on 2025-08-11 14:10_

`T` is bound to an exception type, but the actual caught expression will be an instance of that type. So it's not correct to reveal `T@silence` here, since that would be the type of `type(e)`. `Type::to_instance` will synthesize a new typevar with this modified name. Though a better representation might be letting `NominalInstance` wrap a typevar?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:23 on 2025-08-11 14:13_

I'm definitely open to rendering this as just `typing.TypeVar`, and to do something else with the details. I can see a few options:

- don't worry about trying to show the details
- add a new `ty_extension` function that renders the details
- try to render it like the corresponding `TypeVar` invocation; in this case `typing.TypeVar("T")`; down below e.g. `typing.TypeVar("T", default=int)`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/definition.rs`:26 on 2025-08-11 14:15_

It might not be needed anymore now that `BoundTypeVarInstance` is interned. (When it wasn't, that was needed to (easily) get an `Ord` impl for `BoundTypeVarInstance`)

Update: Yes, not needed anymore. Removed!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/display.rs`:486 on 2025-08-11 14:30_

It's subtly different because of how defaults are rendered. (Note that we're not deferring to `self.bound_typevar.typevar.default_ty` just above)

It's also hard to construct an example where you can see the difference, but this works:

```py
T = TypeVar("T")
U = TypeVar("U", default=T)

# revealed: typing.TypeVar[U = typing.TypeVar[T]]
reveal_type(U)

# revealed: typing.Generic[T, U = T@C]
class C(reveal_type(Generic[T, U])): ...
```

In the first case, `U` is used in a value context, and is still unbound, so `DisplayTypeVarInstance` shows `U = typing.TypeVar[T]`. (i.e. the default value is also unbound)

In the second case, `U` is used in a type context, and is bound to the class, so `DisplayBoundTypeVarInstance` shows `U = T@C`. (i.e. the default value has also been bound to the class)

I've added a comment in the code explaining this.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:46 on 2025-08-11 14:52_

"last" definition might be a poor name for this new function

> So this ensures that any typevar bound by any overload of a generic function ends up bound to the last definition?

No, it ensures that it gets bound to _that overload's_ definition.

> How do we ensure that we detect that an overloaded function binds a given typevar, even if, say, only some of the overloads are generic?

I think we do handle this correctly, it's just exposed weirdly in the `OverloadLiteral` / `FunctionLiteral` / `FunctionType` types.  For instance, in

```py
@overload
def f[T: int](t: T) -> None: ...
@overload
def f[U: str](u: U) -> None: ...
@overload
def f(n: None) -> None: ...
def f(x: Any) -> None: ...
```

There is an `OverloadLiteral` created for each of the four definitions, and two of them are generic, with different `GenericContext`s.

`OverloadLiteral` has a `previous_overload` method that returns the previous definition, if there is one. So the "visible" type of `f` is a `FunctionType`, which wraps a `FunctionLiteral`, which points at the last definition. And we can iterate through all the overloads by starting at the last one, and repeatedly calling that method until it returns `None`.

But somewhat confusingly, we create a `FunctionLiteral` and `FunctionType` for each of the other three definitions as well. So you can e.g. insert `reveal_type`s in between each definition and see it being built up as you go [[playground](https://play.ty.dev/04b02459-9446-43c5-9c7d-cee374a3d1f5)]:

```
def f[T: int](t: T@f) -> None
Overload[(t: T@f) -> None, (u: U@f) -> None]
Overload[(t: T@f) -> None, (u: U@f) -> None, (x: None) -> None]
Overload[(t: T@f) -> None, (u: U@f) -> None, (x: None) -> None]
```

So if we are in the body (or parameter list!) of one of the overloads, this `into_function_literal` is going to give us the "earlier" `FunctionType` that points at (and therefore stops) at that overload. So its "last" definition is the one that we want.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:57 on 2025-08-11 14:55_

> So in this PR, we now create both legacy and PEP 695 typevars as unbound, and we bind them on-demand in the same way, by walking up scopes until we find the one that binds it?

Yes that's right

> in order to track their binding scope through the "KnownInstance" type, we would have to add a "this will be the binding context when we bind this typevar" to the unbound typevar struct, which is both confusing and inefficient in its own way

This part in particular, exactly right

> But I guess in practice the scope-walking is quick,

I've also considered making it Salsa-tracked (either the per-scope list of enclosing generic contexts, or the per-typevar binding context), but that hasn't seemed necessary yet

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:1033 on 2025-08-11 14:57_

Your other change to simplify how we grab the binding context for PEP 695 scopes made this moot

---

_@dcreager reviewed on 2025-08-11 14:58_

---

_@dcreager reviewed on 2025-08-11 17:23_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:23 on 2025-08-11 17:23_

I updated this to go with option (3), so that the revealed "type" of an unbound typevar mimcs the `TypeVar` call you'd use to create it

---

_@dcreager reviewed on 2025-08-11 17:44_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/display.rs`:486 on 2025-08-11 17:44_

(These also no longer look even superficially similar since I updated the unbound rendering to look like a `TypeVar` invocation)

---

_@carljm reviewed on 2025-08-11 18:48_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/exception/basic.md`:105 on 2025-08-11 18:48_

Having `NominalInstance` wrap a typevar seems like an awkward representation, since it would only be valid for a typevar with upper bound of `type[...]`. I feel like what we're doing here makes sense, just hard to come up with a good name for it. Not something to worry about in this PR.

As a side note, this code example seems quite difficult for type checkers:

```py
from typing import Callable

def silence[T: type[BaseException]](
    func: Callable[[], None],
    exception_type: T,
) -> T | None:
    try:
        func()
    except exception_type as e:
        reveal_type(e)
        return e  # should be a type error, only returning `type(e)` would be valid
```

[Pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgLABQDAJtcFAM5I0oDG1A2gBUAXLEQCAQkXbUAogA8%2BCGEjAoAuuoAUDKHqjAArr1HEylGv37qANFABya6rd37qi6stUoA%2BvATUooI2DACUUAC0AHxQglAAPg5Owq56MCBwKfT6OQbGPFqhqVDuSvilnipqfuJQ0iVZufog1ABu1KQ1AVrU4VAAxFAt7aTUTEEA5KjsMES8dNlNQ9QwhiAoJQwMQA) reveals `BaseException*`, which is kind of reasonable but what does the `*` mean? (I _think_ it means "best guess type", which is treated as dynamic.) And then it allows `return e`, which is clearly a false negative.

[Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=bb1b624b03d74009e44c608981559c0d) doesn't even allow the `except exception_type as e`, and then reveals `Any`.

[Pyrefly](https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgFC0Am1wUAzkjSgMbUDaAKgC5YiPgCEiragFEAHjwQwkYFAF1VAClpQdUYAFduw4mUo1evVQBooAORXVr23dXnVFylAH14CasP4rWgBKKABaAD4ofigAHzsHQWcdGBA4JN1MvUMuDWDkqFcFfCL3JRUfUShJQoysnRBqADdqUkq-DWpQqABiKEaW0moGAIByVFYYIm46eobqGH0QFELaIA&version=3.12) just panics.

The next interesting bit is to try replacing `return e` with `return type(e)`, which in principle should be valid. We don't allow that, because we don't track enough metadata to round-trip the `T'instance` typevar back to `T` in `to_meta_type`. (I guess your suggestion of wrapping `T` rather than synthesizing a new typevar might allow us to fix that.)

Anyway, this is all just musing because it's interesting, not relevant to this PR!

---

_@carljm reviewed on 2025-08-11 18:52_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:23 on 2025-08-11 18:52_

I thought about your option 3, then didn't propose it because I thought it was awkward for PEP 695 typevars given that it uses "legacy" syntax. (Presumably at some point everyone will use PEP 695 type vars and nobody will use legacy syntax anymore, at which point this display representation might look weird?)

---

_@carljm reviewed on 2025-08-11 18:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:23 on 2025-08-11 18:56_

On second thought, though, this representation will always be reasonable in the sense that under the hood, PEP 695 (at runtime) is still creating an instance of `typing.TypeVar` with these arguments... it's just syntactic sugar.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:46 on 2025-08-11 19:15_

Makes sense, thank you! `last_definition_signature` does seem accurate here, I was just missing that we are typically calling this on an "incomplete" set of overloads.

---

_@carljm approved on 2025-08-11 19:16_

Looks great!

---

_@dcreager reviewed on 2025-08-11 19:25_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:23 on 2025-08-11 19:25_

> On second thought, though, this representation will always be reasonable in the sense that under the hood, PEP 695 (at runtime) is still creating an instance of `typing.TypeVar` with these arguments... it's just syntactic sugar.

Yep, that's what I was thinking.  We can always revisit this decision if it proves confusing to people

---

_Merged by @dcreager on 2025-08-11 19:29_

---

_Closed by @dcreager on 2025-08-11 19:29_

---

_Branch deleted on 2025-08-11 19:30_

---
