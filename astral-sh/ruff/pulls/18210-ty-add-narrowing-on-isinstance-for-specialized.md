```yaml
number: 18210
title: "[ty] Add narrowing on `isinstance` for specialized generic classes"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
base: main
head: isinstance-specialized-generic-classes
created_at: 2025-05-20T00:45:09Z
updated_at: 2025-06-28T23:12:32Z
url: https://github.com/astral-sh/ruff/pull/18210
synced_at: 2026-01-12T15:56:14Z
```

# [ty] Add narrowing on `isinstance` for specialized generic classes

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/456

## Test Plan

Add test to `narrow/isinstance.md`


---

_Review requested from @carljm by @MatthewMckee4 on 2025-05-20 00:45_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-05-20 00:45_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-05-20 00:45_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-05-20 00:45_

---

_Renamed from "Add narrowing on isinstance for specialized generic classes" to "[ty] Add narrowing on isinstance for specialized generic classes" by @MatthewMckee4 on 2025-05-20 00:45_

---

_Renamed from "[ty] Add narrowing on isinstance for specialized generic classes" to "[ty] Add narrowing on `isinstance` for specialized generic classes" by @MatthewMckee4 on 2025-05-20 00:45_

---

_@MatthewMckee4 reviewed on 2025-05-20 00:46_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/generics.rs`:514 on 2025-05-20 00:46_

I'm not sure this is exactly correct to be honest, but this implies we have either type is unspecialised right?

---

_Comment by @github-actions[bot] on 2025-05-20 00:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
nionutils (https://github.com/nion-software/nionutils)
- error[invalid-argument-type] nion/utils/Stream.py:335:67: Argument to bound method `__init__` is incorrect: Expected `Observable | None`, found `(Observable & ~AbstractStream[Unknown]) | (AbstractStream[Observable] & ~AbstractStream[Unknown])`
- Found 20 diagnostics
+ Found 19 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[invalid-assignment] aioredis/client.py:3432:13: Object of type `(Sequence[@Todo(Inference of subscript on special form)] & ~Mapping[Unknown, Unknown]) | (Mapping[AnyKeyT, int | float] & ~Mapping[Unknown, Unknown])` is not assignable to `Sequence[Unknown] | AbstractSet[AnyKeyT]`
- Found 28 diagnostics
+ Found 27 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[invalid-return-type] pyinstrument/stack_sampler.py:344:12: Return type does not match returned value: expected `dict[Unknown, int | float]`, found `dict[Unknown, Unknown] | dict[Unknown, int | float] | None`
+ error[invalid-return-type] pyinstrument/stack_sampler.py:344:12: Return type does not match returned value: expected `dict[Unknown, int | float]`, found `dict[Unknown, Unknown] | None`

kornia (https://github.com/kornia/kornia)
- error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
- Found 969 diagnostics
+ Found 968 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] src/graphql/execution/execute.py:1802:25: Argument to function `execute_impl` is incorrect: Expected `ExecutionContext`, found `(list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
- error[invalid-argument-type] src/graphql/execution/execute.py:2176:56: Argument to function `create_source_event_stream_impl` is incorrect: Expected `ExecutionContext`, found `(list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
- error[invalid-argument-type] src/graphql/execution/execute.py:2249:44: Argument to function `create_source_event_stream_impl` is incorrect: Expected `ExecutionContext`, found `(@Todo(map_with_boundness: intersections with negative contributions) & ~list[Unknown]) | (list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
- Found 410 diagnostics
+ Found 407 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ warning[unused-ignore-comment] strawberry/types/info.py:81:35: Unused blanket `type: ignore` directive
- Found 436 diagnostics
+ Found 437 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[invalid-argument-type] src/check_jsonschema/transforms/azure_pipelines.py:130:26: Argument to function `traverse_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `(dict[Unknown, Unknown] & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])`
- Found 67 diagnostics
+ Found 66 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[invalid-return-type] porcupine/plugins/langserver.py:234:12: Return type does not match returned value: expected `str`, found `(list[Unknown | str] & ~list[Unknown]) | (Unknown & ~list[Unknown]) | (str & ~list[Unknown])`
- Found 81 diagnostics
+ Found 80 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-argument-type] pydantic/_internal/_decorators.py:389:49: Argument to function `mro` is incorrect: Expected `type[Any]`, found `(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])`
- error[invalid-argument-type] pydantic/_internal/_fields.py:102:52: Argument to bound method `startswith` is incorrect: Expected `str | tuple[str, ...]`, found `str | Pattern[str]`
- error[invalid-assignment] pydantic/main.py:863:13: Object of type `tuple[(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
- error[invalid-argument-type] pydantic/main.py:867:67: Argument to function `map_generic_model_arguments` is incorrect: Expected `tuple[Any, ...]`, found `(type[Any] & tuple[Unknown, ...]) | (tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
- warning[possibly-unbound-attribute] pydantic/plugin/_loader.py:57:12: Attribute `values` on type `dict[Unknown, Unknown] | dict[str, PydanticPluginProtocol] | None` is possibly unbound
+ warning[possibly-unbound-attribute] pydantic/plugin/_loader.py:57:12: Attribute `values` on type `dict[Unknown, Unknown] | None` is possibly unbound
- error[invalid-assignment] pydantic/v1/generics.py:100:13: Object of type `tuple[(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
- error[invalid-argument-type] pydantic/v1/generics.py:106:37: Argument to function `check_parameters_count` is incorrect: Expected `tuple[Any, ...]`, found `(type[Any] & tuple[Unknown, ...]) | (tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
- error[invalid-argument-type] pydantic/v1/utils.py:610:17: Argument to function `assert_never` is incorrect: Expected `Never`, found `(Unknown & ~Mapping[Unknown, Unknown] & ~AbstractSet[Unknown]) | (AbstractSet[@Todo(Inference of subscript on special form)] & ~Mapping[Unknown, Unknown] & ~AbstractSet[Unknown]) | (Mapping[@Todo(Inference of subscript on special form), Any] & ~Mapping[Unknown, Unknown] & ~AbstractSet[Unknown])`
- Found 762 diagnostics
+ Found 755 diagnostics

paasta (https://github.com/yelp/paasta)
- error[invalid-return-type] paasta_tools/cli/utils.py:415:12: Return type does not match returned value: expected `tuple[list[Unknown], str]`, found `tuple[list[str] | list[Unknown], None | str]`
+ error[invalid-return-type] paasta_tools/cli/utils.py:415:12: Return type does not match returned value: expected `tuple[list[Unknown], str]`, found `tuple[list[str], None | str]`
- error[invalid-argument-type] paasta_tools/utils.py:2839:35: Argument to function `split` is incorrect: Expected `str | _ShlexInstream`, found `(str & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])`
- Found 935 diagnostics
+ Found 934 diagnostics

flake8 (https://github.com/pycqa/flake8)
- error[invalid-argument-type] src/flake8/options/manager.py:198:46: Argument to function `normalize_path` is incorrect: Expected `str`, found `(Any & ~list[Unknown]) | (list[str] & ~list[Unknown])`
- Found 44 diagnostics
+ Found 43 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[unsupported-operator] tornado/template.py:829:14: Operator `<` is not supported for types `slice[Any, Any, Any]` and `int`, in comparing `int | slice[Any, Any, Any]` with `Literal[0]`
- Found 344 diagnostics
+ Found 343 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-assignment] src/hydra_zen/wrapper/_implementations.py:1734:9: Object of type `((x) -> Unknown) | (Mapping[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`)] & ~Mapping[Unknown, Unknown]) | (((@Todo(Support for `typing.TypeAlias`), /) -> @Todo(Support for `typing.TypeAlias`)) & ~Mapping[Unknown, Unknown])` is not assignable to `(@Todo(Support for `typing.TypeAlias`), /) -> @Todo(Support for `typing.TypeAlias`)`
- error[type-assertion-failure] tests/annotations/declarations.py:1402:5: Argument does not have asserted type `PBuilds[int]`
- error[type-assertion-failure] tests/annotations/declarations.py:1409:5: Argument does not have asserted type `FullBuilds[int]`
- Found 638 diagnostics
+ Found 635 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ error[no-matching-overload] tests/test_option.py:28:12: No overload of function `pipe` matches arguments
+ error[no-matching-overload] tests/test_option.py:29:12: No overload of function `pipe` matches arguments
- Found 286 diagnostics
+ Found 288 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-assignment] src/werkzeug/sansio/response.py:620:13: Object of type `def on_update(value: WWWAuthenticate) -> None` is not assignable to attribute `_on_update` on type `(WWWAuthenticate & ~AlwaysFalsy & ~list[Unknown]) | (list[WWWAuthenticate] & ~AlwaysFalsy & ~list[Unknown])`
- Found 454 diagnostics
+ Found 453 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-assignment] torchvision/prototype/transforms/_misc.py:34:13: Object of type `dict[Any, (Sequence[int] & ~dict[Unknown, Unknown]) | (dict[type, Sequence[int] | None] & ~dict[Unknown, Unknown])]` is not assignable to `Sequence[int] | dict[type, Sequence[int] | None]`
+ error[invalid-assignment] torchvision/prototype/transforms/_misc.py:34:13: Object of type `dict[Any, Sequence[int] & ~dict[Unknown, Unknown]]` is not assignable to `Sequence[int] | dict[type, Sequence[int] | None]`
- error[invalid-assignment] torchvision/prototype/transforms/_misc.py:56:13: Object of type `dict[Any, (tuple[int, int] & ~dict[Unknown, Unknown]) | (dict[type, tuple[int, int] | None] & ~dict[Unknown, Unknown])]` is not assignable to `tuple[int, int] | dict[type, tuple[int, int] | None]`
+ error[invalid-assignment] torchvision/prototype/transforms/_misc.py:56:13: Object of type `dict[Any, tuple[int, int] & ~dict[Unknown, Unknown]]` is not assignable to `tuple[int, int] | dict[type, tuple[int, int] | None]`
- error[invalid-argument-type] torchvision/transforms/_functional_pil.py:179:42: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number & ~list[Unknown]) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number & ~list[Unknown]) | (int & tuple[Unknown, ...] & ~list[Unknown]) | (list[int] & tuple[Unknown, ...] & ~list[Unknown]) | (tuple[int, ...] & tuple[Unknown, ...] & ~list[Unknown]) | (int & list[Unknown] & ~list[Unknown]) | (list[int] & list[Unknown] & ~list[Unknown]) | (tuple[int, ...] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(map_with_boundness: intersections with negative contributions)`
- error[invalid-argument-type] torchvision/transforms/_functional_pil.py:183:37: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number & ~list[Unknown]) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number & ~list[Unknown]) | (int & tuple[Unknown, ...] & ~list[Unknown]) | (list[int] & tuple[Unknown, ...] & ~list[Unknown]) | (tuple[int, ...] & tuple[Unknown, ...] & ~list[Unknown]) | (int & list[Unknown] & ~list[Unknown]) | (list[int] & list[Unknown] & ~list[Unknown]) | (tuple[int, ...] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[invalid-argument-type] torchvision/transforms/functional.py:1226:45: Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `list[int]`
- error[invalid-argument-type] torchvision/transforms/functional.py:1226:45: Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `(list[int] & list[Unknown]) | (list[int] & tuple[Unknown, ...]) | list[Unknown]`
+ error[invalid-argument-type] torchvision/transforms/functional.py:1226:60: Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `(list[int] & ~tuple[Unknown, ...]) | list[Unknown]`
- error[invalid-argument-type] torchvision/transforms/functional.py:1226:60: Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `(list[int] & list[Unknown] & ~tuple[Unknown, ...]) | (list[int] & tuple[Unknown, ...] & ~tuple[Unknown, ...]) | list[Unknown]`
- Found 1843 diagnostics
+ Found 1841 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[invalid-return-type] schema_salad/sourceline.py:214:12: Return type does not match returned value: expected `int | float | str | CommentedMap | CommentedSeq | None`, found `(int & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (float & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (str & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (MutableMapping[str, Any] & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (MutableSequence[Any] & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | None`
- Found 247 diagnostics
+ Found 246 diagnostics

colour (https://github.com/colour-science/colour)
- error[invalid-assignment] colour/io/ctl.py:165:9: Object of type `(Sequence[str] & ~Sequence[Unknown]) | (dict[Unknown, Unknown] & ~Sequence[Unknown])` is not assignable to `dict[Unknown, Unknown]`
- Found 530 diagnostics
+ Found 529 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] scrapy/crawler.py:282:48: Argument to bound method `load_pre_crawler_settings` is incorrect: Expected `BaseSettings`, found `(dict[str, Any] & ~dict[Unknown, Unknown]) | Settings`
- error[invalid-argument-type] scrapy/utils/log.py:126:37: Argument to function `install_scrapy_root_handler` is incorrect: Expected `Settings`, found `Settings | (dict[Unknown, Any] & ~dict[Unknown, Unknown])`
- Found 1303 diagnostics
+ Found 1301 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[invalid-argument-type] boostedblob/request.py:246:28: Argument to function `dict_to_xml` is incorrect: Expected `Mapping[str, Any]`, found `(dict[str, Any] & ~bytes & ~bytearray) | memoryview[Unknown]`
- Found 40 diagnostics
+ Found 39 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-argument-type] pwndbg/chain.py:138:21: Argument to function `get` is incorrect: Expected `int | None`, found `(int & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])`
- Found 2227 diagnostics
+ Found 2226 diagnostics

altair (https://github.com/vega/altair)
- error[no-matching-overload] tests/__init__.py:159:10: No overload of function `compile` matches arguments
- Found 1300 diagnostics
+ Found 1299 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[invalid-argument-type] mesonbuild/msetup.py:383:37: Argument to function `parse_cmd_line_options` is incorrect: Expected `SharedCMDOptions`, found `CMDOptions | (list[Unknown] & ~list[Unknown])`
- error[invalid-assignment] mesonbuild/msetup.py:388:5: Object of type `Literal[False]` is not assignable to attribute `pager` on type `CMDOptions | (list[Unknown] & ~list[Unknown])`
- error[invalid-argument-type] mesonbuild/msetup.py:391:29: Argument to function `run_genvslite_setup` is incorrect: Expected `CMDOptions`, found `CMDOptions | (list[Unknown] & ~list[Unknown])`
- error[invalid-argument-type] mesonbuild/msetup.py:393:24: Argument to bound method `__init__` is incorrect: Expected `CMDOptions`, found `CMDOptions | (list[Unknown] & ~list[Unknown])`
- Found 1368 diagnostics
+ Found 1364 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[invalid-return-type] cwltool/docker.py:54:12: Return type does not match returned value: expected `list[str]`, found `list[Unknown] | @Todo(list comprehension type) | list[str] | None`
+ error[invalid-return-type] cwltool/docker.py:54:12: Return type does not match returned value: expected `list[str]`, found `list[Unknown] | @Todo(list comprehension type) | None`
+ error[unsupported-operator] cwltool/load_tool.py:228:17: Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["id"]` with `@Todo(Type::Intersection.call()) | None`
+ error[unsupported-operator] cwltool/load_tool.py:229:17: Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["$import"]` with `@Todo(Type::Intersection.call()) | None`
+ warning[call-possibly-unbound-method] cwltool/load_tool.py:231:13: Method `__getitem__` of type `@Todo(Type::Intersection.call()) | None` is possibly unbound
+ error[unsupported-operator] cwltool/main.py:283:51: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["#"]` with `@Todo(Type::Intersection.call()) | None`
+ warning[possibly-unbound-attribute] cwltool/main.py:284:38: Attribute `split` on type `@Todo(Type::Intersection.call()) | None` is possibly unbound
+ warning[possibly-unbound-attribute] cwltool/main.py:284:38: Attribute `split` on type `@Todo(Type::Intersection.call()) | None` is possibly unbound
+ warning[possibly-unbound-attribute] cwltool/main.py:284:38: Attribute `split` on type `@Todo(Type::Intersection.call()) | None` is possibly unbound
- Found 297 diagnostics
+ Found 304 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-assignment] mypy/semanal.py:7660:9: Object of type `tuple[(str & ~tuple[Unknown, ...]) | (tuple[str, ...] & ~tuple[Unknown, ...])]` is not assignable to `str | tuple[str, ...]`
- warning[possibly-unbound-attribute] mypy/test/helpers.py:445:13: Attribute `splitlines` on type `str | Pattern[str]` is possibly unbound
- error[invalid-assignment] mypy/types.py:3581:9: Object of type `tuple[(str & ~tuple[Unknown, ...]) | (tuple[str, ...] & ~tuple[Unknown, ...])]` is not assignable to `str | tuple[str, ...]`
- Found 3316 diagnostics
+ Found 3313 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[no-matching-overload] src/_pytest/_code/code.py:442:26: No overload of function `__new__` matches arguments
- warning[possibly-unbound-attribute] src/_pytest/nodes.py:419:20: Attribute `pytrace` on type `BaseException | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] src/_pytest/nodes.py:422:20: Attribute `formatrepr` on type `BaseException | Unknown` is possibly unbound
+ error[unresolved-attribute] src/_pytest/nodes.py:419:20: Type `BaseException` has no attribute `pytrace`
+ error[unresolved-attribute] src/_pytest/nodes.py:422:20: Type `BaseException` has no attribute `formatrepr`
+ error[unresolved-attribute] src/_pytest/reports.py:368:20: Type `BaseException` has no attribute `_use_item_location`

optuna (https://github.com/optuna/optuna)
+ error[invalid-return-type] optuna/visualization/matplotlib/_contour.py:184:12: Return type does not match returned value: expected `tuple[ndarray[Unknown, Unknown], list[str], list[int], list[int | float]]`, found `tuple[@Todo(unknown type subscript), list[Unknown], list[Unknown], list[int]]`
- Found 1072 diagnostics
+ Found 1073 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[call-non-callable] lib/streamlit/commands/echo.py:84:21: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (bound method dict[int, Any].__getitem__(key: int, /) -> Any)` is not callable on object of type `dict[Unknown, Unknown] | dict[int, Any]`
- warning[possibly-unbound-attribute] lib/streamlit/elements/lib/image_utils.py:411:68: Attribute `shape` on type `(@Todo(unknown type subscript) & ~list[Unknown] & ~str) | (list[str] & ~list[Unknown] & ~str) | None` is possibly unbound
+ warning[possibly-unbound-attribute] lib/streamlit/elements/lib/image_utils.py:411:68: Attribute `shape` on type `(@Todo(unknown type subscript) & ~list[Unknown] & ~str) | None` is possibly unbound
- warning[possibly-unbound-attribute] lib/streamlit/elements/lib/image_utils.py:413:20: Attribute `tolist` on type `(@Todo(unknown type subscript) & ~list[Unknown] & ~str) | (list[str] & ~list[Unknown] & ~str) | None` is possibly unbound
+ warning[possibly-unbound-attribute] lib/streamlit/elements/lib/image_utils.py:413:20: Attribute `tolist` on type `(@Todo(unknown type subscript) & ~list[Unknown] & ~str) | None` is possibly unbound
- error[invalid-argument-type] lib/tests/streamlit/external/langchain/capturing_callback_handler.py:96:42: Argument to function `load_records_from_file` is incorrect: Expected `str`, found `(list[CallbackRecord] & ~list[Unknown]) | (str & ~list[Unknown])`
- error[invalid-return-type] scripts/run_bare_execution_tests.py:59:12: Return type does not match returned value: expected `str`, found `(str & ~list[Unknown]) | (list[str] & ~list[Unknown])`
- Found 3257 diagnostics
+ Found 3254 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-return-type] src/bokeh/core/property/container.py:140:20: Return type does not match returned value: expected `PropertyValueList[T]`, found `list[T] & ~list[Unknown]`
- error[invalid-return-type] src/bokeh/core/property/container.py:165:20: Return type does not match returned value: expected `PropertyValueSet[T]`, found `set[T] & ~set[Unknown]`
- error[invalid-argument-type] src/bokeh/embed/util.py:199:34: Argument to bound method `__init__` is incorrect: Expected `dict[Model, Unknown]`, found `(list[Model] & ~list[Unknown]) | (dict[Model, Unknown] & ~list[Unknown]) | dict[Unknown, Unknown] | @Todo(dict comprehension type)`
+ error[invalid-argument-type] src/bokeh/layouts.py:559:49: Argument to function `traverse` is incorrect: Expected `list[LayoutDOM]`, found `LayoutDOM`
- error[call-non-callable] src/bokeh/plotting/_renderer.py:217:28: Method `__getitem__` of type `(bound method dict[str, Any].__getitem__(key: str, /) -> Any) | (bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown)` is not callable on object of type `dict[str, Any] | dict[Unknown, Unknown]`
+ error[call-non-callable] src/bokeh/plotting/_renderer.py:217:28: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[invalid-argument-type] src/bokeh/util/deprecation.py:70:10: Argument to function `warn` is incorrect: Expected `str`, found `str | (tuple[@Todo(Generic tuple specializations), ...] & ~tuple[Unknown, ...])`
- Found 953 diagnostics
+ Found 950 diagnostics

zulip (https://github.com/zulip/zulip)
- error[not-iterable] zerver/lib/export.py:667:14: Object of type `Unknown | list[str] | None | list[Unknown]` may not be iterable
+ error[not-iterable] zerver/lib/export.py:667:14: Object of type `Unknown | list[str] | None` may not be iterable
- error[not-iterable] zerver/lib/export.py:807:14: Object of type `Unknown | list[str] | None | list[Unknown]` may not be iterable
+ error[not-iterable] zerver/lib/export.py:807:14: Object of type `Unknown | list[str] | None` may not be iterable
- error[invalid-assignment] zerver/lib/message.py:1139:5: Object of type `dict[str, list[UnreadDirectMessageInfo] | list[UnreadStreamInfo] | list[UnreadDirectMessageGroupInfo] | list[Unknown] | int | @Todo(TypedDict)]` is not assignable to `UnreadMessagesResult`
+ error[invalid-assignment] zerver/lib/message.py:1139:5: Object of type `dict[str, list[UnreadDirectMessageInfo] | list[UnreadStreamInfo] | list[UnreadDirectMessageGroupInfo] | int | @Todo(TypedDict)]` is not assignable to `UnreadMessagesResult`
- error[invalid-return-type] zerver/lib/recipient_parsing.py:10:12: Return type does not match returned value: expected `int`, found `(int & ~list[Unknown]) | (list[int] & ~list[Unknown])`
- error[not-iterable] zerver/lib/templates.py:142:14: Object of type `list[Unknown] | list[Extension] | None` may not be iterable
+ error[not-iterable] zerver/lib/templates.py:142:14: Object of type `list[Unknown] | None` may not be iterable
- error[not-iterable] zerver/lib/templates.py:158:44: Object of type `list[Unknown] | list[Extension] | None` may not be iterable
+ error[not-iterable] zerver/lib/templates.py:158:44: Object of type `list[Unknown] | None` may not be iterable
- error[invalid-argument-type] zerver/views/push_notifications.py:112:47: Argument to function `send_test_push_notification` is incorrect: Expected `list[PushDeviceToken]`, found `list[Unknown] | list[Self]`
- warning[possibly-unbound-attribute] zproject/computed_settings.py:118:5: Attribute `append` on type `list[Unknown] | list[str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] zproject/computed_settings.py:118:5: Attribute `append` on type `list[Unknown] | None` is possibly unbound
- Found 6884 diagnostics
+ Found 6882 diagnostics

scipy (https://github.com/scipy/scipy)
- error[invalid-assignment] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:511:9: Object of type `tuple[(int & ~tuple[Unknown, ...]) | (tuple[int, ...] & ~tuple[Unknown, ...])]` is not assignable to `int | tuple[int, ...]`
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:512:25: Argument to function `len` is incorrect: Expected `Sized`, found `int | tuple[int, ...]`
- error[no-matching-overload] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:513:24: No overload of function `min` matches arguments
- error[no-matching-overload] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:513:45: No overload of function `max` matches arguments
- error[not-iterable] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:518:40: Object of type `int | tuple[int, ...]` may not be iterable
- error[invalid-assignment] subprojects/highs/src/highspy/highs.py:1127:21: Object of type `Literal[-1]` is not assignable to attribute `index` on type `(Mapping[Any, highs_cons] & ~Mapping[Unknown, Unknown] & ~Sequence[Unknown]) | (Sequence[highs_cons] & ~Mapping[Unknown, Unknown] & ~Sequence[Unknown]) | (highs_cons & ~Mapping[Unknown, Unknown] & ~Sequence[Unknown])`
- Found 7504 diagnostics
+ Found 7498 diagnostics

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-05-20 01:13_

---

_@JelleZijlstra reviewed on 2025-05-20 02:06_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:129 on 2025-05-20 02:06_

Unrelated but this is wrong, `type(x)` would only ever return `A`.

---

_@JelleZijlstra reviewed on 2025-05-20 02:10_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:514 on 2025-05-20 02:10_

This seems incorrect; the types `list[int]` and `list[Any]` are equivalent.

---

_@MatthewMckee4 reviewed on 2025-05-20 08:51_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/generics.rs`:514 on 2025-05-20 08:51_

Yes I thought I should just change to dynamic 

---

_@MatthewMckee4 reviewed on 2025-05-20 09:30_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/generics.rs`:514 on 2025-05-20 09:30_

If i change this to is_dynamic, we have lots of tests failing in `variance.md` like:
```py
static_assert(not is_gradual_equivalent_to(C[Any], C[A]))
```


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:514 on 2025-05-20 14:21_

I don't think either proposed edit to this method is correct.

> This seems incorrect; the types `list[int]` and `list[Any]` are equivalent.

Those two are consistent, but not equivalent nor gradually equivalent, right?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/builder.rs`:797 on 2025-05-20 14:24_

I think the correct fix might be to use `is_assignable_to` here instead of `is_subtype_of`?

---

_@dcreager reviewed on 2025-05-20 14:24_

---

_@JelleZijlstra reviewed on 2025-05-20 14:35_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:514 on 2025-05-20 14:35_

Yes sorry, my wording was wrong. But I think the approach in this PR is wrong in a more general way; see my comment on the issue.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:227 on 2025-05-20 15:45_

I think simplifying `A[int] & A[Unknown]` to just `A[int]` is not correct in general. It depends on the variance. In this particular test case as currently written, `A` should be bivariant in `T` because `T` is unused in `A`, which is particularly odd and means that any specialization of `A` is equivalent to any other. With any other variance, `A[Unknown] & A[int]` should not simplify, because `Unknown` can materialize to some type that might make the whole type `Never` (in case of invariance) or narrower (in case of e.g. contravariance and `Unknown` specializing to e.g. `object`, or covariance and `Unknown` specializing to e.g. `bool`).

But as discussed in https://github.com/astral-sh/ty/issues/456, all of this becomes irrelevant when we realize that `A[Unknown]` is not really the correct type to be narrowing against here; instead we should use a new type that represents "all possible specializations of A".

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:129 on 2025-05-20 15:52_

Yes, in this case we should ignore the narrowing (and potentially also treat it as a known-false condition and unreachable code); the test will never pass.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:797 on 2025-05-20 15:54_

No, we shouldn't simplify intersections according to assignability -- that would imply that e.g. `int & Any` collapses, which it should not. `Any` could materialize to either a wider or narrower type than `int`, so it doesn't reduce (it represents a dynamic type with upper bound of `int`.)

---

_@carljm requested changes on 2025-05-20 15:55_

Thanks for the PR! As discussed inline and in the issue, I think we need a different approach here.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-20 17:58_

---

_Closed by @MatthewMckee4 on 2025-05-20 22:40_

---

_Branch deleted on 2025-06-28 23:12_

---
