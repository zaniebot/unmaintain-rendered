```yaml
number: 18589
title: "[ty] Add partial support for `TypeIs`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-typeis
created_at: 2025-06-09T12:30:26Z
updated_at: 2025-06-14T16:48:44Z
url: https://github.com/astral-sh/ruff/pull/18589
synced_at: 2026-01-12T15:56:22Z
```

# [ty] Add partial support for `TypeIs`

---

_@InSyncWithFoo_

## Summary

Redo of #16314 and #18294, part of [#117](https://github.com/astral-sh/ty/issues/117).

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

Delayed usages of a bound type has no effect, however:

```python
b = f(a)

if b:
	reveal_type(a)     # str
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

_Review requested from @carljm by @InSyncWithFoo on 2025-06-09 12:30_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-06-09 12:30_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-06-09 12:30_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-06-09 12:30_

---

_Comment by @github-actions[bot] on 2025-06-09 12:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- error[call-non-callable] bidict/_iter.py:50:34: Object of type `None` is not callable
- Found 15 diagnostics
+ Found 14 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- error[call-non-callable] nion/utils/Binding.py:262:32: Object of type `None` is not callable
- Found 17 diagnostics
+ Found 16 diagnostics

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
- error[invalid-argument-type] beartype/bite/_infermain.py:244:36: Argument to function `infer_hint_callable` is incorrect: Expected `(...) -> Unknown`, found `~type`
+ error[invalid-type-guard-call] beartype/door/_cls/doorsuper.py:667:27: Type guard call does not have a target
- Found 583 diagnostics
+ Found 581 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[call-non-callable] pyinstrument/middleware.py:50:13: Object of type `None` is not callable
- Found 87 diagnostics
+ Found 86 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-return-type] src/jinja2/async_utils.py:70:12: Return type does not match returned value: expected `V`, found `Awaitable[V] | V`
+ warning[redundant-cast] src/jinja2/async_utils.py:68:22: Value is already of type `Awaitable[V]`

anyio (https://github.com/agronholm/anyio)
- error[invalid-argument-type] src/anyio/from_thread.py:237:35: Argument to bound method `set_result` is incorrect: Expected `T_Retval`, found `@Todo(generic `typing.Awaitable` type) | Awaitable[T_Retval] | T_Retval`
- Found 109 diagnostics
+ Found 108 diagnostics

operator (https://github.com/canonical/operator)
- error[invalid-argument-type] ops/charm.py:1621:41: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type`
+ warning[unused-ignore-comment] ops/charm.py:1635:55: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] ops/model.py:1938:69: Unused blanket `type: ignore` directive
- error[invalid-argument-type] ops/model.py:1932:45: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `~type`
- error[no-matching-overload] ops/model.py:1935:22: No overload of function `asdict` matches arguments
+ warning[unused-ignore-comment] ops/model.py:1941:90: Unused blanket `type: ignore` directive

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-argument-type] pydantic/_internal/_fields.py:281:61: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type`
- error[call-non-callable] pydantic/fields.py:1193:13: Object of type `None` is not callable
- Found 762 diagnostics
+ Found 760 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] src/graphql/execution/execute.py:1901:9: Argument to function `experimental_execute_incrementally` is incorrect: Expected `((Any, /) -> bool) | None`, found `bool | None | (def assume_not_awaitable(_value: Any) -> bool)`
- error[call-non-callable] src/graphql/execution/execute.py:2121:16: Object of type `None` is not callable
- error[invalid-argument-type] src/graphql/graphql.py:147:9: Argument to function `graphql_impl` is incorrect: Expected `((Any, /) -> bool) | None`, found `bool | None | (def assume_not_awaitable(_value: Any) -> bool)`
- Found 441 diagnostics
+ Found 438 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-argument-type] src/trio/_core/_tests/test_ki.py:685:43: Argument to function `_consume_async_generator` is incorrect: Expected `AsyncGenerator[None, None]`, found `object`
- error[unresolved-attribute] src/trio/_core/_tests/test_ki.py:690:13: Type `object` has no attribute `send`
- Found 1103 diagnostics
+ Found 1101 diagnostics

kopf (https://github.com/nolar/kopf)
- error[call-non-callable] kopf/_core/intents/registries.py:470:16: Object of type `str` is not callable
- error[call-non-callable] kopf/_core/intents/registries.py:470:16: Object of type `MetaFilterToken` is not callable
- Found 157 diagnostics
+ Found 155 diagnostics

kornia (https://github.com/kornia/kornia)
- error[unresolved-attribute] kornia/utils/helpers.py:116:24: Type `(...) -> Any` has no attribute `__name__`
- Found 949 diagnostics
+ Found 948 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-argument-type] strawberry/cli/commands/codegen.py:20:24: Argument to function `issubclass` is incorrect: Expected `type`, found `object`
- error[call-non-callable] strawberry/cli/utils/__init__.py:23:29: Object of type `object` is not callable
- error[invalid-argument-type] strawberry/experimental/pydantic/conversion.py:106:37: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type & ~<Protocol with members 'to_pydantic'>`
- error[call-non-callable] strawberry/extensions/query_depth_limiter.py:149:17: Object of type `None` is not callable
- error[call-non-callable] strawberry/types/field.py:134:38: Object of type `object` is not callable
- Found 435 diagnostics
+ Found 430 diagnostics

nox (https://github.com/wntrblm/nox)
- error[call-non-callable] nox/_option_set.py:207:21: Object of type `bool` is not callable
- Found 33 diagnostics
+ Found 32 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-argument-type] src/hydra_zen/_launch.py:397:44: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `@Todo(Support for `typing.TypeAlias`) | Mapping[str, Any]`
- error[invalid-argument-type] src/hydra_zen/_launch.py:478:50: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `@Todo(Support for `typing.TypeAlias`) | Mapping[str, Any]`
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:1133:34: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `(~None & ~<Protocol with members '__iter__'> & ~type) | (Unknown & ~type)`
- error[invalid-assignment] src/hydra_zen/third_party/beartype.py:125:9: Implicit shadowing of function `__init__`
- warning[redundant-cast] src/hydra_zen/third_party/beartype.py:130:12: Value is already of type `_T`
- warning[redundant-cast] src/hydra_zen/third_party/pydantic.py:111:16: Value is already of type `_T`
- error[call-non-callable] src/hydra_zen/wrapper/_implementations.py:1610:20: Object of type `None` is not callable
+ warning[unused-ignore-comment] src/hydra_zen/wrapper/_implementations.py:1600:75: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/hydra_zen/wrapper/_implementations.py:1605:80: Unused blanket `type: ignore` directive
- Found 609 diagnostics
+ Found 604 diagnostics

websockets (https://github.com/aaugustin/websockets)
- error[call-non-callable] src/websockets/legacy/server.py:633:29: Object of type `None` is not callable
- Found 120 diagnostics
+ Found 119 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[call-non-callable] ignite/handlers/base_logger.py:48:20: Object of type `list[str]` is not callable
- error[not-iterable] ignite/handlers/base_logger.py:52:29: Object of type `list[str] | ((str, Unknown, /) -> bool)` may not be iterable
- Found 2228 diagnostics
+ Found 2226 diagnostics

optuna (https://github.com/optuna/optuna)
- error[unresolved-attribute] tests/test_deprecated.py:56:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_deprecated.py:72:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_deprecated.py:192:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_experimental.py:51:12: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_experimental.py:64:12: Type `(...) -> Unknown` has no attribute `__name__`
- Found 1060 diagnostics
+ Found 1055 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[call-non-callable] src/poetry/utils/cache.py:147:21: Object of type `T` is not callable
- error[invalid-return-type] src/poetry/utils/cache.py:149:16: Return type does not match returned value: expected `T`, found `(Unknown & ~None) | Unknown | T | (() -> T)`
+ error[invalid-return-type] src/poetry/utils/cache.py:149:16: Return type does not match returned value: expected `T`, found `(Unknown & ~None) | @Todo(Type::Intersection.call()) | (T & ~((...) -> object)) | ((() -> T) & ~((...) -> object))`
- Found 1028 diagnostics
+ Found 1027 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[unresolved-attribute] src/schemathesis/hooks.py:269:16: Type `str | ((...) -> Unknown)` has no attribute `__name__`
+ error[unresolved-attribute] src/schemathesis/hooks.py:269:16: Type `(str & ((...) -> object)) | (((...) -> Unknown) & ((...) -> object))` has no attribute `__name__`
- error[call-non-callable] src/schemathesis/pytest/lazy.py:51:39: Object of type `None` is not callable
- Found 418 diagnostics
+ Found 417 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[call-non-callable] tornado/escape.py:352:28: Object of type `str` is not callable
- Found 326 diagnostics
+ Found 325 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[invalid-type-form] cki_lib/messagequeue.py:373:36: Variable of type `def callable(obj: object, /) -> @Todo(`TypeIs[]` special form)` is not allowed in a type expression
+ error[invalid-type-form] cki_lib/messagequeue.py:373:36: Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression

vision (https://github.com/pytorch/vision)
- error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `Unknown | Sequence[int] | int | ((int, /) -> Sequence[int] | int)`
+ error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `@Todo(Type::Intersection.call()) | (Sequence[int] & ~((...) -> object)) | (int & ~((...) -> object)) | (((int, /) -> Sequence[int] | int) & ~((...) -> object))`
- error[call-non-callable] test/datasets_utils.py:835:57: Object of type `Sequence[int]` is not callable
- error[call-non-callable] test/datasets_utils.py:835:57: Object of type `int` is not callable
- error[call-non-callable] test/datasets_utils.py:956:57: Object of type `Sequence[int]` is not callable
- error[call-non-callable] test/datasets_utils.py:956:57: Object of type `int` is not callable
- error[call-non-callable] torchvision/models/_utils.py:201:43: Object of type `WeightsEnum` is not callable
- error[call-non-callable] torchvision/models/_utils.py:201:43: Object of type `None` is not callable
- error[invalid-return-type] torchvision/transforms/v2/_utils.py:149:16: Return type does not match returned value: expected `(Any, /) -> Any`, found `(str & ~Literal["default"]) | ((Any, /) -> Any) | None`
- Found 1546 diagnostics
+ Found 1539 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[call-non-callable] src/werkzeug/utils.py:505:19: Object of type `None` is not callable
- error[call-non-callable] src/werkzeug/utils.py:505:19: Object of type `int` is not callable
- error[unsupported-operator] src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `int | ((str | None, /) -> int | None) | (Unknown & ~None)` with `Literal[0]`
+ error[unsupported-operator] src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object)) | (@Todo(Type::Intersection.call()) & ~None)` with `Literal[0]`
- error[invalid-assignment] src/werkzeug/utils.py:512:9: Object of type `int | ((str | None, /) -> int | None) | (Unknown & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ error[invalid-assignment] src/werkzeug/utils.py:512:9: Object of type `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object)) | (@Todo(Type::Intersection.call()) & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ warning[unused-ignore-comment] src/werkzeug/utils.py:513:45: Unused blanket `type: ignore` directive
- Found 451 diagnostics
+ Found 450 diagnostics

altair (https://github.com/vega/altair)
+ error[invalid-argument-type] altair/utils/data.py:362:35: Argument to function `sanitize_geo_interface` is incorrect: Expected `MutableMapping[Any, Any]`, found `Series[Unknown] | @Todo(map_with_boundness: intersections with negative contributions)`
- error[invalid-argument-type] altair/utils/data.py:361:37: Argument to function `sanitize_pandas_dataframe` is incorrect: Argument type `SupportsGeoInterface` does not satisfy upper bound of type variable `_PandasDataFrameT`
- error[call-non-callable] altair/utils/data.py:430:22: Object of type `None` is not callable
- error[not-iterable] altair/utils/schemapi.py:1132:25: Object of type `Literal[False] | Iterable[Any]` may not be iterable
- error[invalid-argument-type] altair/vegalite/v5/api.py:254:30: Argument to function `_dataset_name` is incorrect: Expected `dict[str, Any] | list[Unknown] | InlineDataset`, found `Unknown | UndefinedType`
- error[invalid-assignment] altair/vegalite/v5/api.py:3863:13: Object of type `Facet | (@Todo(Support for `typing.TypeAlias`) & ~str) | (UndefinedType & ~str)` is not assignable to `Facet | FacetMapping`
- error[invalid-argument-type] altair/vegalite/v5/api.py:4200:44: Argument to function `_repeat_names` is incorrect: Expected `list[str] | LayerRepeatMapping | RepeatMapping`, found `@Todo(Support for `typing.TypeAlias`) | UndefinedType`
- Found 1296 diagnostics
+ Found 1291 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[call-non-callable] scrapy/downloadermiddlewares/retry.py:109:22: Object of type `str` is not callable
- error[call-non-callable] scrapy/downloadermiddlewares/retry.py:109:22: Object of type `Exception` is not callable
- error[call-non-callable] scrapy/spiders/__init__.py:180:50: Object of type `None` is not callable
- error[invalid-return-type] scrapy/spiders/crawl.py:51:16: Return type does not match returned value: expected `((...) -> Unknown) | None`, found `((...) -> Unknown) | str | None`
- error[invalid-argument-type] scrapy/utils/decorators.py:43:21: Argument to function `deco` is incorrect: Expected `(...) -> Unknown`, found `Any | None`
- error[no-matching-overload] scrapy/utils/defer.py:387:36: No overload of function `ensure_future` matches arguments
- warning[possibly-unbound-attribute] tests/test_utils_console.py:28:16: Attribute `__name__` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_utils_console.py:34:16: Attribute `__name__` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
- Found 1328 diagnostics
+ Found 1320 diagnostics

apprise (https://github.com/caronc/apprise)
- error[call-non-callable] apprise/manager.py:241:64: Object of type `None` is not callable
- Found 3691 diagnostics
+ Found 3690 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- error[invalid-argument-type] discord/ext/commands/converter.py:1322:54: Argument to function `issubclass` is incorrect: Expected `type`, found `(Any & ~<class 'bool'>) | Any | None`
- warning[possibly-unbound-attribute] discord/ext/commands/converter.py:1340:20: Attribute `__name__` on type `(Any & ~<class 'bool'> & ~Converter[Unknown]) | (Any & ~Converter[Unknown]) | None` is possibly unbound
+ warning[possibly-unbound-attribute] discord/ext/commands/converter.py:1340:20: Attribute `__name__` on type `(Any & ~<class 'bool'> & ~type[Any] & ~Converter[Unknown]) | (Any & ~<class 'bool'> & ~Converter[Unknown]) | (Any & ~type[Any] & ~Converter[Unknown]) | None | (Any & ~Converter[Unknown])` is possibly unbound
- Found 620 diagnostics
+ Found 619 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] src/_pytest/fixtures.py:1022:42: Argument to function `_eval_scope_callable` is incorrect: Expected `(str, Config, /) -> Unknown`, found `Scope | (Unknown & ~None) | ((str, Config, /) -> Unknown)`
- error[invalid-argument-type] src/_pytest/fixtures.py:1386:9: Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | Sequence[object] | ((Any, /) -> object) | tuple[Unknown, ...]`
- error[invalid-argument-type] src/_pytest/fixtures.py:1386:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[object] | ((Any, /) -> object)`
+ error[invalid-argument-type] src/_pytest/fixtures.py:1386:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Sequence[object] & ~((...) -> object)) | (((Any, /) -> object) & ~((...) -> object))`
- error[invalid-argument-type] src/_pytest/python.py:1386:13: Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | Iterable[object] | ((Any, /) -> object)`
- Found 775 diagnostics
+ Found 772 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[call-non-callable] openlibrary/plugins/worksearch/code.py:230:34: Object of type `str` is not callable
- Found 725 diagnostics
+ Found 724 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[call-non-callable] aiohttp/client.py:838:23: Object of type `bool` is not callable
- error[unresolved-attribute] aiohttp/helpers.py:808:34: Type `ErrorableProtocol` has no attribute `done`
- Found 174 diagnostics
+ Found 172 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-return-type] src/bokeh/core/property/bases.py:185:20: Return type does not match returned value: expected `T`, found `(() -> T) | T`
+ error[invalid-return-type] src/bokeh/core/property/bases.py:185:20: Return type does not match returned value: expected `T`, found `((() -> T) & ~((...) -> object)) | (T & ~((...) -> object))`
- error[invalid-return-type] src/bokeh/core/property/bases.py:188:24: Return type does not match returned value: expected `T`, found `(() -> T) | T`
+ error[invalid-return-type] src/bokeh/core/property/bases.py:188:24: Return type does not match returned value: expected `T`, found `((() -> T) & ((...) -> object)) | (T & ((...) -> object))`
- error[call-non-callable] src/bokeh/core/property/bases.py:189:20: Object of type `T` is not callable
- error[call-non-callable] src/bokeh/io/notebook.py:564:18: Object of type `str` is not callable
- error[call-non-callable] src/bokeh/io/notebook.py:576:15: Object of type `str` is not callable
- error[call-non-callable] src/bokeh/model/util.py:108:61: Object of type `None` is not callable
- error[call-non-callable] src/bokeh/plotting/graph.py:120:28: Object of type `dict[int | str, Sequence[int | float]]` is not callable
- Found 938 diagnostics
+ Found 933 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/core/display.py:135:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:147:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:159:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:171:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
- error[invalid-argument-type] static_frame/core/display.py:183:27: Argument to function `issubclass` is incorrect: Expected `type`, found `type | Unknown | dtype[Any]`
+ warning[unused-ignore-comment] static_frame/core/frame.py:8197:45: Unused blanket `type: ignore` directive
- Found 1921 diagnostics
+ Found 1917 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-return-type] lib/streamlit/runtime/metrics_util.py:262:16: Return type does not match returned value: expected `str`, found `object`
- error[invalid-argument-type] lib/streamlit/testing/v1/element_tree.py:1664:29: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `object`
- error[call-non-callable] lib/streamlit/vendor/pympler/asizeof.py:269:12: Object of type `None` is not callable
- error[call-non-callable] lib/streamlit/vendor/pympler/asizeof.py:275:12: Object of type `None` is not callable
- error[call-non-callable] lib/streamlit/vendor/pympler/asizeof.py:281:12: Object of type `None` is not callable
- Found 3292 diagnostics
+ Found 3287 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[invalid-assignment] sphinx/environment/__init__.py:392:13: Object of type `str | ((Node, /) -> bool)` is not assignable to `Literal[False] | ((Node, /) -> bool)`
+ error[invalid-assignment] sphinx/environment/__init__.py:392:13: Object of type `(str & ((...) -> object)) | (((Node, /) -> bool) & ((...) -> object))` is not assignable to `Literal[False] | ((Node, /) -> bool)`
- error[call-non-callable] sphinx/util/inspect.py:102:19: Object of type `None` is not callable
- error[unresolved-attribute] sphinx/util/inspect.py:296:33: Type `object` has no attribute `__self__`
- Found 649 diagnostics
+ Found 647 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[call-non-callable] mesonbuild/compilers/compilers.py:1290:26: Object of type `None` is not callable
- error[call-non-callable] mesonbuild/compilers/compilers.py:1290:26: Object of type `CompilerArgs` is not callable
- error[call-non-callable] mesonbuild/compilers/compilers.py:1290:26: Object of type `list[str]` is not callable
- error[call-non-callable] mesonbuild/compilers/vala.py:152:26: Object of type `None` is not callable
- error[call-non-callable] mesonbuild/compilers/vala.py:152:26: Object of type `CompilerArgs` is not callable
- error[call-non-callable] mesonbuild/compilers/vala.py:152:26: Object of type `list[str]` is not callable
- Found 1310 diagnostics
+ Found 1304 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[call-non-callable] cloudinit/sources/helpers/azure.py:171:9: Object of type `None` is not callable
- Found 750 diagnostics
+ Found 749 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[invalid-argument-type] src/prefect/_experimental/bundles/__init__.py:149:29: Argument to function `run` is incorrect: Expected `Coroutine[Any, Any, Unknown]`, found `Unknown | State[Any] | None | Coroutine[Any, Any, Unknown | State[Any] | None] | Generator[Unknown, None, None] | AsyncGenerator[Unknown, None]`
- error[call-non-callable] src/prefect/client/orchestration/__init__.py:449:36: Object of type `None` is not callable
- error[call-non-callable] src/prefect/client/orchestration/__init__.py:1299:36: Object of type `None` is not callable
- error[invalid-argument-type] src/prefect/concurrency/v1/sync.py:63:58: Argument to function `emit_concurrency_acquisition_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `Unknown | Coroutine[Any, Any, Unknown]`
+ error[invalid-argument-type] src/prefect/concurrency/v1/sync.py:63:58: Argument to function `emit_concurrency_acquisition_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `(Unknown & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Unknown] & ~Coroutine[Any, Any, Any])`
- error[invalid-argument-type] src/prefect/concurrency/v1/sync.py:72:41: Argument to function `emit_concurrency_release_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `Unknown | Coroutine[Any, Any, Unknown]`
+ error[invalid-argument-type] src/prefect/concurrency/v1/sync.py:72:41: Argument to function `emit_concurrency_release_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `(Unknown & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Unknown] & ~Coroutine[Any, Any, Any])`
- warning[possibly-unbound-attribute] src/prefect/flows.py:290:34: Attribute `__name__` on type `(((...) -> Unknown) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ~classmethod & ~staticmethod) | (((...) -> Unknown) & ~staticmethod) | ((...) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] src/prefect/flows.py:290:34: Attribute `__name__` on type `(((...) -> Unknown) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> Unknown) & ((...) -> object) & ~staticmethod) | (((...) -> Unknown) & ((...) -> object))` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/flows.py:407:68: Attribute `__name__` on type `(((...) -> Unknown) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ~classmethod & ~staticmethod) | (((...) -> Unknown) & ~staticmethod) | ((...) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] src/prefect/flows.py:407:68: Attribute `__name__` on type `(((...) -> Unknown) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> Unknown) & ((...) -> object) & ~staticmethod) | (((...) -> Unknown) & ((...) -> object))` is possibly unbound
- error[invalid-argument-type] src/prefect/input/run_input.py:426:23: Argument to function `issubclass` is incorrect: Expected `type`, found `@Todo(unsupported type[X] special form) | BaseModel`
- error[invalid-argument-type] src/prefect/input/run_input.py:428:25: Argument to function `issubclass` is incorrect: Expected `type`, found `(@Todo(unsupported type[X] special form) & ~type[RunInput]) | (BaseModel & ~type[RunInput])`
- error[call-non-callable] src/prefect/tasks.py:550:40: Object of type `int` is not callable
- error[call-non-callable] src/prefect/tasks.py:550:40: Object of type `float` is not callable
- error[call-non-callable] src/prefect/tasks.py:550:40: Object of type `list[int | float]` is not callable
- Found 4451 diagnostics
+ Found 4443 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-argument-type] zerver/lib/event_schema.py:462:54: Argument to function `issubclass` is incorrect: Expected `type`, found `type | UnionType`
- warning[possibly-unbound-attribute] zerver/tests/test_openapi.py:584:58: Attribute `__name__` on type `(Unknown & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | ((...) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] zerver/tests/test_openapi.py:584:58: Attribute `__name__` on type `(Unknown & ((...) -> object) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | (@Todo(map_with_boundness: intersections with negative contributions) & ~(def get_events(request: HttpRequest, user_profile: UserProfile) -> HttpResponse)) | ((...) -> Unknown)` is possibly unbound
- Found 7743 diagnostics
+ Found 7742 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- error[call-non-callable] sklearn/cluster/_agglomerative.py:562:17: Object of type `Literal["euclidean"]` is not callable
- warning[possibly-unbound-attribute] sklearn/externals/array_api_compat/common/_aliases.py:387:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/externals/array_api_compat/common/_aliases.py:388:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[call-non-callable] sklearn/externals/array_api_compat/common/_helpers.py:749:20: Object of type `None` is not callable
- warning[possibly-unbound-attribute] sklearn/externals/array_api_compat/dask/array/_aliases.py:221:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/externals/array_api_compat/dask/array/_aliases.py:222:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:141:16: Type `ModuleType` has no attribute `map_blocks`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:157:16: Type `ModuleType` has no attribute `where`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:162:17: Type `ModuleType` has no attribute `result_type`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:164:19: Type `ModuleType` has no attribute `full_like`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:166:19: Type `ModuleType` has no attribute `astype`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:170:17: Type `ModuleType` has no attribute `result_type`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_funcs.py:171:15: Type `ModuleType` has no attribute `empty_like`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_lazy.py:251:13: Type `ModuleType` has no attribute `from_delayed`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_lazy.py:337:45: Argument to function `device` is incorrect: Expected `Array`, found `Array | int | float | complex`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:89:9: Type `ModuleType` has no attribute `testing`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:93:9: Type `ModuleType` has no attribute `testing`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:114:36: Type `ModuleType` has no attribute `asarray`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:114:62: Type `ModuleType` has no attribute `Device`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:115:37: Type `ModuleType` has no attribute `asarray`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:115:64: Type `ModuleType` has no attribute `Device`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:169:9: Type `ModuleType` has no attribute `testing`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:173:9: Type `ModuleType` has no attribute `testing`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:188:36: Type `ModuleType` has no attribute `asarray`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:188:62: Type `ModuleType` has no attribute `Device`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:189:37: Type `ModuleType` has no attribute `asarray`
- error[unresolved-attribute] sklearn/externals/array_api_extra/_lib/_testing.py:189:64: Type `ModuleType` has no attribute `Device`
- warning[possibly-unbound-attribute] sklearn/externals/array_api_extra/_lib/_utils/_helpers.py:185:23: Attribute `dtype` on type `Array | int | float | complex` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/externals/array_api_extra/_lib/_utils/_helpers.py:186:38: Attribute `dtype` on type `Array | int | float | complex` is possibly unbound
- error[invalid-return-type] sklearn/externals/array_api_extra/_lib/_utils/_helpers.py:198:12: Return type does not match returned value: expected `tuple[Array, Array]`, found `tuple[Unknown, Array | int | float | complex | Unknown] | tuple[Array | int | float | complex | Unknown, Unknown]`
- warning[possibly-unbound-attribute] sklearn/metrics/pairwise.py:600:21: Attribute `nnz` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/metrics/pairwise.py:600:37: Attribute `shape` on type `Unknown | None` is possibly unbound
- error[call-non-callable] sklearn/metrics/pairwise.py:1375:28: Object of type `Literal["euclidean"]` is not callable
- Found 2519 diagnostics
+ Found 2486 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-type-form] ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:20: Variable of type `def callable(obj: object, /) -> @Todo(`TypeIs[]` special form)` is not allowed in a type expression
+ error[invalid-type-form] ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:20: Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression

scipy (https://github.com/scipy/scipy)
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/common/_aliases.py:387:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/common/_aliases.py:388:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[call-non-callable] scipy/_lib/array_api_compat/array_api_compat/common/_helpers.py:749:20: Object of type `None` is not callable
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/dask/array/_aliases.py:221:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-attribute] scipy/_lib/array_api_compat/array_api_compat/dask/array/_aliases.py:222:43: Attribute `shape` on type `int | float | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_lazy.py:342:45: Argument to function `device` is incorrect: Expected `Array`, found `Array | int | float | complex`
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
- Found 7607 diagnostics
+ Found 7582 diagnostics

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
- Found 18566 diagnostics
+ Found 18556 diagnostics

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-06-09 12:35_

---

_Converted to draft by @InSyncWithFoo on 2025-06-09 12:50_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:882 on 2025-06-09 18:41_

It doesn't seem like we need a TODO comment here. The current assertion is correct, and passes, albeit for the wrong reason. There's nothing to change in this test.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:411 on 2025-06-09 18:41_

Same here - doesn't seem like there's any need for a TODO here.

---

_@carljm reviewed on 2025-06-10 22:19_

I pushed a commit that fixes the fuzzer-found panic here; it's a pre-existing issue that crops up here due to extra cycles created by the extra type inference now done in `evaluate_name_expr`.

Otherwise, looks like some comments on the previous PR aren't yet addressed here? But I realize this is still in draft.

---

_Comment by @InSyncWithFoo on 2025-06-11 06:25_

@carljm Thanks for looking into the panic. The PR is ready for a full review now.

---

_Marked ready for review by @InSyncWithFoo on 2025-06-11 06:25_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-06-11 06:25_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:222 on 2025-06-11 18:51_

The other useful thing to test here would be `reveal_type(a[0])` -- that's easier to support (and I think might already work?) than the indirect inference of the tuple type you demonstrate here

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:224 on 2025-06-11 18:53_

I think we should avoid display types that look like something you could spell in a type annotation, but aren't actually valid when spelled that way.

`TypeIs` takes a single type parameter, so we shouldn't display it as if it takes two.

I don't think showing the place-name that it is "bound" to is necessary; it would be fine for this to just be displayed as `TypeIs[int]`

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:214 on 2025-06-11 18:55_

I think we do want to support this, and it would be easy to miss updating this text -- the usual way we handle this is to write the prose for what we aim to support, and use TODO comments where we aren't there yet:
```suggestion
Indirect usage is supported within the same scope:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:260 on 2025-06-11 18:55_

```suggestion
    if c:
        # TODO should be `int`
        reveal_type(a)  # revealed: str | int
    else:
        # TODO should be `str & ~int`
        reveal_type(a)  # revealed: str | int
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:265 on 2025-06-11 18:56_

````suggestion
```

Further writes to the narrowed place invalidate the narrowing:

```py
````

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:883 on 2025-06-11 18:59_

These tests are good -- let's also add tests showing them not assignable to some other arbitrary type. It's always good to have both positive and negative tests, to demonstrate that the positive ones are passing for the right reason (not e.g. because we are just always treating it as dynamic, which actually is the case for TypeGuard still)
```suggestion
static_assert(is_assignable_to(TypeGuard[Unknown], bool))
static_assert(is_assignable_to(TypeIs[Any], bool))

# TODO no error
static_assert(not is_assignable_to(TypeGuard[Unknown], str))  # error: [static-assert-error]
static_assert(not is_assignable_to(TypeIs[Any], str))
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:412 on 2025-06-11 18:59_

```suggestion
static_assert(not is_disjoint_from(bool, TypeGuard[str]))
static_assert(not is_disjoint_from(bool, TypeIs[str]))

# TODO no error
static_assert(is_disjoint_from(str, TypeGuard[str]))  # error: [static-assert-error]
static_assert(is_disjoint_from(str, TypeIs[str]))
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8338 on 2025-06-11 19:05_

```suggestion
    /// The ID of the scope to which the place belongs
    /// and the ID of the place itself within that scope.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8520 on 2025-06-11 19:07_

Hmm, can you say more why you picked `return_type` here, instead of e.g. `narrowed_type` which I'd suggested in the last PR?

I think it's not a big deal, but I'm not really clear in what sense this is a "return type"

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:220 on 2025-06-11 19:09_

As mentioned above, I think we should remove this -- wrongly gives the impression that `TypeIs` can accept two parameters.

If you feel strongly about including this, I think it would be better if we use a syntax that clearly is not a valid annotation. E.g. something like `TypeIs[str](a)` could perhaps work?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5759 on 2025-06-11 19:12_

I'm fine with more smaller PRs -- but also, given the general `Place` support, it seems like this would be a quite easy TODO to fix now?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5763 on 2025-06-11 19:12_

I think we should just eliminate this TODO comment, unless you have concrete ideas of what should be supported that isn't currently.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:736 on 2025-06-11 19:15_

Same with this comment -- let's remove unless it is clear what support is missing that we actually need

---

_@carljm reviewed on 2025-06-11 19:15_

Looking good! A few comments.

---

_@InSyncWithFoo reviewed on 2025-06-11 20:28_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:222 on 2025-06-11 20:28_

It doesn't work, actually; we don't support subscripting unions yet.

---

_@InSyncWithFoo reviewed on 2025-06-11 20:28_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types.rs`:8520 on 2025-06-11 20:28_

Its official name is <i>[`TypeIs` return type](https://typing.python.org/en/latest/spec/narrowing.html#typeis)</i>. I think I also call it `guarded_type` elsewhere.

---

_@InSyncWithFoo reviewed on 2025-06-11 20:28_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/display.rs`:220 on 2025-06-11 20:28_

I do feel that the name should be included, especially if we are going to support indirect usages later. I'll change the format to `TypeIs[str](a)`, but `TypeIs[a, str]` still looks better, in my opinion. Would a specialized error message helps?

---

_@InSyncWithFoo reviewed on 2025-06-11 20:39_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/infer.rs`:5759 on 2025-06-11 20:39_

I'm not sure what I'm doing wrong, but calling `PlaceExpr::try_from()` on `a[0]` returns `None`.

---

_@carljm reviewed on 2025-06-12 03:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8520 on 2025-06-12 03:51_

Ok that's fair! Fine to leave this as is

---

_@carljm reviewed on 2025-06-12 04:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5759 on 2025-06-12 04:24_

Tried it locally a bit, it seems like `PlaceExpr::try_from()` is returning the `PlaceExpr` just fine, but the problem is we don't actually support narrowing on non-Name Places just yet, that's still coming with https://github.com/astral-sh/ruff/pull/17643 -- so never mind this comment, we can leave this as TODO for now.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:220 on 2025-06-12 04:52_

`TypeIs(a)[str]` look any better to you?

---

_@carljm reviewed on 2025-06-12 04:52_

---

_@carljm reviewed on 2025-06-12 04:53_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8348 on 2025-06-12 04:53_

This TODO is easy to address since `PlaceExpr` implements `Display`. But we can also leave it for now.

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/display.rs`:220 on 2025-06-12 08:08_

Not better than `TypeIs[a, str]`, I'm afraid, and equally confusing as `TypeIs[str](a)`. How about `TypeIs[a@<place-id>, str]`?

---

_@InSyncWithFoo reviewed on 2025-06-12 08:08_

---

_@AlexWaygood reviewed on 2025-06-12 09:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:220 on 2025-06-12 09:35_

I agree with Carl that putting multiple parameters in the brackets looks confusing. The type display doesn't need to be legal syntax, though -- several of our other type displays already aren't, e.g. `@Todo` type, and other type checkers often use non-Python syntax in their type displays. What about something like `TypeIs[int @ a[0]]`? Or `TypeIs[a[0] => int]`, or `TypeIs[a[0] :: int]`?

---

_@InSyncWithFoo reviewed on 2025-06-12 12:32_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/display.rs`:220 on 2025-06-12 12:32_

I like the first option. `TypeIs[int @ a]` it is, then.

---

_Comment by @InSyncWithFoo on 2025-06-12 12:36_

Now there's a new panic:

```text
type_guards.md:104 Attempting to "pull types" caused a panic at crates/ty_python_semantic/src/semantic_model.rs:100:44
type_guards.md:104 Note: either fix the panic or add the `<!-- pull-types:skip -->` directive to this test
type_guards.md:104 
type_guards.md:104 Failed to retrieve the inferred type for an `ast::Expr` node passed to `TypeInference::expression_type()`. The `TypeInferenceBuilder` should infer and store types for all `ast::Expr` nodes in any `TypeInference` region it analyzes.
```

Line 104 is this one:

```python
from typing_extensions import TypeGuard, TypeIs
```

---

_Comment by @AlexWaygood on 2025-06-12 12:44_

> Now there's a new panic:

You need to make sure you store a type for each subexpression when parsing the type annotation, even if you know that the types of the subexpressions will be irrelevant to the type of the outer expression. We made our tests more rigorous about catching this error recently (https://github.com/astral-sh/ruff/pull/18539). You can take a look at what I'm doing for `SpecialFormType::Not`, `SpecialFormType::CallableTypeOf` and `SpecialFormType::TypeOf` in https://github.com/astral-sh/ruff/pull/18642 for how to fix it.

---

_Comment by @InSyncWithFoo on 2025-06-12 13:01_

Got it, thanks!

---

_@carljm approved on 2025-06-13 22:27_

Thank you!

---

_Merged by @carljm on 2025-06-13 22:27_

---

_Closed by @carljm on 2025-06-13 22:27_

---

_Branch deleted on 2025-06-14 16:48_

---
