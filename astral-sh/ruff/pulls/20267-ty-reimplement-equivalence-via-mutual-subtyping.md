```yaml
number: 20267
title: "[ty] Reimplement equivalence via mutual subtyping"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/rework-equivalence-relation
created_at: 2025-09-05T14:57:12Z
updated_at: 2025-09-07T12:59:10Z
url: https://github.com/astral-sh/ruff/pull/20267
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Reimplement equivalence via mutual subtyping

---

_Pull request opened by @AlexWaygood on 2025-09-05 14:57_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-09-05 14:57_

---

_Comment by @github-actions[bot] on 2025-09-05 14:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-05 15:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/_check/metadata/metadecor.py:523:34: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ beartype/_check/metadata/metadecor.py:523:34: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT@callable_cached & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT@callable_cached` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT@method_cached_arg_by_id & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT@method_cached_arg_by_id` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Type `CallableT@property_cached & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Type `CallableT@property_cached` has no attribute `__name__`
- beartype/_util/func/utilfuncscope.py:261:29: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/func/utilfuncscope.py:261:29: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
- beartype/_util/func/utilfuncscope.py:320:16: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__qualname__`
+ beartype/_util/func/utilfuncscope.py:320:16: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__qualname__`
- beartype/_util/func/utilfuncscope.py:334:16: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__qualname__`
+ beartype/_util/func/utilfuncscope.py:334:16: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__qualname__`
- beartype/_util/func/utilfunctest.py:907:16: error[unresolved-attribute] Type `((...) -> Unknown) & ~((...) -> Unknown)` has no attribute `__qualname__`
- beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
+ beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
- Found 589 diagnostics
+ Found 588 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_deprecate.py:49:19: error[unresolved-attribute] Type `<Protocol with members '__qualname__'>` has no attribute `__module__`
- Found 676 diagnostics
+ Found 677 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/plugins/ses.py:472:17: error[invalid-argument-type] Argument to function `formataddr` is incorrect: Expected `tuple[str | None, str]`, found `tuple[(Unknown & ~AlwaysFalsy) | Literal[False], Unknown]`
+ apprise/plugins/ses.py:472:17: error[invalid-argument-type] Argument to function `formataddr` is incorrect: Expected `tuple[str | None, str]`, found `tuple[(Unknown & ~AlwaysFalsy) | (@Todo(Subscript expressions on intersections) & ~AlwaysFalsy) | Literal[False], Unknown]`

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/core/output/sanitization.py:54:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/schemathesis/hooks.py:297:16: error[unresolved-attribute] Type `(str & ((...) -> object)) | (((...) -> Unknown) & ((...) -> object))` has no attribute `__name__`
+ src/schemathesis/hooks.py:297:16: error[unresolved-attribute] Type `(str & ((...) -> object)) | ((...) -> Unknown)` has no attribute `__name__`
+ src/schemathesis/schemas.py:281:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 271 diagnostics
+ Found 273 diagnostics

flake8 (https://github.com/pycqa/flake8)
+ src/flake8/processor.py:284:16: error[invalid-return-type] Return type does not match returned value: expected `dict[int, str]`, found `dict[int, LiteralString]`
- Found 39 diagnostics
+ Found 40 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/cog.py:538:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:538:13: error[unresolved-attribute] Unresolved attribute `__cog_listener__` on type `(...) -> Unknown`.
- discord/ext/commands/cog.py:539:33: error[unresolved-attribute] Type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)` has no attribute `__name__`
+ discord/ext/commands/cog.py:539:33: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
- discord/ext/commands/cog.py:541:17: error[unresolved-attribute] Type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)` has no attribute `__cog_listener_names__`
+ discord/ext/commands/cog.py:541:17: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__cog_listener_names__`
- discord/ext/commands/cog.py:543:17: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:543:17: error[unresolved-attribute] Unresolved attribute `__cog_listener_names__` on type `(...) -> Unknown`.

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/vcs/git/backend.py:548:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ tests/integration/test_utils_vcs_git.py:432:43: error[unresolved-attribute] Type `Literal[b""]` has no attribute `encode`
- Found 928 diagnostics
+ Found 930 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/config/cc_growpart.py:214:12: error[invalid-return-type] Return type does not match returned value: expected `Resizer`, found `None | (Unknown & ~AlwaysFalsy) | (ResizeGrowPart & ~AlwaysFalsy) | (ResizeGrowFS & ~AlwaysFalsy) | (ResizeGpart & ~AlwaysFalsy)`
+ cloudinit/config/cc_growpart.py:214:12: error[invalid-return-type] Return type does not match returned value: expected `Resizer`, found `None | (Unknown & ~AlwaysFalsy) | (ResizeGrowPart & ~AlwaysFalsy) | (ResizeGrowFS & ~AlwaysFalsy) | (ResizeGpart & ~AlwaysFalsy) | (@Todo(dict literal value type) & ~AlwaysFalsy)`

tornado (https://github.com/tornadoweb/tornado)
- tornado/routing.py:357:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Matcher`, found `object`
+ tornado/routing.py:357:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Matcher`, found `str | Matcher | Any`
- tornado/routing.py:357:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `object`
+ tornado/routing.py:357:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `str | Matcher | Any`
- tornado/routing.py:357:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
+ tornado/routing.py:357:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `str | Matcher | Any`

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/data/converter/orderflow.py:76:9: error[no-matching-overload] No overload of bound method `drop` matches arguments
+ freqtrade/data/converter/orderflow.py:135:21: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ freqtrade/data/converter/trade_converter.py:33:35: error[invalid-argument-type] Argument to bound method `drop_duplicates` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ freqtrade/freqai/data_kitchen.py:717:36: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `list[@Todo(list comprehension element type)]`
+ freqtrade/freqai/freqai_interface.py:944:56: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `list[Unknown]`
+ freqtrade/optimize/hyperopt_tools.py:445:30: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- Found 387 diagnostics
+ Found 393 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/debugger/__init__.py:120:11: warning[possibly-unbound-attribute] Attribute `tb_next` on type `(Unknown & ~None) | TracebackType | None` is possibly unbound
+ Pythonwin/pywin/debugger/__init__.py:120:11: warning[possibly-unbound-attribute] Attribute `tb_next` on type `(Unknown & ~None) | (@Todo(Support for `typing.TypeAlias`) & ~None) | TracebackType | None` is possibly unbound
- Pythonwin/pywin/debugger/__init__.py:121:13: warning[possibly-unbound-attribute] Attribute `tb_next` on type `(Unknown & ~None) | TracebackType | None` is possibly unbound
+ Pythonwin/pywin/debugger/__init__.py:121:13: warning[possibly-unbound-attribute] Attribute `tb_next` on type `(Unknown & ~None) | (@Todo(Support for `typing.TypeAlias`) & ~None) | TracebackType | None` is possibly unbound
- Pythonwin/pywin/debugger/__init__.py:125:23: warning[possibly-unbound-attribute] Attribute `tb_frame` on type `(Unknown & ~None) | TracebackType | None` is possibly unbound
+ Pythonwin/pywin/debugger/__init__.py:125:23: warning[possibly-unbound-attribute] Attribute `tb_frame` on type `(Unknown & ~None) | (@Todo(Support for `typing.TypeAlias`) & ~None) | TracebackType | None` is possibly unbound

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:524:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 198 diagnostics
+ Found 199 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/test_server.py:62:13: error[invalid-assignment] Object of type `def _kill(d: ServerConnectionHookData) -> None` is not assignable to attribute `side_effect` on type `Mock | ((...) -> Unknown)`
- test/mitmproxy/proxy/test_server.py:68:12: warning[possibly-unbound-attribute] Attribute `call_args` on type `Mock | ((...) -> Unknown)` is possibly unbound
- test/mitmproxy/proxy/test_server.py:70:12: warning[possibly-unbound-attribute] Attribute `called` on type `Mock | ((...) -> Unknown)` is possibly unbound
+ test/mitmproxy/proxy/test_server.py:62:13: error[unresolved-attribute] Unresolved attribute `side_effect` on type `(...) -> Unknown`.
- test/mitmproxy/proxy/test_server.py:71:12: warning[possibly-unbound-attribute] Attribute `called` on type `Mock | ((...) -> Unknown)` is possibly unbound
- test/mitmproxy/proxy/test_server.py:73:12: warning[possibly-unbound-attribute] Attribute `called` on type `Mock | ((...) -> Unknown)` is possibly unbound
+ test/mitmproxy/proxy/test_server.py:68:12: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `call_args`
+ test/mitmproxy/proxy/test_server.py:70:12: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `called`
+ test/mitmproxy/proxy/test_server.py:71:12: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `called`
+ test/mitmproxy/proxy/test_server.py:73:12: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `called`

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/load_tool.py:259:54: error[invalid-argument-type] Argument to function `_fast_parser_convert_stdstreams_to_files` is incorrect: Expected `Process | MutableSequence[Process]`, found `object`
- cwltool/load_tool.py:286:39: error[invalid-argument-type] Argument to function `_fast_parser_handle_hints` is incorrect: Expected `Process | MutableSequence[Process]`, found `object`
- Found 146 diagnostics
+ Found 144 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- mypy_django_plugin/django/context.py:331:64: warning[possibly-unbound-attribute] Attribute `base_field` on type `(Field[Any, Any] & Top[ArrayField[Unknown, Unknown]]) | (ForeignObjectRel & Top[ArrayField[Unknown, Unknown]]) | (Field[Any, Any] & ArrayField) | (ForeignObjectRel & ArrayField) | (AutoField[Unknown, Unknown] & Field[Unknown, Unknown] & Top[ArrayField[Unknown, Unknown]]) | (AutoField[Unknown, Unknown] & Field[Unknown, Unknown] & ArrayField)` is possibly unbound
+ mypy_django_plugin/django/context.py:331:64: warning[possibly-unbound-attribute] Attribute `base_field` on type `(Field[Any, Any] & Top[ArrayField[Unknown, Unknown]]) | (ForeignObjectRel & Top[ArrayField[Unknown, Unknown]]) | (Field[Any, Any] & ArrayField) | (ForeignObjectRel & ArrayField)` is possibly unbound

pytest (https://github.com/pytest-dev/pytest)
+ testing/test_mark_expression.py:14:47: warning[redundant-cast] Value is already of type `MatcherCall`
- Found 471 diagnostics
+ Found 472 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/embed/util.py:315:19: error[unresolved-attribute] Type `~Document` has no attribute `document`
- Found 848 diagnostics
+ Found 847 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/coverstore/utils.py:92:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, dict[str, str]]`, found `tuple[Literal[b""], dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)]]`
+ openlibrary/plugins/upstream/utils.py:1517:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 710 diagnostics
+ Found 712 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/builders/linkcheck.py:758:20: error[invalid-return-type] Return type does not match returned value: expected `str | None`, found `Literal[b""]`
+ sphinx/directives/code.py:146:34: error[invalid-argument-type] Argument to function `dedent_lines` is incorrect: Expected `list[str]`, found `list[LiteralString]`
- Found 515 diagnostics
+ Found 517 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/www.py:115:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any]) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any])`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any]`
- Found 1798 diagnostics
+ Found 1799 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `(...) -> Unknown`, in comparing `Unknown` with `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Unknown, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]`
+ xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `(...) -> Unknown`, in comparing `Unknown` with `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]`
+ xarray/computation/rolling.py:1214:38: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `set[Unknown]`
+ xarray/conventions.py:598:24: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `set[Hashable | Unknown]`
+ xarray/core/common.py:401:9: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to `Hashable`
+ xarray/core/common.py:403:9: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `Hashable`
+ xarray/core/common.py:412:9: error[invalid-assignment] Object of type `list[@Todo(list comprehension element type)]` is not assignable to `Hashable`
+ xarray/core/common.py:414:45: error[not-iterable] Object of type `Hashable` is not iterable
+ xarray/core/common.py:418:12: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `Hashable`
- xarray/core/dataset.py:1340:49: error[invalid-argument-type] Argument to function `did_you_mean` is incorrect: Expected `Hashable`, found `Hashable | Iterable[Hashable]`
- xarray/core/dataset.py:1406:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Hashable | Iterable[Hashable]`
+ xarray/core/dataset.py:1406:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Hashable`
+ xarray/core/dataset.py:1829:13: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `Hashable`
+ xarray/core/dataset.py:1831:13: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to `Hashable`
- xarray/core/dataset.py:4623:28: error[unsupported-operator] Operator `<` is not supported for types `object` and `int`
- xarray/core/dataset.py:4623:48: error[unsupported-operator] Operator `<` is not supported for types `int` and `object`
- xarray/core/dataset.py:4629:38: error[unsupported-operator] Operator `>=` is not supported for types `object` and `int`, in comparing `object` with `Literal[0]`
- xarray/core/dataset.py:4629:50: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `object`
+ xarray/core/dataset.py:5929:13: error[invalid-assignment] Object of type `set[@Todo(set literal element type)]` is not assignable to `Hashable`
+ xarray/core/dataset.py:5931:13: error[invalid-assignment] Object of type `set[Unknown]` is not assignable to `Hashable`
+ xarray/core/dataset.py:5941:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Hashable`
+ xarray/core/dataset.py:5947:50: error[invalid-argument-type] Argument to function `assert_no_index_corrupted` is incorrect: Expected `set[Hashable]`, found `Hashable`
+ xarray/core/dataset.py:5951:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `Hashable`
+ xarray/core/dataset.py:5956:62: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `Hashable`
+ xarray/core/parallel.py:664:32: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `set[Hashable]`
+ xarray/structure/merge.py:1049:9: error[invalid-assignment] Object of type `set[Unknown]` is not assignable to `Hashable`
+ xarray/structure/merge.py:1051:9: error[invalid-assignment] Object of type `set[@Todo(set literal element type)]` is not assignable to `Hashable`
+ xarray/structure/merge.py:1063:16: error[unsupported-operator] Operator `in` is not supported for types `@Todo(Inference of subscript on special form)` and `Hashable`, in comparing `@Todo(Inference of subscript on special form)` with `Hashable & ~AlwaysFalsy`
+ xarray/tests/test_concat.py:1550:23: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_concat.py:1558:23: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_concat.py:1572:23: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataarray.py:7195:37: error[invalid-argument-type] Argument to bound method `drop_duplicates` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:963:39: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:969:39: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:2751:27: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:2756:27: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:3106:34: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:3121:34: error[invalid-argument-type] Argument to bound method `drop_indexes` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:4556:13: error[invalid-assignment] Method `__setitem__` of type `bound method Dataset.__setitem__(key: Hashable, value: Any) -> None` cannot be called with a key of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` and a value of type `float` on object of type `Dataset`
+ xarray/tests/test_dataset.py:4708:9: error[invalid-assignment] Method `__setitem__` of type `bound method Dataset.__setitem__(key: Hashable, value: Any) -> None` cannot be called with a key of type `dict[str, DataArray]` and a value of type `Dataset` on object of type `Dataset`
+ xarray/tests/test_dataset.py:4738:9: error[invalid-assignment] Method `__setitem__` of type `bound method Dataset.__setitem__(key: Hashable, value: Any) -> None` cannot be called with a key of type `list[@Todo(list literal element type)]` and a value of type `list[@Todo(list literal element type)]` on object of type `Dataset`
+ xarray/tests/test_dataset.py:4742:9: error[invalid-assignment] Method `__setitem__` of type `bound method Dataset.__setitem__(key: Hashable, value: Any) -> None` cannot be called with a key of type `list[@Todo(list literal element type)]` and a value of type `list[@Todo(list comprehension element type)]` on object of type `Dataset`
+ xarray/tests/test_groupby.py:2044:28: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_groupby.py:2513:28: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_indexing.py:139:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[Any, Index]`, found `dict[Unknown, Any | None]`
+ xarray/tests/test_plot.py:1297:28: error[invalid-argument-type] Argument to bound method `set_coords` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- Found 1679 diagnostics
+ Found 1711 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/series/test_series.py:921:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[tuple[Unknown, ...], Series[int]]]`
- tests/series/test_series.py:933:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[@Todo(Support for `typing.TypeAlias`), Series[int]]]`
- tests/series/test_series.py:943:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[tuple[Unknown, ...], Series[int]]]`
- tests/series/test_series.py:952:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[@Todo(Support for `typing.TypeAlias`), Series[int]]]`
- tests/series/test_series.py:984:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Period, Series[int]]]`
- tests/series/test_series.py:993:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Timestamp, Series[int]]]`
- tests/series/test_series.py:1002:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Timedelta, Series[int]]]`
- tests/series/test_series.py:1015:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Interval[Timestamp], Series[int]]]`
- tests/series/test_series.py:1042:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Any, Series[int]]]`
- tests/series/test_series.py:1058:9: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Any, Series[int]]]`
- tests/series/test_series.py:1828:5: error[type-assertion-failure] Argument does not have asserted type `dict[Any, str]`
- tests/series/test_series.py:3892:11: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Hashable, int]]`
+ tests/test_frame.py:458:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ tests/test_frame.py:459:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `Index[Any]`
+ tests/test_frame.py:460:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `set[@Todo(set literal element type)]`
+ tests/test_frame.py:468:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ tests/test_frame.py:472:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `Index[Any]`
+ tests/test_frame.py:477:32: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable`, found `Index[str]`
+ tests/test_frame.py:525:42: error[invalid-argument-type] Argument to bound method `drop_duplicates` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
+ tests/test_frame.py:537:46: error[invalid-argument-type] Argument to bound method `drop_duplicates` is incorrect: Expected `Hashable`, found `set[@Todo(set literal element type)]`
+ tests/test_frame.py:539:44: error[invalid-argument-type] Argument to bound method `drop_duplicates` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- tests/test_frame.py:3400:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[tuple[Unknown, ...], DataFrame]]`
- tests/test_frame.py:3423:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[tuple[Unknown, ...], DataFrame]]`
- tests/test_frame.py:3447:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Period, DataFrame]]`
- tests/test_frame.py:3456:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Timestamp, DataFrame]]`
- tests/test_frame.py:3465:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Timedelta, DataFrame]]`
- tests/test_frame.py:3478:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[tuple[Interval[Timestamp], DataFrame]]`
- tests/test_frame.py:3777:9: error[type-assertion-failure] Argument does not have asserted type `defaultdict[Any, list[Unknown]]`
- tests/test_frame.py:3794:9: error[type-assertion-failure] Argument does not have asserted type `list[defaultdict[Any, list[Unknown]]]`
- Found 4969 diagnostics
+ Found 4958 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-bitbucket/prefect_bitbucket/repository.py:136:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""] | Unknown`
+ src/integrations/prefect-github/prefect_github/repository.py:77:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""] | Unknown`
+ src/integrations/prefect-gitlab/prefect_gitlab/repositories.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""] | Unknown`
- src/prefect/concurrency/v1/sync.py:63:58: error[invalid-argument-type] Argument to function `emit_concurrency_acquisition_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `(Unknown & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Unknown] & ~Coroutine[Any, Any, Any])`
- src/prefect/concurrency/v1/sync.py:72:41: error[invalid-argument-type] Argument to function `emit_concurrency_release_events` is incorrect: Expected `list[MinimalConcurrencyLimitResponse]`, found `(Unknown & ~Coroutine[Any, Any, Any]) | (Coroutine[Any, Any, Unknown] & ~Coroutine[Any, Any, Any])`
- src/prefect/flows.py:288:34: warning[possibly-unbound-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object) & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object))` is possibly unbound
- src/prefect/flows.py:405:68: warning[possibly-unbound-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object) & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object))` is possibly unbound
+ src/prefect/flows.py:288:34: warning[possibly-unbound-attribute] Attribute `__name__` on type `((...) -> R@__init__) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod)` is possibly unbound
+ src/prefect/flows.py:405:68: warning[possibly-unbound-attribute] Attribute `__name__` on type `((...) -> R@__init__) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod)` is possibly unbound
+ src/prefect/runner/storage.py:197:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/prefect/server/events/models/automations.py:223:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/server/events/models/automations.py:270:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/events/models/automations.py:290:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/events/services/event_persister.py:72:25: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/artifacts.py:449:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/artifacts.py:449:39: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/artifacts.py:534:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/block_documents.py:473:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/block_documents.py:664:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
- src/prefect/server/models/block_schemas.py:369:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/server/models/block_schemas.py:303:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
- src/prefect/server/models/block_schemas.py:782:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/server/models/block_types.py:200:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/block_types.py:221:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits.py:149:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits.py:161:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits_v2.py:144:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits_v2.py:165:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits_v2.py:209:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits_v2.py:257:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/concurrency_limits_v2.py:279:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/csrf_token.py:105:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/deployments.py:318:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/deployments.py:587:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/deployments.py:1035:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/deployments.py:1060:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/deployments.py:1087:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flow_run_input.py:91:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flow_run_states.py:79:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flow_runs.py:174:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flow_runs.py:504:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flow_runs.py:685:19: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flows.py:83:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/flows.py:290:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/logs.py:113:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/saved_searches.py:146:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/task_run_states.py:74:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/task_runs.py:176:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/task_runs.py:462:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/variables.py:123:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/variables.py:140:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/variables.py:154:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/variables.py:168:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/work_queues.py:276:15: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/work_queues.py:305:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/workers.py:253:15: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/workers.py:289:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/workers.py:626:15: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/workers.py:770:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
+ src/prefect/server/models/workers.py:799:12: error[unresolved-attribute] Type `Result[Unknown]` has no attribute `rowcount`
- Found 2987 diagnostics
+ Found 3033 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:3408:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3408:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/interpreter/interpreter.py:3427:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) ->...*[Comment body truncated]*

---

_Comment by @AlexWaygood on 2025-09-05 15:14_

### `beartype`

The [beartype change](https://github.com/beartype/beartype/blob/2c625cac1be01e0caa03bbba4bc2c9bb86a89136/beartype/_util/func/utilfunctest.py#L829-L908) seems to be desirable -- we now consider `((...) -> Unknown) & ~((...) -> Unknown)` to simplify to `Never`, which means things like this now have more intuitive behaviour:

```py
def f(x: object):
    if callable(x):
        reveal_type(x)
        if not callable(x):
            reveal_type(x)  # now `Never`, was previously `((...) -> Unknown) & ~((...) -> Unknown)`
```

### `cwltool`

I'm not totally sure why on `main` we infer the type of of `processobj` as `object` [here](https://github.com/common-workflow-language/cwltool/blob/256306a5da1eb0a8391d5f6734e7baae96922079/cwltool/load_tool.py#L249-L259) on `main`, but it seems like an improvement that we no longer do on this branch.

---

_Comment by @carljm on 2025-09-05 15:28_

We should narrow `if callable(x)` using the top materialization of `(...) -> Unknown` instead of using a gradual type (just like we recently started doing for invariant generics); that would give us the beartype improvement too.

---

_Comment by @carljm on 2025-09-05 15:28_

How do the property tests feel about this change?

---

_Comment by @carljm on 2025-09-05 15:30_

See https://github.com/astral-sh/ruff/pull/18799 for where I previously experimented with this; the PR description there explains why this breaks transitivity of subtyping.

---

_Comment by @JelleZijlstra on 2025-09-06 01:30_

> we now consider `((...) -> Unknown) & ~((...) -> Unknown)` to simplify to `Never`, which means things like this now have more intuitive behaviour:

That seems incorrect in principle. That type is equivalent to `((...) -> Unknown)`, for the same reason that `~Any` is the same as `Any`.

`callable()` is currently defined as returning `TypeIs[Callable[..., object]]`. It should really be `TypeIs[Top[Callable[..., object]]]`, though of course we can't do that in typeshed. Might be worth special-casing in ty though.

---

_Renamed from "[ty] Make `Any` a subtype of `Any`" to "[ty] Reimplement equivalence via mutual subtyping" by @AlexWaygood on 2025-09-07 12:36_

---

_Comment by @AlexWaygood on 2025-09-07 12:47_

I think you're right that making `Any` a subtype of `Any` probably isn't theoretically sound. The motivation here was to see if we could implement equivalence via mutual subtyping rather than having an entirely different logical approach to equivalence. That would mean that we could delete a lot of code and probably be more internally consistent about things (see the updated PR diff!). It would also make it easier to explore doing less eager union simplification.

Anyway, it's not possible to implement `a.is_equivalent_to(b)` as `a.is_subtype_of(b) && b.is_subtype_of(a)` unless `Any.is_subtype_of(Any)` holds, since our current logic (correctly, I think) views `Any` as equivalent to itself. @JelleZijlstra's idea in https://github.com/astral-sh/ty/issues/994 of implementing equivalence using `Top[]` and `Bottom[]` types might work better here. One more option is that we might consider introducing a third type relation -- sitting between `TypeRelation::Assignability` and `TypeRelation::Subtyping` -- `TypeRelation::UnionSimplification`. This could be used to reflect the fact that `Any` is not a subtype of `Any`, but `Any` is equivalent to `Any` and the union `Any | Any` can be simplified to `Any`.

---

_Comment by @codspeed-hq[bot] on 2025-09-07 12:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Frework-equivalence-relation?runnerMode=WallTime)

### Merging #20267 will **degrade performances by 10.69%**

<sub>Comparing <code>alex/rework-equivalence-relation</code> (1a73410) with <code>main</code> (2467c43)</sub>



### Summary

`❌ 5` regressions  
`✅ 3` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Frework-equivalence-relation?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` medium[colour-science] `` | 6.9 s | 7.4 s | -6.05% |
| ❌ | `` medium[pandas] `` | 26.3 s | 29.5 s | -10.69% |
| ❌ | `` small[altair] `` | 2.5 s | 2.7 s | -8.58% |
| ❌ | `` small[freqtrade] `` | 4 s | 4.4 s | -7.99% |
| ❌ | `` small[pydantic] `` | 2.3 s | 2.5 s | -4.95% |


---

_Comment by @codspeed-hq[bot] on 2025-09-07 12:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Frework-equivalence-relation?runnerMode=Instrumentation)

### Merging #20267 will **degrade performances by 14.29%**

<sub>Comparing <code>alex/rework-equivalence-relation</code> (1a73410) with <code>main</code> (2467c43)</sub>



### Summary

`⚡ 1` improvements  
`❌ 1` regressions  
`✅ 41` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Frework-equivalence-relation?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` DateType `` | 268 ms | 254.4 ms | +5.37% |
| ❌ | `` hydra-zen `` | 716.4 ms | 835.8 ms | -14.29% |


---

_Comment by @AlexWaygood on 2025-09-07 12:57_

Anyway, I don't plan to spend much more time on this right now; it was just an experiment prompted by https://github.com/astral-sh/ty/issues/1132 (for which one solution would be to do less eager union simplification -- but doing less eager union simplification breaks our current implementation of equivalence).

> `callable()` is currently defined as returning `TypeIs[Callable[..., object]]`. It should really be `TypeIs[Top[Callable[..., object]]]`, though of course we can't do that in typeshed. Might be worth special-casing in ty though.

the actual code in beartype used a custom function with a `TypeIs[Callable]` return type FWIW, which would be harder to special-case; I just used `callable()` in my comment above for a smaller repro.

---

_Closed by @AlexWaygood on 2025-09-07 12:57_

---
