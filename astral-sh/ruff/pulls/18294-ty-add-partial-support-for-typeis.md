```yaml
number: 18294
title: "[ty] Add partial support for `TypeIs`"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - ty
assignees: []
base: main
head: ty-typeis
created_at: 2025-05-25T01:17:56Z
updated_at: 2025-06-09T12:33:48Z
url: https://github.com/astral-sh/ruff/pull/18294
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Add partial support for `TypeIs`

---

_Pull request opened by @InSyncWithFoo on 2025-05-25 01:17_

## Summary

Redo of #16314, part of [#117](https://github.com/astral-sh/ty/issues/117).

`TypeIs[]` is a special form that allows users to define their own narrowing functions. Despite the syntax, `TypeIs` is not a generic and, on its own, it is meaningless as a type. [Officially](https://typing.python.org/en/latest/spec/narrowing.html#typeis), a function annotated as returning a `TypeIs[T]` is a <i>type narrowing function</i>, where `T` is called the <i>`TypeIs` return type</i>.

A `TypeIs[T]` may or may not be bound to a symbol. Only bound types have narrowing effect:

```python
def f(v: object = object()) -> TypeIs[int]: ...

a: str = returns_str()

if reveal_type(f()):   # Unbound: TypeIs[int]
	reveal_type(a)     # str

if reveal_type(f(a)):  # Bound:   TypeIs[a, int]
	reveal_type(a)     # str & int
```

A `TypeIs[T]` type:

* Is fully static when `T` is fully static.
* Is a singleton/single-valued when it is bound.
* Has exactly two runtime inhabitants when it is unbound: `True` and `False`.
  In other words, an unbound type have ambiguous truthiness.
  It is possible to infer more precise truthiness for bound types; however, that is not part of this change.

`TypeIs[T]` is a subtype of or otherwise assignable to `bool`. `TypeIs` is invariant with respect to the `TypeIs` return type: `TypeIs[int]` is neither a subtype nor a supertype of `TypeIs[bool]`. When ty sees a function marked as returning `TypeIs[T]`, its `return`s will be checked against `bool` instead. ty will also report such functions if they don't accept a positional argument. Addtionally, a type narrowing function call with no positional arguments (e.g., `f()` in the example above) will be considered invalid.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-25 01:17_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-25 01:17_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-25 01:17_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-25 01:17_

---

_Comment by @github-actions[bot] on 2025-05-25 01:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- error[unresolved-attribute] beartype/_check/metadata/metadecor.py:467:34: Type `(...) -> Unknown` has no attribute `__name__`
+ error[unresolved-attribute] beartype/_check/metadata/metadecor.py:467:34: Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
- error[invalid-argument-type] beartype/_decor/_nontype/decornontype.py:369:23: Argument to function `no_type_check` is incorrect: Argument type `BeartypeableT` does not satisfy upper bound of type variable `_F`
+ warning[unused-ignore-comment] beartype/_decor/_nontype/decornontype.py:378:42: Unused blanket `type: ignore` directive
- error[invalid-argument-type] beartype/_decor/_nontype/decornontype.py:409:9: Argument to function `make_func` is incorrect: Expected `((...) -> Unknown) | None`, found `BeartypeableT`
+ error[invalid-type-guard-call] beartype/_decor/_type/_pep/_decortypepep557.py:324:31: Type guard call does not have a target
- error[invalid-argument-type] beartype/_util/api/standard/utilfunctools.py:292:29: Argument to function `unwrap_func_once` is incorrect: Expected `(...) -> Unknown`, found `BeartypeableT`
- error[unresolved-attribute] beartype/_util/cache/utilcachecall.py:568:44: Type `CallableT` has no attribute `__name__`
+ error[unresolved-attribute] beartype/_util/cache/utilcachecall.py:568:44: Type `CallableT & ((...) -> object)` has no attribute `__name__`
- error[unresolved-attribute] beartype/_util/func/utilfuncscope.py:261:29: Type `(...) -> Unknown` has no attribute `__name__`
+ error[unresolved-attribute] beartype/_util/func/utilfuncscope.py:261:29: Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
- error[unresolved-attribute] beartype/_util/func/utilfuncscope.py:320:16: Type `(...) -> Unknown` has no attribute `__qualname__`
+ error[unresolved-attribute] beartype/_util/func/utilfuncscope.py:320:16: Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__qualname__`
- error[unresolved-attribute] beartype/_util/func/utilfuncscope.py:334:16: Type `(...) -> Unknown` has no attribute `__qualname__`
+ error[unresolved-attribute] beartype/_util/func/utilfuncscope.py:334:16: Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__qualname__`
- error[unresolved-attribute] beartype/_util/func/utilfunctest.py:907:16: Type `(...) -> Unknown` has no attribute `__qualname__`
- warning[possibly-unbound-attribute] beartype/_util/func/utilfunctest.py:1100:16: Attribute `__name__` on type `Any | ((...) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_util/func/utilfunctest.py:1100:16: Attribute `__name__` on type `(Any & ~MethodType) | ((...) -> Unknown)` is possibly unbound
+ error[invalid-type-guard-call] beartype/_util/func/utilfuncwrap.py:428:38: Type guard call does not have a target
- error[invalid-argument-type] beartype/_util/hint/pep/proposal/pep484585/pep484585ref.py:532:43: Argument to function `label_callable` is incorrect: Expected `(...) -> Unknown`, found `((...) -> Unknown) | None`
+ error[invalid-type-guard-call] beartype/door/_cls/doorsuper.py:667:27: Type guard call does not have a target
- error[invalid-argument-type] beartype/door/_func/infer/inferhint.py:244:36: Argument to function `infer_hint_callable` is incorrect: Expected `(...) -> Unknown`, found `~type`
- Found 553 diagnostics
+ Found 551 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- error[call-non-callable] nion/utils/Binding.py:262:32: Object of type `None` is not callable
- Found 17 diagnostics
+ Found 16 diagnostics

bidict (https://github.com/jab/bidict)
- error[call-non-callable] bidict/_iter.py:50:34: Object of type `None` is not callable
- Found 15 diagnostics
+ Found 14 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[call-non-callable] pyinstrument/middleware.py:50:13: Object of type `None` is not callable
- Found 89 diagnostics
+ Found 88 diagnostics

kornia (https://github.com/kornia/kornia)
- error[unresolved-attribute] kornia/utils/helpers.py:116:24: Type `(...) -> Any` has no attribute `__name__`
- Found 941 diagnostics
+ Found 940 diagnostics

kopf (https://github.com/nolar/kopf)
- error[call-non-callable] kopf/_core/intents/registries.py:470:16: Object of type `str` is not callable
- error[call-non-callable] kopf/_core/intents/registries.py:470:16: Object of type `MetaFilterToken` is not callable
- Found 163 diagnostics
+ Found 161 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[invalid-argument-type] src/anyio/from_thread.py:236:35: Argument to bound method `set_result` is incorrect: Expected `T_Retval`, found `@Todo(generic `typing.Awaitable` type) | Awaitable[T_Retval] | T_Retval`
- Found 112 diagnostics
+ Found 111 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] src/graphql/execution/execute.py:1901:9: Argument to function `experimental_execute_incrementally` is incorrect: Expected `((Any, /) -> bool) | None`, found `bool | None | (def assume_not_awaitable(_value: Any) -> bool)`
+ error[invalid-argument-type] src/graphql/execution/execute.py:1901:9: Argument to function `experimental_execute_incrementally` is incorrect: Expected `((Any, /) -> bool) | None`, found `(bool & ((...) -> object)) | None | (def assume_not_awaitable(_value: Any) -> bool)`
- error[call-non-callable] src/graphql/execution/execute.py:2121:16: Object of type `None` is not callable
- error[invalid-argument-type] src/graphql/graphql.py:147:9: Argument to function `graphql_impl` is incorrect: Expected `((Any, /) -> bool) | None`, found `bool | None | (def assume_not_awaitable(_value: Any) -> bool)`
+ error[invalid-argument-type] src/graphql/graphql.py:147:9: Argument to function `graphql_impl` is incorrect: Expected `((Any, /) -> bool) | None`, found `(bool & ((...) -> object)) | None | (def assume_not_awaitable(_value: Any) -> bool)`
- Found 407 diagnostics
+ Found 406 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[call-non-callable] src/werkzeug/utils.py:505:19: Object of type `None` is not callable
- error[call-non-callable] src/werkzeug/utils.py:505:19: Object of type `int` is not callable
- error[unsupported-operator] src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `int | (((str | None, /) -> int | None) & ~None) | (Unknown & ~None)` with `Literal[0]`
+ error[unsupported-operator] src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object) & ~None) | (@Todo(Type::Intersection.call()) & ~None)` with `Literal[0]`
- error[invalid-assignment] src/werkzeug/utils.py:512:9: Object of type `int | (((str | None, /) -> int | None) & ~None) | (Unknown & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ error[invalid-assignment] src/werkzeug/utils.py:512:9: Object of type `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object) & ~None) | (@Todo(Type::Intersection.call()) & ~None)` is not assignable to attribute `max_age` of type `int | None`
- Found 452 diagnostics
+ Found 450 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-argument-type] strawberry/cli/commands/codegen.py:20:24: Argument to function `issubclass` is incorrect: Expected `type`, found `object`
- error[call-non-callable] strawberry/cli/utils/__init__.py:23:29: Object of type `object` is not callable
- error[invalid-argument-type] strawberry/experimental/pydantic/conversion.py:106:37: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type & ~<Protocol with members 'to_pydantic'>`
- error[call-non-callable] strawberry/extensions/query_depth_limiter.py:149:17: Object of type `None` is not callable
- error[call-non-callable] strawberry/types/field.py:134:38: Object of type `object` is not callable
- Found 432 diagnostics
+ Found 427 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-argument-type] src/trio/_core/_tests/test_ki.py:685:43: Argument to function `_consume_async_generator` is incorrect: Expected `AsyncGenerator[None, None]`, found `object`
- error[unresolved-attribute] src/trio/_core/_tests/test_ki.py:690:13: Type `object` has no attribute `send`
- Found 1090 diagnostics
+ Found 1088 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-argument-type] pydantic/_internal/_fields.py:281:61: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type`
- error[call-non-callable] pydantic/fields.py:1193:13: Object of type `None` is not callable
- Found 762 diagnostics
+ Found 760 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[call-non-callable] src/poetry/utils/cache.py:147:21: Object of type `T` is not callable
- error[invalid-return-type] src/poetry/utils/cache.py:149:16: Return type does not match returned value: expected `T`, found `(Unknown & ~None) | Unknown | T | (() -> T)`
+ error[invalid-return-type] src/poetry/utils/cache.py:149:16: Return type does not match returned value: expected `T`, found `(Unknown & ~None) | @Todo(Type::Intersection.call()) | (T & ~((...) -> object)) | ((() -> T) & ~((...) -> object))`
- Found 1018 diagnostics
+ Found 1017 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[call-non-callable] ignite/handlers/base_logger.py:48:20: Object of type `list[Unknown]` is not callable
- Found 2222 diagnostics
+ Found 2221 diagnostics

nox (https://github.com/wntrblm/nox)
- error[call-non-callable] nox/_option_set.py:207:21: Object of type `bool` is not callable
- Found 37 diagnostics
+ Found 36 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[call-non-callable] scrapy/downloadermiddlewares/retry.py:109:22: Object of type `str` is not callable
- error[call-non-callable] scrapy/downloadermiddlewares/retry.py:109:22: Object of type `Exception` is not callable
- error[call-non-callable] scrapy/spiders/__init__.py:180:50: Object of type `None` is not callable
- error[invalid-return-type] scrapy/spiders/crawl.py:48:16: Return type does not match returned value: expected `((...) -> Unknown) | None`, found `((...) -> Unknown) | str | None`
- error[invalid-argument-type] scrapy/utils/decorators.py:43:21: Argument to function `deco` is incorrect: Expected `(...) -> Unknown`, found `Any | None`
- error[no-matching-overload] scrapy/utils/defer.py:387:36: No overload of function `ensure_future` matches arguments
- warning[possibly-unbound-attribute] tests/test_utils_console.py:28:16: Attribute `__name__` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_utils_console.py:28:16: Attribute `__name__` on type `(@Todo(Inference of subscript on special form) & ((...) -> object)) | (None & ((...) -> object))` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_utils_console.py:34:16: Attribute `__name__` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_utils_console.py:34:16: Attribute `__name__` on type `(@Todo(Inference of subscript on special form) & ((...) -> object)) | (None & ((...) -> object))` is possibly unbound
- Found 1342 diagnostics
+ Found 1336 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-return-type] src/jinja2/async_utils.py:70:12: Return type does not match returned value: expected `V`, found `Awaitable[V] | V`
+ warning[redundant-cast] src/jinja2/async_utils.py:68:22: Value is already of type `Awaitable[V]`

websockets (https://github.com/aaugustin/websockets)
- error[call-non-callable] src/websockets/legacy/server.py:633:29: Object of type `None` is not callable
- Found 109 diagnostics
+ Found 108 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[invalid-type-form] cki_lib/messagequeue.py:373:36: Variable of type `def callable(obj: object, /) -> @Todo(`TypeIs[]` special form)` is not allowed in a type expression
+ error[invalid-type-form] cki_lib/messagequeue.py:373:36: Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression

vision (https://github.com/pytorch/vision)
- error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `Unknown | Sequence[int] | int | (((int, /) -> Sequence[int] | int) & ~None)`
+ error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `@Todo(Type::Intersection.call()) | (Sequence[int] & ~((...) -> object)) | (int & ~((...) -> object)) | (((int, /) -> Sequence[int] | int) & ~None & ~((...) -> object))`
- error[call-non-callable] test/datasets_utils.py:835:57: Object of type `Sequence[int]` is not callable
- error[call-non-callable] test/datasets_utils.py:835:57: Object of type `int` is not callable
- error[call-non-callable] test/datasets_utils.py:956:57: Object of type `Sequence[int]` is not callable
- error[call-non-callable] test/datasets_utils.py:956:57: Object of type `int` is not callable
- error[call-non-callable] torchvision/models/_utils.py:201:43: Object of type `W` is not callable
- error[call-non-callable] torchvision/models/_utils.py:201:43: Object of type `None` is not callable
- error[invalid-return-type] torchvision/transforms/v2/_utils.py:149:16: Return type does not match returned value: expected `(Any, /) -> Any`, found `(str & ~Literal["default"]) | ((Any, /) -> Any) | None`
- Found 1546 diagnostics
+ Found 1539 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-argument-type] src/hydra_zen/_launch.py:397:44: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `@Todo(Support for `typing.TypeAlias`) | Mapping[str, Any]`
- error[invalid-argument-type] src/hydra_zen/_launch.py:478:50: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `@Todo(Support for `typing.TypeAlias`) | Mapping[str, Any]`
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:1133:34: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `(~None & ~<Protocol with members '__iter__'> & ~type) | (Unknown & ~type)`
- warning[redundant-cast] src/hydra_zen/third_party/beartype.py:130:12: Value is already of type `_T`
- warning[redundant-cast] src/hydra_zen/third_party/pydantic.py:111:16: Value is already of type `_T`
- error[call-non-callable] src/hydra_zen/wrapper/_implementations.py:1610:20: Object of type `None` is not callable
+ warning[unused-ignore-comment] src/hydra_zen/wrapper/_implementations.py:1600:75: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/hydra_zen/wrapper/_implementations.py:1605:80: Unused blanket `type: ignore` directive
- Found 639 diagnostics
+ Found 635 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[call-non-callable] tornado/escape.py:352:28: Object of type `str` is not callable
- Found 351 diagnostics
+ Found 350 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/core/display.py:118:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:128:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:138:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:148:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:158:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
+ warning[unused-ignore-comment] static_frame/core/frame.py:7800:44: Unused blanket `type: ignore` directive
- Found 2059 diagnostics
+ Found 2055 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] src/_pytest/fixtures.py:1022:42: Argument to function `_eval_scope_callable` is incorrect: Expected `(str, Config, /) -> Unknown`, found `Scope | (Unknown & ~None) | (((str, Config, /) -> Unknown) & ~None)`
- error[invalid-argument-type] src/_pytest/fixtures.py:1385:9: Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | Sequence[object] | (((Any, /) -> object) & ~None) | tuple[Unknown, ...]`
- error[invalid-argument-type] src/_pytest/fixtures.py:1385:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[object] | (((Any, /) -> object) & ~None)`
+ error[invalid-argument-type] src/_pytest/fixtures.py:1385:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Sequence[object] & ~((...) -> object)) | (((Any, /) -> object) & ~None & ~((...) -> object))`
- error[invalid-argument-type] src/_pytest/python.py:1380:13: Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | Iterable[object] | (((Any, /) -> object) & ~None)`
- Found 882 diagnostics
+ Found 879 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[call-non-callable] mesonbuild/compilers/compilers.py:1290:26: Object of type `None` is not callable
- error[call-non-callable] mesonbuild/compilers/compilers.py:1290:26: Object of type `CompilerArgs` is not callable
- error[call-non-callable] mesonbuild/compilers/compilers.py:1290:26: Object of type `list[Unknown]` is not callable
- error[call-non-callable] mesonbuild/compilers/vala.py:152:26: Object of type `None` is not callable
- error[call-non-callable] mesonbuild/compilers/vala.py:152:26: Object of type `CompilerArgs` is not callable
- error[call-non-callable] mesonbuild/compilers/vala.py:152:26: Object of type `list[Unknown]` is not callable
- Found 1398 diagnostics
+ Found 1392 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[call-non-callable] cloudinit/sources/helpers/azure.py:171:9: Object of type `None` is not callable
- Found 705 diagnostics
+ Found 704 diagnostics

altair (https://github.com/vega/altair)
+ error[invalid-argument-type] altair/utils/data.py:362:35: Argument to function `sanitize_geo_interface` is incorrect: Expected `MutableMapping[Any, Any]`, found `Series[Unknown] | @Todo(map_with_boundness: intersections with negative contributions)`
- error[invalid-argument-type] altair/utils/data.py:361:37: Argument to function `sanitize_pandas_dataframe` is incorrect: Argument type `SupportsGeoInterface` does not satisfy upper bound of type variable `_PandasDataFrameT`
- error[call-non-callable] altair/utils/data.py:430:22: Object of type `None` is not callable
- error[not-iterable] altair/utils/schemapi.py:1132:25: Object of type `Literal[False] | Iterable[Any]` may not be iterable
- error[invalid-argument-type] altair/vegalite/v5/api.py:254:30: Argument to function `_dataset_name` is incorrect: Expected `dict[str, Any] | list[Unknown] | InlineDataset`, found `Unknown | UndefinedType`
- error[invalid-assignment] altair/vegalite/v5/api.py:3863:13: Object of type `Facet | (@Todo(Support for `typing.TypeAlias`) & ~str) | (UndefinedType & ~str)` is not assignable to `Facet | FacetMapping`
- error[invalid-argument-type] altair/vegalite/v5/api.py:4200:44: Argument to function `_repeat_names` is incorrect: Expected `list[str] | LayerRepeatMapping | RepeatMapping`, found `@Todo(Support for `typing.TypeAlias`) | UndefinedType`
- Found 1299 diagnostics
+ Found 1294 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[call-non-callable] aiohttp/client.py:827:23: Object of type `bool` is not callable
- error[unresolved-attribute] aiohttp/helpers.py:808:34: Type `ErrorableProtocol` has no attribute `done`
- Found 173 diagnostics
+ Found 171 diagnostics

apprise (https://github.com/caronc/apprise)
- error[call-non-callable] apprise/manager.py:241:64: Object of type `None` is not callable
- Found 3694 diagnostics
+ Found 3693 diagnostics

optuna (https://github.com/optuna/optuna)
- error[unresolved-attribute] tests/test_deprecated.py:56:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_deprecated.py:72:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_deprecated.py:192:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_experimental.py:51:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_experimental.py:64:12: Type `(...) -> Unknown` has no attribute `__name__`
- Found 1061 diagnostics
+ Found 1056 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-return-type] src/bokeh/core/property/bases.py:187:20: Return type does not match returned value: expected `T`, found `(() -> T) | T`
+ error[invalid-return-type] src/bokeh/core/property/bases.py:187:20: Return type does not match returned value: expected `T`, found `((() -> T) & ~((...) -> object)) | (T & ~((...) -> object))`
- error[invalid-return-type] src/bokeh/core/property/bases.py:190:24: Return type does not match returned value: expected `T`, found `(() -> T) | T`
+ error[invalid-return-type] src/bokeh/core/property/bases.py:190:24: Return type does not match returned value: expected `T`, found `((() -> T) & ((...) -> object)) | (T & ((...) -> object))`
- error[call-non-callable] src/bokeh/core/property/bases.py:191:20: Object of type `T` is not callable
- error[call-non-callable] src/bokeh/io/notebook.py:564:18: Object of type `str` is not callable
- error[call-non-callable] src/bokeh/io/notebook.py:576:15: Object of type `str` is not callable
- error[call-non-callable] src/bokeh/model/util.py:108:61: Object of type `None` is not callable
- error[call-non-callable] src/bokeh/plotting/graph.py:120:28: Object of type `dict[int | str, Sequence[int | float]]` is not callable
- Found 953 diagnostics
+ Found 948 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[invalid-assignment] sphinx/environment/__init__.py:371:13: Object of type `str | ((Node, /) -> bool)` is not assignable to `Literal[False] | ((Node, /) -> bool)`
+ error[invalid-assignment] sphinx/environment/__init__.py:371:13: Object of type `(str & ((...) -> object)) | (((Node, /) -> bool) & ((...) -> object))` is not assignable to `Literal[False] | ((Node, /) -> bool)`
- error[call-non-callable] sphinx/util/inspect.py:102:19: Object of type `None` is not callable
- error[unresolved-attribute] sphinx/util/inspect.py:296:33: Type `object` has no attribute `__self__`
- Found 657 diagnostics
+ Found 655 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-return-type] lib/streamlit/runtime/metrics_util.py:262:16: Return type does not match returned value: expected `str`, found `object`
- error[invalid-argument-type] lib/streamlit/testing/v1/element_tree.py:1664:29: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `object`
- error[call-non-callable] lib/streamlit/vendor/pympler/asizeof.py:269:12: Object of type `None` is not callable
- error[call-non-callable] lib/streamlit/vendor/pympler/asizeof.py:275:12: Object of type `None` is not callable
- error[call-non-callable] lib/streamlit/vendor/pympler/asizeof.py:281:12: Object of type `None` is not callable
- Found 3271 diagnostics
+ Found 3266 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[call-non-callable] openlibrary/plugins/worksearch/code.py:225:34: Object of type `str` is not callable
- Found 742 diagnostics
+ Found 741 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-type-form] ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:20: Variable of type `def callable(obj: object, /) -> @Todo(`TypeIs[]` special form)` is not allowed in a type expression
+ error[invalid-type-form] ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:20: Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression

zulip (https://github.com/zulip/zulip)
- error[invalid-argument-type] zerver/lib/event_schema.py:462:54: Argument to function `issubclass` is incorrect: Expected `type`, found `type | UnionType`
- warning[possibly-unbound-attribute] zerver/tests/test_openapi.py:584:58: Attribute `__name__` on type `(Unknown & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | ((...) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] zerver/tests/test_openapi.py:584:58: Attribute `__name__` on type `(Unknown & ((...) -> object) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | (@Todo(map_with_boundness: intersections with negative contributions) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | ((...) -> Unknown)` is possibly unbound
- Found 6977 diagnostics
+ Found 6976 diagnostics

sympy (https://github.com/sympy/sympy)
- error[call-non-callable] sympy/matrices/common.py:3069:20: Object of type `None` is not callable
- warning[possibly-unbound-implicit-call] sympy/matrices/common.py:3077:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] sympy/matrices/common.py:3077:24: Method `__getitem__` of type `(Unknown & ~FunctionType) | None | @Todo(list comprehension type)` is possibly unbound
- error[invalid-argument-type] sympy/matrices/common.py:3078:24: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None`
+ error[invalid-argument-type] sympy/matrices/common.py:3078:24: Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~FunctionType) | None | @Todo(list comprehension type)`
- error[not-iterable] sympy/matrices/common.py:3079:31: Object of type `Unknown | None` may not be iterable
+ error[not-iterable] sympy/matrices/common.py:3079:31: Object of type `(Unknown & ~FunctionType) | None | @Todo(list comprehension type)` may not be iterable
- error[not-iterable] sympy/matrices/common.py:3082:52: Object of type `Unknown | None` may not be iterable
+ error[not-iterable] sympy/matrices/common.py:3082:52: Object of type `(Unknown & ~FunctionType) | None | @Todo(list comprehension type)` may not be iterable
- error[call-non-callable] sympy/plotting/pygletplot/color_scheme.py:240:13: Object of type `None` is not callable
- error[call-non-callable] sympy/plotting/pygletplot/color_scheme.py:255:17: Object of type `None` is not callable
- error[call-non-callable] sympy/plotting/pygletplot/color_scheme.py:266:17: Object of type `None` is not callable
- error[call-non-callable] sympy/plotting/pygletplot/color_scheme.py:278:13: Object of type `None` is not callable
- error[call-non-callable] sympy/plotting/pygletplot/color_scheme.py:295:21: Object of type `None` is not callable
- error[call-non-callable] sympy/plotting/pygletplot/color_scheme.py:308:21: Object of type `None` is not callable
- error[call-non-callable] sympy/utilities/lambdify.py:892:23: Object of type `Literal[False]` is not callable
- error[call-non-callable] sympy/utilities/lambdify.py:892:23: Object of type `Literal[False]` is not callable
- error[call-non-callable] sympy/utilities/lambdify.py:892:23: Object of type `Literal[False]` is not callable
- Found 18553 diagnostics
+ Found 18543 diagnostics

scipy (https://github.com/scipy/scipy)
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/common/_aliases.py:387:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/common/_aliases.py:388:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[call-non-callable] scipy/_lib/array_api_compat/array_api_compat/common/_helpers.py:749:20: Object of type `None` is not callable
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/dask/array/_aliases.py:221:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/dask/array/_aliases.py:222:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_lazy.py:342:45: Argument to function `device` is incorrect: Expected `Array`, found `Array | int | float | complex`
+ warning[unused-ignore-comment] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:193:22: Unused blanket `type: ignore` directive
- warning[possibly-unbound-attribute] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:213:23: Attribute `dtype` on type `Array | int | float | complex` is possibly unbound
- warning[possibly-unbound-attribute] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:214:38: Attribute `dtype` on type `Array | int | float | complex` is possibly unbound
- error[invalid-return-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:226:12: Return type does not match returned value: expected `tuple[Array, Array]`, found `tuple[Unknown, Array | int | float | complex | Unknown] | tuple[Array | int | float | complex | Unknown, Unknown]`
- error[invalid-argument-type] scipy/integrate/_ivp/ivp.py:574:52: Argument to function `issubclass` is incorrect: Expected `type`, found `Unknown | Literal["RK45"]`
- error[call-non-callable] scipy/integrate/_quad_vec.py:381:38: Object of type `Literal["2"]` is not callable
- error[call-non-callable] scipy/integrate/_quad_vec.py:417:42: Object of type `Literal["2"]` is not callable
- error[call-non-callable] scipy/optimize/_basinhopping.py:708:9: Object of type `None` is not callable
- error[call-non-callable] scipy/optimize/_basinhopping.py:719:19: Object of type `None` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:478:26: Object of type `Literal[1]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:485:25: Object of type `Literal[2]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:573:20: Object of type `Literal[0]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:589:22: Object of type `Literal[0]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:591:20: Object of type `Literal[0]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:623:24: Object of type `Literal[0]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:639:24: Object of type `Literal[0]` is not callable
- error[call-non-callable] scipy/stats/_axis_nan_policy.py:648:24: Object of type `Literal[0]` is not callable
- error[call-non-callable] scipy/stats/_binned_statistic.py:652:24: Object of type `Literal["mean"]` is not callable
- error[invalid-argument-type] scipy/stats/_multivariate.py:783:27: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~AlwaysFalsy) | Literal[1] | tuple[Unknown, ...] | tuple[(Unknown & ~AlwaysFalsy) | Literal[1] | tuple[Unknown, ...]]`
- error[call-non-callable] scipy/stats/_sensitivity_analysis.py:629:27: Object of type `Literal["saltelli_2010"]` is not callable
- Found 7754 diagnostics
+ Found 7730 diagnostics

```
</details>


---

_Comment by @InSyncWithFoo on 2025-05-25 01:22_

Regarding the three problems outlined [here](https://github.com/astral-sh/ruff/pull/16314#issuecomment-2676178735):

* Function definition checking: Not covered in this PR.
* Arbitrary guard expressions: Only names and calls are allowed.
* `TypeGuard` narrowing logic: `TypeGuard` is not covered in this PR.

`invalid-type-guard-definition` and `TypeGuard` tests are kept, however.

This PR itself has two new issues:

* In `reveal_type(g(a))`, where `g(a)` returns a bound `TypeIs` type, the `reveal_type()` call is incorrectly recognized as a guard call, since it returns the inner `TypeIs` unchanged. I tried checking for the declared return type of `callable_ty` (if it is a literal function) using `infer_expression_types()`, but it panicked, saying that there was no `Expression` associated with the function's return type expression.
* In this particular example and no other, the constraints are not inverted in the negative case, for some reason that's beyond me:

	```py
	def guard_str(a: object) -> TypeGuard[str]: return True
	def is_int(a: object) -> TypeIs[int]: return True
	def _(a: str | int):
	    if guard_str(a):
	        # TODO: Should be `str`
	        reveal_type(a)  # revealed: str | int
	    else:
	        reveal_type(a)  # revealed: str | int
	
	    if is_int(a):
	        reveal_type(a)  # revealed: int
	    else:
            # Should be `str & ~int`!
	        reveal_type(a)  # revealed: str | int
	```

---

_Label `ty` added by @AlexWaygood on 2025-05-25 07:58_

---

_Converted to draft by @InSyncWithFoo on 2025-05-25 21:34_

---

_Comment by @carljm on 2025-05-29 00:47_

Sorry, didn't realize there was a comment here looking for feedback. (In general I don't see draft PRs in my notifications, and I assume draft PRs are not ready for review, unless I'm pinged by name for comment.)

> * In `reveal_type(g(a))`, where `g(a)` returns a bound `TypeIs` type, the `reveal_type()` call is incorrectly recognized as a guard call

I think this is a feature, not a bug. The point of `reveal_type` being an identity function is that it can be wrapped around any expression in existing code, without changing the meaning of the code. This should be true in a type-is check call just as much as anywhere else. So I would not make any effort to adjust this behavior.

> * In this particular example and no other, the constraints are not inverted in the negative case

Happy to dig into this tomorrow, if this is the only issue blocking this PR.

---

_Comment by @InSyncWithFoo on 2025-05-29 01:07_

@carljm Actually, I think I got them:

* Reporting `invalid-type-guard-call` may lead to multiple false positives, since such calls are not always used for their narrowing effects. Now ty will only report this lint for `TypeIs` function calls that have no arguments.
* The second problem was a simple logic error that have since been fixed.

The only blocking problem left is that [the fuzzer says this PR causes new panics](https://github.com/astral-sh/ruff/actions/runs/15313404253/job/43082745392?pr=18294). Here's the generated snippet:

```python
for name_3 in something:

    class name_5[**name_4](name_3):
        pass
else:

    class name_3[name_5](b'', name_5=name_1):
        pass
    name_5()
for name_2 in something:
    pass
else:

    @name_3
    def name_3():
        pass
(name_3 := f'') and (name_1 := name_2)
assert name_3
```

I reckon this has something to do with the `evaluate_expr_named()` method in `narrow.rs`, but I'm not sure.

Otherwise, this is ready for review.

---

_Marked ready for review by @InSyncWithFoo on 2025-05-29 01:07_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-05-29 01:07_

---

_Comment by @carljm on 2025-06-06 00:24_

Sorry I didn't get to this sooner; been focused on getting the big attribute-assignment-narrowing PR landed; it's now landed!

It looks like this currently has a build issue and needs conflict resolution with main. @InSyncWithFoo let me know if you're able to get it back into shape; if so I can look into the new panic right away.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:130 on 2025-06-06 00:27_

TODO comment for an error here?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:162 on 2025-06-06 00:29_

no, this error should I think go away when we support splatted arguments in calls
```suggestion
    # TODO: no error, once we support splatted call args
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:174 on 2025-06-06 00:30_

```suggestion
        # TODO: Should be `tuple[int, str]`
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:176 on 2025-06-06 00:33_

Why is this test in "invalid calls" section instead of "narrowing" section?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:230 on 2025-06-06 00:46_

I am a bit hesitant to land this PR with this TODO unfixed, because it suggests that the approach used for preserving context in the `TypeIs` type may need significant re-working in order to address this TODO.

But maybe it won't; maybe it will just require storing some additional context (probably the visible definitions at the point of the `TypeIs` call).

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:801 on 2025-06-06 00:47_

The problem with this approach of simply commenting out the `static_assert` is that the test won't fail when we add `TypeGuard` support, and we may forget to uncomment it.

I actually think you should be able to uncomment this as-is and it will pass, since `TypeGuard` is currently inferred as a Todo type.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1036 on 2025-06-06 00:50_

Does this end up returning just the normalized nested type, no longer a `TypeIs` type? Seems like we need to wrap this in a `type_is.with_ty(...)` call?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:355 on 2025-06-06 00:53_

Maybe also test that it's a subtype of `int`, too

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9244 on 2025-06-06 00:59_

Given the scope and the symbol ID, we should be able to look up the symbol name? Also, I'm not sure we need to include the symbol name in the display of a bound `TypeIs` anyway. So I would remove the `String` here.

Also, I think this information will be insufficient to properly handle the case where the symbol may be re-bound. I think we will need to store the visible definitions of the symbol here to support that.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9239 on 2025-06-06 01:00_

I don't think we need/want a `return_ref` on a field that is a `Type` -- `Type` is small and `Copy`

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9240 on 2025-06-06 01:00_

nit: would prefer a more descriptive name here, e.g. `narrowed_type`. (We've also been moving away from using variables named `ty`)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5596 on 2025-06-06 01:08_

What kind of handling would we expect here? I don't think it's valid to expect narrowing from returning a union where one of the elements is a `TypeIs`...

---

_@carljm reviewed on 2025-06-06 01:14_

Some initial review comments. Thanks for your work on this!

---

_Comment by @InSyncWithFoo on 2025-06-06 01:54_

@carljm Thanks for the heads up. I probably won't be able to work on this earlier than Sunday (June 8th). Also, the reason for the panic was that the same `ExprName` in a named expression had two corresponding IDs (50 and 52, to be exact); I'm not sure why that is even the case, though.

---

_@InSyncWithFoo reviewed on 2025-06-06 01:58_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:176 on 2025-06-06 01:58_

`a[0]` wasn't considered a valid narrowing target. Well, at least not until I learned about #17643.

---

_Closed by @InSyncWithFoo on 2025-06-09 12:26_

---

_Branch deleted on 2025-06-09 12:26_

---

_Comment by @InSyncWithFoo on 2025-06-09 12:31_

This PR is unsalvageable; I submitted #18589 as replacement.

---

_@InSyncWithFoo reviewed on 2025-06-09 12:33_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/infer.rs`:5596 on 2025-06-09 12:33_

True. As for intersections, `TypeIs[a, str] & TypeIs[b, int]` should have narrowing effect, but I'm not sure if that type is constructable from Python code.

---
