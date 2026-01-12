```yaml
number: 17371
title: "[ty] infer function's return type"
type: pull_request
state: open
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: infer-return-type
created_at: 2025-04-13T05:48:17Z
updated_at: 2025-12-31T03:33:48Z
url: https://github.com/astral-sh/ruff/pull/17371
synced_at: 2026-01-12T15:56:01Z
```

# [ty] infer function's return type

---

_@mtshiba_

## Summary

This PR closes astral-sh/ty#128.

`FunctionType::infer_return_type` is added to infer the return type of a function when its return type is not specified.

## TODOs

- [x] infer simple function's return type
~~- [ ] infer generator's return type~~
~~- [ ] infer coroutine's return type~~
~~- [ ] infer lambda function's return type~~

## Test Plan

New test cases are added to `mdtest/function/return_type.md`.

---

_Review requested from @carljm by @mtshiba on 2025-04-13 05:48_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-04-13 05:48_

---

_Review requested from @sharkdp by @mtshiba on 2025-04-13 05:48_

---

_Review requested from @dcreager by @mtshiba on 2025-04-13 05:48_

---

_Comment by @github-actions[bot] on 2025-04-13 05:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
+ zipp/__init__.py:373:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | str | None`
+ zipp/__init__.py:433:16: error[no-matching-overload] No overload of function `join` matches arguments
+ zipp/glob.py:67:16: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 4 diagnostics
+ Found 7 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ src/attr/_make.py:453:19: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Self@from_counting_attr | Unknown`
+ src/attr/_make.py:457:19: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Self@from_counting_attr | Unknown`
- src/attr/_make.py:475:35: warning[possibly-missing-attribute] Attribute `init` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:475:35: warning[possibly-missing-attribute] Attribute `init` may be missing on object of type `Unknown | Self@evolve`
- src/attr/_make.py:475:59: warning[possibly-missing-attribute] Attribute `kw_only` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:475:59: warning[possibly-missing-attribute] Attribute `kw_only` may be missing on object of type `Unknown | Self@evolve`
- src/attr/_make.py:480:37: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:480:37: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Unknown | Self@evolve`
- src/attr/_make.py:487:16: warning[possibly-missing-attribute] Attribute `alias` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:487:16: warning[possibly-missing-attribute] Attribute `alias` may be missing on object of type `Unknown | Self@evolve`
- src/attr/_make.py:489:70: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:489:70: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | Self@evolve`
- src/attr/_make.py:493:19: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:493:19: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | Self@evolve`
- src/attr/_make.py:797:13: error[invalid-assignment] Object of type `ReferenceType[Unknown]` is not assignable to attribute `__attrs_base_of_slotted__` on type `Unknown | type`
+ src/attr/_make.py:797:13: error[invalid-assignment] Object of type `ReferenceType[Unknown | type]` is not assignable to attribute `__attrs_base_of_slotted__` on type `Unknown | type`
+ src/attr/_make.py:809:13: warning[possibly-missing-attribute] Attribute `__attrs_init_subclass__` may be missing on object of type `Unknown | type`
- src/attr/_make.py:1061:13: error[invalid-argument-type] Argument to function `_make_hash_script` is incorrect: Expected `list[Attribute | Unknown]`, found `Unknown | type`
+ src/attr/_make.py:1061:13: error[invalid-argument-type] Argument to function `_make_hash_script` is incorrect: Expected `list[Attribute]`, found `Unknown | type`
- src/attr/_make.py:1608:13: error[invalid-assignment] Object of type `tuple[Attribute | Unknown, ...]` is not assignable to `list[Attribute | Unknown]`
+ src/attr/_make.py:1608:13: error[invalid-assignment] Object of type `tuple[Attribute, ...]` is not assignable to `list[Attribute]`
- src/attr/_make.py:1609:29: warning[possibly-missing-attribute] Attribute `hash` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1609:48: warning[possibly-missing-attribute] Attribute `hash` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1609:67: warning[possibly-missing-attribute] Attribute `eq` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1647:16: warning[possibly-missing-attribute] Attribute `eq_key` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1648:32: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1649:35: warning[possibly-missing-attribute] Attribute `eq_key` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1651:57: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:1654:62: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2189:12: warning[possibly-missing-attribute] Attribute `validator` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2192:21: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2193:26: warning[possibly-missing-attribute] Attribute `on_setattr` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2194:13: warning[possibly-missing-attribute] Attribute `on_setattr` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2198:20: warning[possibly-missing-attribute] Attribute `alias` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2200:34: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2201:48: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2203:12: warning[possibly-missing-attribute] Attribute `converter` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2203:55: warning[possibly-missing-attribute] Attribute `converter` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2204:35: warning[possibly-missing-attribute] Attribute `converter` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2206:25: warning[possibly-missing-attribute] Attribute `converter` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2208:12: warning[possibly-missing-attribute] Attribute `init` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2210:58: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2220:66: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2231:56: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2241:62: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2252:14: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2254:16: warning[possibly-missing-attribute] Attribute `kw_only` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2266:62: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2274:16: warning[possibly-missing-attribute] Attribute `kw_only` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2281:54: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2299:62: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2315:52: warning[possibly-missing-attribute] Attribute `default` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2317:16: warning[possibly-missing-attribute] Attribute `kw_only` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2329:62: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2335:12: warning[possibly-missing-attribute] Attribute `init` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2336:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2337:41: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2373:33: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Attribute | Unknown`
- src/attr/_make.py:2373:60: warning[possibly-missing-attribute] Attribute `init` may be missing on object of type `Attribute | Unknown`
+ src/attr/_make.py:1609:29: error[unresolved-attribute] Object of type `Attribute` has no attribute `hash`
+ src/attr/_make.py:1609:48: error[unresolved-attribute] Object of type `Attribute` has no attribute `hash`
+ src/attr/_make.py:1609:67: error[unresolved-attribute] Object of type `Attribute` has no attribute `eq`
+ src/attr/_make.py:1647:16: error[unresolved-attribute] Object of type `Attribute` has no attribute `eq_key`
+ src/attr/_make.py:1648:32: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:1649:35: error[unresolved-attribute] Object of type `Attribute` has no attribute `eq_key`
+ src/attr/_make.py:1651:57: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:1654:62: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2189:12: error[unresolved-attribute] Object of type `Attribute` has no attribute `validator`
+ src/attr/_make.py:2192:21: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2193:26: error[unresolved-attribute] Object of type `Attribute` has no attribute `on_setattr`
+ src/attr/_make.py:2194:13: error[unresolved-attribute] Object of type `Attribute` has no attribute `on_setattr`
+ src/attr/_make.py:2198:20: error[unresolved-attribute] Object of type `Attribute` has no attribute `alias`
+ src/attr/_make.py:2200:34: error[unresolved-attribute] Object of type `Attribute` has no attribute `default`
+ src/attr/_make.py:2201:48: error[unresolved-attribute] Object of type `Attribute` has no attribute `default`
+ src/attr/_make.py:2203:12: error[unresolved-attribute] Object of type `Attribute` has no attribute `converter`
+ src/attr/_make.py:2203:55: error[unresolved-attribute] Object of type `Attribute` has no attribute `converter`
+ src/attr/_make.py:2204:35: error[unresolved-attribute] Object of type `Attribute` has no attribute `converter`
+ src/attr/_make.py:2206:25: error[unresolved-attribute] Object of type `Attribute` has no attribute `converter`
+ src/attr/_make.py:2208:12: error[unresolved-attribute] Object of type `Attribute` has no attribute `init`
+ src/attr/_make.py:2210:58: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2220:66: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2231:56: error[unresolved-attribute] Object of type `Attribute` has no attribute `default`
+ src/attr/_make.py:2241:62: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2252:14: error[unresolved-attribute] Object of type `Attribute` has no attribute `default`
+ src/attr/_make.py:2254:16: error[unresolved-attribute] Object of type `Attribute` has no attribute `kw_only`
+ src/attr/_make.py:2266:62: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2274:16: error[unresolved-attribute] Object of type `Attribute` has no attribute `kw_only`
+ src/attr/_make.py:2281:54: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2299:62: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2315:52: error[unresolved-attribute] Object of type `Attribute` has no attribute `default`
+ src/attr/_make.py:2317:16: error[unresolved-attribute] Object of type `Attribute` has no attribute `kw_only`
+ src/attr/_make.py:2329:62: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2335:12: error[unresolved-attribute] Object of type `Attribute` has no attribute `init`
+ src/attr/_make.py:2336:16: error[unresolved-attribute] Object of type `Attribute` has no attribute `type`
+ src/attr/_make.py:2337:41: error[unresolved-attribute] Object of type `Attribute` has no attribute `type`
+ src/attr/_make.py:2373:33: error[unresolved-attribute] Object of type `Attribute` has no attribute `name`
+ src/attr/_make.py:2373:60: error[unresolved-attribute] Object of type `Attribute` has no attribute `init`
+ src/attr/_make.py:2641:13: error[invalid-assignment] Object of type `type` is not assignable to `<class 'Attribute'>`
+ src/attr/_make.py:3038:11: error[invalid-assignment] Object of type `type` is not assignable to `<class 'Factory'>`
+ src/attr/_make.py:3157:13: error[invalid-assignment] Object of type `type` is not assignable to `<class 'Converter'>`
+ src/attr/_make.py:3278:18: error[not-iterable] Object of type `Unknown | _CountingAttr` may not be iterable
+ src/attr/_version_info.py:86:16: error[unsupported-operator] Operator `<` is not supported between objects of type `list[Unknown] | @Todo | tuple[Unknown, ...]` and `Unknown | tuple[Unknown, ...]`
+ src/attr/validators.py:98:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `Unknown | _CountingAttr`
+ src/attr/validators.py:137:16: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:138:53: warning[possibly-missing-attribute] Attribute `pattern` may be missing on object of type `Unknown | _CountingAttr`
+ src/attr/validators.py:206:9: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:240:26: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | _CountingAttr`
+ src/attr/validators.py:342:13: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:345:13: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:396:13: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:400:17: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:402:17: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:468:16: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:544:12: error[unsupported-operator] Operator `>` is not supported between objects of type `int` and `Unknown | _CountingAttr`
+ src/attr/validators.py:573:12: error[unsupported-operator] Operator `<` is not supported between objects of type `int` and `Unknown | _CountingAttr`
+ src/attr/validators.py:602:34: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Divergent, ...]`, found `Unknown | _CountingAttr`
+ src/attr/validators.py:650:13: error[call-non-callable] Object of type `_CountingAttr` is not callable
+ src/attr/validators.py:651:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `Unknown | _CountingAttr`
+ src/attr/validators.py:655:17: warning[possibly-missing-attribute] Attribute `format` may be missing on object of type `Unknown | _CountingAttr`
+ src/attr/validators.py:710:18: error[not-iterable] Object of type `Unknown | _CountingAttr` may not be iterable
+ tests/test_make.py:1365:16: warning[possibly-missing-attribute] Attribute `__attrs_attrs__` may be missing on object of type `Unknown | type`
+ tests/test_make.py:1380:16: warning[possibly-missing-attribute] Attribute `__attrs_attrs__` may be missing on object of type `Unknown | type`
+ tests/test_make.py:1468:30: warning[possibly-missing-attribute] Attribute `echo` may be missing on object of type `Unknown | type`
+ tests/test_make.py:1487:25: warning[possibly-missing-attribute] Attribute `__attrs_attrs__` may be missing on object of type `Unknown | type`
+ tests/test_make.py:3051:30: warning[possibly-missing-attribute] Attribute `__match_args__` may be missing on object of type `Unknown | type`
+ tests/test_make.py:3057:22: warning[possibly-missing-attribute] Attribute `__match_args__` may be missing on object of type `Unknown | type`
+ tests/test_make.py:3060:26: warning[possibly-missing-attribute] Attribute `__match_args__` may be missing on object of type `Unknown | type`
- Found 616 diagnostics
+ Found 649 diagnostics

parso (https://github.com/davidhalter/parso)
+ parso/__init__.py:58:26: error[invalid-argument-type] Argument to bound method `parse` is incorrect: Expected `str | bytes`, found `Unknown | None`
- parso/grammar.py:163:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ parso/grammar.py:199:16: warning[possibly-missing-attribute] Attribute `walk` may be missing on object of type `Unknown | None | Normalizer`
+ parso/grammar.py:203:9: warning[possibly-missing-attribute] Attribute `walk` may be missing on object of type `Unknown | None | Normalizer`
+ parso/grammar.py:204:16: warning[possibly-missing-attribute] Attribute `issues` may be missing on object of type `Unknown | None | Normalizer`
+ parso/pgen2/grammar_parser.py:106:20: error[not-iterable] Object of type `None` is not iterable
+ parso/python/diff.py:423:17: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ parso/python/diff.py:621:16: warning[possibly-missing-attribute] Attribute `tree_node` may be missing on object of type `Unknown | None`
+ parso/python/diff.py:622:9: warning[possibly-missing-attribute] Attribute `add_tree_nodes` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:783:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:784:26: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:784:54: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:1166:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:1167:34: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:1167:62: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | None`
- parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/tokenize.py:109:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Unknown, ...]` and value of type `Unknown` on object of type `dict[PythonVersionInfo, TokenCollection]`
+ parso/python/tokenize.py:109:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Unknown, ...]` and value of type `TokenCollection` on object of type `dict[PythonVersionInfo, TokenCollection]`
+ parso/python/tokenize.py:588:31: error[invalid-assignment] Object of type `Pattern[Unknown] | None` is not assignable to `Pattern[Unknown]`
+ parso/python/tree.py:135:21: warning[possibly-missing-attribute] Attribute `token_type` may be missing on object of type `(Unknown & ~None) | NodeOrLeaf`
+ parso/python/tree.py:1129:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1130:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1131:26: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1149:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/tree.py:447:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `Unknown | None`
- Found 199 diagnostics
+ Found 219 diagnostics

kornia (https://github.com/kornia/kornia)
+ kornia/feature/dedode/transformer/dinov2.py:317:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["x_norm_clstoken"]` on object of type `list[Unknown]`
- Found 758 diagnostics
+ Found 759 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/cli/cmdoptions.py:123:51: error[invalid-argument-type] Argument to function `get_file_content` is incorrect: Expected `PipSession`, found `Self@__enter__ | Unknown`
+ src/pip/_internal/commands/index.py:119:17: error[invalid-argument-type] Argument to bound method `_build_package_finder` is incorrect: Expected `PipSession`, found `Self@__enter__ | Unknown`
+ src/pip/_internal/commands/list.py:245:58: error[invalid-argument-type] Argument to bound method `_build_package_finder` is incorrect: Expected `PipSession`, found `Self@__enter__ | Unknown`
+ src/pip/_internal/index/collector.py:311:9: error[invalid-argument-type] Argument is incorrect: Expected `bytes`, found `(Unknown & ~Literal[False]) | None | (bytes & ~AlwaysFalsy) | Literal[b""]`
- src/pip/_vendor/cachecontrol/adapter.py:162:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/pip/_vendor/cachecontrol/serialize.py:35:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/pip/_vendor/cachecontrol/serialize.py:36:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `None | bytes | Unknown`
+ src/pip/_vendor/cachecontrol/serialize.py:146:47: error[invalid-argument-type] Argument to bound method `prepare_response` is incorrect: Expected `Mapping[str, Any]`, found `int | Unknown | None | ... omitted 9 union elements`
+ src/pip/_vendor/distlib/compat.py:1127:22: error[call-non-callable] Object of type `ModuleType` is not callable
+ src/pip/_vendor/distlib/resources.py:196:33: error[not-iterable] Object of type `Unknown | Self@__get__` may not be iterable
+ src/pip/_vendor/distlib/resources.py:196:33: warning[possibly-missing-attribute] Attribute `resources` may be missing on object of type `Unknown | ResourceContainer | Resource`
+ src/pip/_vendor/distlib/resources.py:202:28: warning[possibly-missing-attribute] Attribute `is_container` may be missing on object of type `None | ResourceContainer | Resource | Unknown`
+ src/pip/_vendor/msgpack/fallback.py:540:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | SupportsBytes | Buffer`, found `None | Unknown | int | bytearray`
+ src/pip/_vendor/msgpack/fallback.py:542:23: warning[possibly-missing-attribute] Attribute `decode` may be missing on object of type `None | Unknown | int | bytearray`
+ src/pip/_vendor/msgpack/fallback.py:545:26: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | SupportsBytes | Buffer`, found `None | Unknown | int | bytearray`
+ src/pip/_vendor/msgpack/fallback.py:548:49: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | SupportsBytes | Buffer`, found `None | Unknown | int | bytearray`
+ src/pip/_vendor/msgpack/fallback.py:558:48: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | SupportsBytes | Buffer`, found `None | Unknown | int | bytearray`
+ src/pip/_vendor/pkg_resources/__init__.py:2308:28: error[invalid-argument-type] Argument to function `distributions_from_metadata` is incorrect: Expected `str`, found `str | bytes`
+ src/pip/_vendor/pkg_resources/__init__.py:2308:28: error[invalid-argument-type] Argument to function `find_distributions` is incorrect: Expected `str`, found `str | bytes`
+ src/pip/_vendor/pkg_resources/__init__.py:2828:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | str` and value of type `Self@parse | Unknown` on object of type `dict[str, Self@parse_group]`
+ src/pip/_vendor/pkg_resources/__init__.py:2852:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[str, Self@parse_group] | Unknown` on object of type `dict[str, dict[str, Self@parse_map]]`
+ src/pip/_vendor/pkg_resources/__init__.py:2941:16: error[unsupported-operator] Operator `<` is not supported between two objects of type `tuple[Unknown | Version, Unknown | int, Unknown | str, Unknown | str | None, (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""], (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""]]`
+ src/pip/_vendor/pkg_resources/__init__.py:2944:16: error[unsupported-operator] Operator `<=` is not supported between two objects of type `tuple[Unknown | Version, Unknown | int, Unknown | str, Unknown | str | None, (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""], (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""]]`
+ src/pip/_vendor/pkg_resources/__init__.py:2947:16: error[unsupported-operator] Operator `>` is not supported between two objects of type `tuple[Unknown | Version, Unknown | int, Unknown | str, Unknown | str | None, (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""], (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""]]`
+ src/pip/_vendor/pkg_resources/__init__.py:2950:16: error[unsupported-operator] Operator `>=` is not supported between two objects of type `tuple[Unknown | Version, Unknown | int, Unknown | str, Unknown | str | None, (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""], (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""]]`
- src/pip/_vendor/pkg_resources/__init__.py:3466:40: error[invalid-argument-type] Argument to bound method `contains` is incorrect: Expected `Version | str`, found `(str & ~Distribution) | (tuple[str, ...] & ~Distribution) | Unknown`
+ src/pip/_vendor/pkg_resources/__init__.py:3466:40: error[invalid-argument-type] Argument to bound method `contains` is incorrect: Expected `Version | str`, found `str | (tuple[str, ...] & ~Distribution) | Unknown`
+ src/pip/_vendor/pygments/lexers/python.py:362:42: error[invalid-argument-type] Argument to function `fstring_rules` is incorrect: Expected `PythonLexer`, found `Any | _TokenType`
+ src/pip/_vendor/pygments/lexers/python.py:363:42: error[invalid-argument-type] Argument to function `fstring_rules` is incorrect: Expected `PythonLexer`, found `Any | _TokenType`
+ src/pip/_vendor/pygments/lexers/python.py:364:45: error[invalid-argument-type] Argument to function `innerstring_rules` is incorrect: Expected `PythonLexer`, found `Any | _TokenType`
+ src/pip/_vendor/pygments/lexers/python.py:365:45: error[invalid-argument-type] Argument to function `innerstring_rules` is incorrect: Expected `PythonLexer`, found `Any | _TokenType`
+ src/pip/_vendor/pygments/lexers/python.py:611:45: error[invalid-argument-type] Argument to function `innerstring_rules` is incorrect: Expected `Python2Lexer`, found `Any | _TokenType`
+ src/pip/_vendor/pygments/lexers/python.py:612:45: error[invalid-argument-type] Argument to function `innerstring_rules` is incorrect: Expected `Python2Lexer`, found `Any | _TokenType`
+ src/pip/_vendor/pygments/sphinxext.py:108:26: warning[possibly-missing-attribute] Attribute `filenames` may be missing on object of type `Unknown | None`
+ src/pip/_vendor/pygments/sphinxext.py:108:48: warning[possibly-missing-attribute] Attribute `alias_filenames` may be missing on object of type `Unknown | None`
+ src/pip/_vendor/pygments/sphinxext.py:111:46: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Unknown | None`
+ src/pip/_vendor/requests/models.py:121:26: error[not-iterable] Object of type `None | list[Unknown]` may not be iterable
+ src/pip/_vendor/requests/models.py:155:27: error[not-iterable] Object of type `None | list[Unknown]` may not be iterable
+ src/pip/_vendor/requests/models.py:173:21: error[not-iterable] Object of type `None | list[Unknown]` may not be iterable
+ src/pip/_vendor/requests/models.py:602:17: error[call-non-callable] Object of type `tuple[str, str]` is not callable
- src/pip/_vendor/requests/models.py:935:41: error[invalid-argument-type] Argument to class `str` is incorrect: Expected `str`, found `Unknown | None`
+ src/pip/_vendor/requests/models.py:935:41: error[invalid-argument-type] Argument to class `str` is incorrect: Expected `str`, found `Unknown | None | Literal["utf-8"]`
+ src/pip/_vendor/requests/sessions.py:80:5: error[no-matching-overload] No overload of bound method `update` matches arguments
+ src/pip/_vendor/requests/utils.py:615:31: error[invalid-argument-type] Argument to class `str` is incorrect: Expected `str`, found `None | Unknown | Literal["ISO-8859-1", "utf-8"]`
+ src/pip/_vendor/rich/syntax.py:167:27: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["#"]` and `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[True]`
+ src/pip/_vendor/rich/syntax.py:168:29: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["#"]` and `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[True]`
+ src/pip/_vendor/rich/syntax.py:169:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | None`, found `Unknown | str | None | bool`
+ src/pip/_vendor/rich/syntax.py:170:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | None`, found `Unknown | str | None | bool`
+ src/pip/_vendor/rich/syntax.py:171:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | None`, found `Unknown | str | None | bool`
- src/pip/_vendor/urllib3/connectionpool.py:785:21: warning[possibly-missing-attribute] Attribute `proxy` may be missing on object of type `None | Unknown`
+ src/pip/_vendor/urllib3/connectionpool.py:785:21: warning[possibly-missing-attribute] Attribute `proxy` may be missing on object of type `None | Unknown | HTTPConnection`
- src/pip/_vendor/urllib3/connectionpool.py:786:21: warning[possibly-missing-attribute] Attribute `proxy` may be missing on object of type `None | Unknown`
+ src/pip/_vendor/urllib3/connectionpool.py:786:21: warning[possibly-missing-attribute] Attribute `proxy` may be missing on object of type `None | Unknown | HTTPConnection`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:136:5: warning[possibly-missing-attribute] Attribute `SecItemImport` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:146:5: warning[possibly-missing-attribute] Attribute `SecItemImport` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:148:5: warning[possibly-missing-attribute] Attribute `SecCertificateGetTypeID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:149:5: warning[possibly-missing-attribute] Attribute `SecCertificateGetTypeID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:151:5: warning[possibly-missing-attribute] Attribute `SecIdentityGetTypeID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:152:5: warning[possibly-missing-attribute] Attribute `SecIdentityGetTypeID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:154:5: warning[possibly-missing-attribute] Attribute `SecKeyGetTypeID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:155:5: warning[possibly-missing-attribute] Attribute `SecKeyGetTypeID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:157:5: warning[possibly-missing-attribute] Attribute `SecCertificateCreateWithData` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:158:5: warning[possibly-missing-attribute] Attribute `SecCertificateCreateWithData` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:160:5: warning[possibly-missing-attribute] Attribute `SecCertificateCopyData` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:161:5: warning[possibly-missing-attribute] Attribute `SecCertificateCopyData` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:163:5: warning[possibly-missing-attribute] Attribute `SecCopyErrorMessageString` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:164:5: warning[possibly-missing-attribute] Attribute `SecCopyErrorMessageString` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:166:5: warning[possibly-missing-attribute] Attribute `SecIdentityCreateWithCertificate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:171:5: warning[possibly-missing-attribute] Attribute `SecIdentityCreateWithCertificate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:173:5: warning[possibly-missing-attribute] Attribute `SecKeychainCreate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:181:5: warning[possibly-missing-attribute] Attribute `SecKeychainCreate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:183:5: warning[possibly-missing-attribute] Attribute `SecKeychainDelete` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:184:5: warning[possibly-missing-attribute] Attribute `SecKeychainDelete` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:186:5: warning[possibly-missing-attribute] Attribute `SecPKCS12Import` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:191:5: warning[possibly-missing-attribute] Attribute `SecPKCS12Import` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:198:5: warning[possibly-missing-attribute] Attribute `SSLSetIOFuncs` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:199:5: warning[possibly-missing-attribute] Attribute `SSLSetIOFuncs` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:201:5: warning[possibly-missing-attribute] Attribute `SSLSetPeerID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:202:5: warning[possibly-missing-attribute] Attribute `SSLSetPeerID` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:204:5: warning[possibly-missing-attribute] Attribute `SSLSetCertificate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:205:5: warning[possibly-missing-attribute] Attribute `SSLSetCertificate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:207:5: warning[possibly-missing-attribute] Attribute `SSLSetCertificateAuthorities` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:208:5: warning[possibly-missing-attribute] Attribute `SSLSetCertificateAuthorities` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:210:5: warning[possibly-missing-attribute] Attribute `SSLSetConnection` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:211:5: warning[possibly-missing-attribute] Attribute `SSLSetConnection` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:213:5: warning[possibly-missing-attribute] Attribute `SSLSetPeerDomainName` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:214:5: warning[possibly-missing-attribute] Attribute `SSLSetPeerDomainName` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:216:5: warning[possibly-missing-attribute] Attribute `SSLHandshake` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:217:5: warning[possibly-missing-attribute] Attribute `SSLHandshake` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:219:5: warning[possibly-missing-attribute] Attribute `SSLRead` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:220:5: warning[possibly-missing-attribute] Attribute `SSLRead` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:222:5: warning[possibly-missing-attribute] Attribute `SSLWrite` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:223:5: warning[possibly-missing-attribute] Attribute `SSLWrite` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:225:5: warning[possibly-missing-attribute] Attribute `SSLClose` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:226:5: warning[possibly-missing-attribute] Attribute `SSLClose` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:228:5: warning[possibly-missing-attribute] Attribute `SSLGetNumberSupportedCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:229:5: warning[possibly-missing-attribute] Attribute `SSLGetNumberSupportedCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:231:5: warning[possibly-missing-attribute] Attribute `SSLGetSupportedCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:236:5: warning[possibly-missing-attribute] Attribute `SSLGetSupportedCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:238:5: warning[possibly-missing-attribute] Attribute `SSLSetEnabledCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:243:5: warning[possibly-missing-attribute] Attribute `SSLSetEnabledCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:245:5: error[unresolved-attribute] Unresolved attribute `argtype` on type `_NamedFuncPointer`.
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:245:5: warning[possibly-missing-attribute] Attribute `SSLGetNumberEnabledCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:246:5: warning[possibly-missing-attribute] Attribute `SSLGetNumberEnabledCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:248:5: warning[possibly-missing-attribute] Attribute `SSLGetEnabledCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:253:5: warning[possibly-missing-attribute] Attribute `SSLGetEnabledCiphers` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:255:5: warning[possibly-missing-attribute] Attribute `SSLGetNegotiatedCipher` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:256:5: warning[possibly-missing-attribute] Attribute `SSLGetNegotiatedCipher` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:258:5: warning[possibly-missing-attribute] Attribute `SSLGetNegotiatedProtocolVersion` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:262:5: warning[possibly-missing-attribute] Attribute `SSLGetNegotiatedProtocolVersion` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:264:5: warning[possibly-missing-attribute] Attribute `SSLCopyPeerTrust` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:265:5: warning[possibly-missing-attribute] Attribute `SSLCopyPeerTrust` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:267:5: warning[possibly-missing-attribute] Attribute `SecTrustSetAnchorCertificates` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:268:5: warning[possibly-missing-attribute] Attribute `SecTrustSetAnchorCertificates` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:270:5: error[unresolved-attribute] Unresolved attribute `argstypes` on type `_NamedFuncPointer`.
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:270:5: warning[possibly-missing-attribute] Attribute `SecTrustSetAnchorCertificatesOnly` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:271:5: warning[possibly-missing-attribute] Attribute `SecTrustSetAnchorCertificatesOnly` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:273:5: warning[possibly-missing-attribute] Attribute `SecTrustEvaluate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:274:5: warning[possibly-missing-attribute] Attribute `SecTrustEvaluate` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:276:5: warning[possibly-missing-attribute] Attribute `SecTrustGetCertificateCount` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:277:5: warning[possibly-missing-attribute] Attribute `SecTrustGetCertificateCount` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:279:5: warning[possibly-missing-attribute] Attribute `SecTrustGetCertificateAtIndex` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:280:5: warning[possibly-missing-attribute] Attribute `SecTrustGetCertificateAtIndex` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:282:5: warning[possibly-missing-attribute] Attribute `SSLCreateContext` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:287:5: warning[possibly-missing-attribute] Attribute `SSLCreateContext` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:289:5: warning[possibly-missing-attribute] Attribute `SSLSetSessionOption` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:290:5: warning[possibly-missing-attribute] Attribute `SSLSetSessionOption` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:292:5: warning[possibly-missing-attribute] Attribute `SSLSetProtocolVersionMin` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:293:5: warning[possibly-missing-attribute] Attribute `SSLSetProtocolVersionMin` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:295:5: warning[possibly-missing-attribute] Attribute `SSLSetProtocolVersionMax` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:296:5: warning[possibly-missing-attribute] Attribute `SSLSetProtocolVersionMax` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:299:9: warning[possibly-missing-attribute] Attribute `SSLSetALPNProtocols` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:300:9: warning[possibly-missing-attribute] Attribute `SSLSetALPNProtocols` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:305:5: warning[possibly-missing-attribute] Attribute `SecCopyErrorMessageString` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:306:5: warning[possibly-missing-attribute] Attribute `SecCopyErrorMessageString` may be missing on object of type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:308:5: error[invalid-assignment] Object of type `type[_CFunctionType]` is not assignable to attribute `SSLReadFunc` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:309:5: error[invalid-assignment] Object of type `type[_CFunctionType]` is not assignable to attribute `SSLWriteFunc` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:310:5: error[invalid-assignment] Object of type `type[_Pointer[c_void_p]]` is not assignable to attribute `SSLContextRef` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:311:5: error[invalid-assignment] Object of type `<class 'c_uint32'>` is not assignable to attribute `SSLProtocol` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:312:5: error[invalid-assignment] Object of type `<class 'c_uint32'>` is not assignable to attribute `SSLCipherSuite` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:313:5: error[invalid-assignment] Object of type `type[_Pointer[c_void_p]]` is not assignable to attribute `SecIdentityRef` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:314:5: error[invalid-assignment] Object of type `type[_Pointer[c_void_p]]` is not assignable to attribute `SecKeychainRef` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:315:5: error[invalid-assignment] Object of type `type[_Pointer[c_void_p]]` is not assignable to attribute `SecTrustRef` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:316:5: error[invalid-assignment] Object of type `<class 'c_uint32'>` is not assignable to attribute `SecTrustResultType` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:317:5: error[invalid-assignment] Object of type `<class 'c_uint32'>` is not assignable to attribute `SecExternalFormat` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:318:5: error[invalid-assignment] Object of type `<class 'c_int32'>` is not assignable to attribute `OSStatus` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:320:5: error[invalid-assignment] Object of type `_Pointer[c_void_p]` is not assignable to attribute `kSecImportExportPassphrase` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:321:9: error[invalid-argument-type] Argument to bound method `in_dll` is incorrect: Expected `CDLL`, found `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:323:5: error[invalid-assignment] Object of type `_Pointer[c_void_p]` is not assignable to attribute `kSecImportItemIdentity` on type `CDLL | None`
+ src/pip/_vendor/urllib3/contrib/_securetransport/bindings.py:324:9: error[invalid-argument-type] Argument to bound method `in_dll` is 

... (truncated 65012 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     struct fields = ~11MB
+     struct fields = ~12MB
-     memo fields = ~108MB
+     memo fields = ~113MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~167MB
+     memo metadata = ~176MB


```

</details>




---

_Comment by @mtshiba on 2025-04-13 07:35_

Most of the changes detected by mypy_primer appear to be due to ill-typed code errors now being caught correctly.

Code like the following now produces false positive warnings, but this may be resolved by type narrowing of the instance attributes, which I'm currently working on as a separate issue.

```python
from typing import reveal_type

class C:
    def __init__(self):
        self.x: int = 1

class D:
    def __init__(self):
        self.c: C | None = None

def get_d():
    return D()

d = get_d()
d.c = C()
# TODO: no error
# error: [possibly-unbound-attribute] "Attribute `x` on type `C | None` is possibly unbound"
reveal_type(d.c.x)  # revealed: int
```

---

_Label `red-knot` added by @AlexWaygood on 2025-04-13 14:34_

---

_Comment by @sharkdp on 2025-04-14 07:30_

Thank you for looking into this!

> but this may be resolved by type narrowing of the instance attributes, which I'm currently working on as a separate issue

I'm assuming you're talking about https://github.com/astral-sh/ty/issues/164? Good to know that you are already working on this, because I planned to do that this week :smile:. I think it would be great to work on this first. Narrowing on attribute expressions will lead to a large reduction in false positives. And it seems like it would help us evaluate the ecosystem changes in this PR more clearly, if we implement narrowing on attribute assignments first.

---

_Comment by @mtshiba on 2025-04-15 16:55_

If a function contains a yield expression, i.e. if it is a generator, current type inference is incorrect. But supporting this requires solving another issues, so I will leave it as a TODO for now.

As for return type inference for normal functions, I think the work is completed.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-15 17:42_

---

_Renamed from "[red-knot] infer function's return type" to "[ty] infer function's return type" by @MichaReiser on 2025-05-08 16:11_

---

_Comment by @carljm on 2025-05-09 00:44_

Sorry that I haven't gotten around to reviewing this PR yet. There's just been a lot to do, and providing this feature is lower on my priority list than some other features. But I realize that places a burden on you to keep it up to date with (rapidly changing) main branch. But I will try to find time to review it soon.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:560 on 2025-05-22 22:16_

This should be an invalid-override error (if we detected invalid overrides, which we don't yet). The base method accepts an argument of any type; this method accepts only ints.

I think that is fixable by giving `T` an upper bound above in the base method: `def g[T: int]`

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:330 on 2025-05-22 22:18_

These results are unsound; we would allow the override of `D.g` over `C.g` only because of the lack of return annotation on `D.g`, but the return type we infer for `D.g` creates a Liskov violation: `int` is not a subtype of `Literal[1]`.

I think if we infer a return type for a method which is not assignable to the return types of any overridden method, we should emit an invalid-override diagnostic on the overriding method. (This can be a TODO, doesn't have to be implemented in this PR.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:344 on 2025-05-22 22:21_

We could also have a TODO (or implement) that if `H` were final we could safely infer `Never` as its return type.

```suggestion
# We can only reveal `Literal[2]` here, not `Never`, because of potential
# subclasses of `H`, which are bound by the annotated return types of
# `F.f` and `G.f`, but are not bound by our inference on `H.f`.
reveal_type(H().f())  # revealed: Literal[2]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4243 on 2025-05-22 22:46_

```suggestion
    /// Returns the inferred return type of `self` if it is a function literal / bound method.
```

---

_@carljm reviewed on 2025-05-22 22:56_

Partial review here; plane is landing now so submitting the comments I have. Will come back to this later.

Thank you for working on this, and sorry for the slow review! It's a very useful feature.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4318 on 2025-06-10 22:41_

naming nit: functions are verbs (and we prefer `type` over `ty`), let's name this callback `infer_return_type` not `inferred_return_ty` (and same for all similar locations)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:597 on 2025-06-10 22:48_

On second look, I think we should have a TODO here that this should be an invalid inheritance error (when we enforce Liskov), because `F.f` is not assignable to `G.f`. This matches what both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=168022fc3037fe95cfacb39af9f54a7b) and [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNQMaXkDOTUAYgFzVQ9QAmJYFGAAKJiUrAAlFAC0APkLEyVANoBGADRQATAF0uvI1BAkYAVxAoo62g2asA4od4Cho8ZJkKlpCpVUdbQBmA25jHlMLKyhgu0YWKAAJETZtRykXHgQHIA) do, and I think it's correct: the MRO of `H` will be serialized with `F` below `G`, therefore `F.f` must be assignable to `G.f`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:354 on 2025-06-10 22:49_

And given the above, I think this can just reveal `Literal[1, 2]` (that is, the return type of `F.f`). If we enforce Liskov correctly, we don't need to do any intersecting, we can just look for the nearest `f` in the MRO, which will simplify the implementation a lot.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6986 on 2025-06-10 22:50_

Given the above comments on the tests, I think we don't need this method -- it can be replaced by a simple linear MRO lookup.

---

_@carljm reviewed on 2025-06-10 22:51_

Ok, reviewed this again (and more fully). I think it's pretty close, but we can simplify the handling of multiple inheritance.

---

_Comment by @codspeed-hq[bot] on 2025-06-21 06:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #17371 will **degrade performance by 39.89%**

<sub>Comparing <code>mtshiba:infer-return-type</code> (f5e5165) with <code>main</code> (f619783)</sub>



### Summary

`❌ 4` regressions  
`✅ 48` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 52.1 s | 86.6 s | -39.89% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 64.8 s | 68.5 s | -5.51% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 8.1 s | 8.5 s | -5.26% |
| ❌ | Simulation | [`` attrs ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aattrs%3A%3Aproject%3A%3Aattrs&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 436.2 ms | 498.8 ms | -12.54% |


---

_Comment by @mtshiba on 2025-06-23 03:37_

Since ty has type inference by fixed-point iteration, I thought that this PR would be able to infer the return type for recursive functions without any additional work. But, it seems that this is incorrect in some cases.

```python
def divergent(value):
    if type(value) is tuple:
        return (divergent(value[0]),)
    else:
        return None
```

The return type inference for this function diverges because it becomes `tuple[tuple[tuple[...] | None] | None] | None`. We need to detect and suppress this kind of divergence.

A formally complete solution to the problem is to synthesize a recursive type, that is, to infer it as follows:

```python
type NestedTuple[T] = T | tuple[NestedTuple[T]]

def divergent[T](value: NestedTuple[T]) -> NestedTuple[None]:
    if type(value) is tuple:
        return (divergent(value[0]),)
    else:
        return None
```

I'm not sure if this is actually possible in Python's type system. ML languages ​​provide complete type inference, including for recursive functions, but it may only be possible due to the nature of the lack of narrowing and subtyping.

So a practical solution would be to detect unstoppable recursion at some point and make the resulting type some kind of gradual type.

This is still troublesome, simply stopping the recursion at some point and falling back to `Unknown` will still not allow a fixed point to be reached (in the case of a divergent type, the next revision will end up with a type like `tuple[Unknown] | None`).
Also, functions with many `return`s will cause the resulting union type to grow explosively within cycles, so if the recursion is not detected within the first few cycles, a significant amount of time/memory will be consumed.

One approach I'm thinking of is to classify recursive functions as divergent / non-divergent (such as `fibonacci/even/odd`) and monitor them with `TypeInferenceBuilder`. If the former is detected, replace the divergent part of the union of the function's return type with `Unknown` in both `Type::infer_return_type` and `TypeInference::infer_return_type`. This attempts to discover divergence syntactically.

Another approach is to limit not only the number of elements (= "width") but also the "depth" when building union types.
We currently limit the maximum number of literals a union type can have to 200. This is mainly for an optimization, but it also helps to prevent union types from diverging. So what about limiting the depth of a union type as well? (or limit the total number of types in a union type ≃ depth * width)

---

_Comment by @MichaReiser on 2025-06-23 09:09_

This PR seems to blow up the runtime for the large wall-time benchmark.

---

_Comment by @mtshiba on 2025-06-23 14:12_

> This PR seems to blow up the runtime for the large wall-time benchmark.

I suspect this is caused by an exploding union type when trying to infer the return type of a divergent recursive function.

For example, this function is causing a hang: https://github.com/quora/asynq/blob/1bf4f7b5596401000c8db92a5e08a667d11092b2/asynq/async_task.py#L427-L471

---

_Comment by @carljm on 2025-07-02 17:33_

I think the issues we run into here on some recursive functions are closely related to the existing issues we have with recursive type aliases.

---

_Review requested from @MichaReiser by @mtshiba on 2025-07-07 11:18_

---

_Comment by @github-actions[bot] on 2025-07-07 11:42_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:252 on 2025-07-07 14:51_

It's a bit unfortunate that we can't infer `bool` here, but pyright gives the same result.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/primer/bad.txt`:23 on 2025-07-07 14:54_

When I checked locally, no stack overflow occurred in a single thread (it can also be confirmed that there was no stack overflow in ty_walltime).

---

_Comment by @codspeed-hq[bot] on 2025-07-07 14:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?runnerMode=WallTime)

### Merging #17371 will **degrade performances by 32.81%**

<sub>Comparing <code>mtshiba:infer-return-type</code> (706eff8) with <code>main</code> (edeb458)</sub>



### Summary

`❌ 2` regressions  
`✅ 6` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Ainfer-return-type?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 44.1 s | 65.6 s | -32.81% |
| ❌ | `` medium[pandas] `` | 32.5 s | 34 s | -4.19% |


---

_@mtshiba reviewed on 2025-07-07 14:57_

---

_Review comment by @mtshiba on `crates/ruff_benchmark/benches/ty_walltime.rs`:202 on 2025-07-07 15:05_

The number of errors in the sympy checks has increased from about 10000 to 70000. The degradation of the benchmark is likely due to an explosion in the number of errors.

---

_@mtshiba reviewed on 2025-07-07 15:05_

---

_Review requested from @carljm by @mtshiba on 2025-07-07 15:11_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:368 on 2025-07-22 02:16_

I think what we reveal here is not super important, since it's the result of an invalid override, but it is important that we not reveal an unspecialized `T` here. If anything we could just fall back to `Unknown`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:145 on 2025-07-22 02:20_

Maybe let's have the custom `type()` function imported from a stub, so we can keep the clear original intent of the test here, without RTI making it more confusing.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/primer/bad.txt`:23 on 2025-07-22 02:21_

I would really prefer to see if we can eliminate these stack overflows before we land this. Have you looked at stack traces? Do you know what we are doing when we stack overflow?

---

_@carljm reviewed on 2025-07-22 02:31_

The behavior here is looking pretty good to me, overall. Thanks for your continued work on it!

The cycle-detection by manually maintaining a stack of function scopes we are inferring is interesting; I've considered something similar for recursive type aliases, but I've been trying to avoid it. I need to think about that a bit more -- it seems like it can introduce non-determinism, depending on what call we try to infer first?

I'm hesitant to land this while it introduces new stack overflows.

---

_@MichaReiser requested changes on 2025-07-28 09:10_

Based on Carl's feedback

---

_Comment by @github-actions[bot] on 2025-08-01 18:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @mtshiba on 2025-08-04 17:25_

> The cycle-detection by manually maintaining a stack of function scopes we are inferring is interesting; I've considered something similar for recursive type aliases, but I've been trying to avoid it. I need to think about that a bit more -- it seems like it can introduce non-determinism, depending on what call we try to infer first?

As you point out, this change definitely introduces non-determinism that affects the inference results, and I strongly feel that we should look for an alternative way. (I confirmed that even without stack overflows, the number of errors in mypy_primer results fluctuates each time during multi-threaded execution. If I understand correctly, salsa assumes that query functions are pure, and having manual call stack breaks this assumption. This must be the cause of the non-determinism.)




---

_@MichaReiser reviewed on 2025-08-04 17:48_

The out of bound `CallStack` without a proper equality also breaks salsa caching. 

---

_Converted to draft by @carljm on 2025-08-06 21:26_

---

_Comment by @carljm on 2025-08-06 21:26_

Marking this draft for now; please mark ready when its ready for review again. (This just helps streamline my notifications workflow.)

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:7031 on 2025-09-08 07:09_

We now introduce the `Divergent` type for function return type inference, but I think this should also help suppress other divergent type inference.
For example, the inference of this code currently causes a panic. This can also be suppressed with `Divergent`:

```python
class C:
    def f(self, x: "C"):
        self.x = (x.x, None)

c = C()
reveal_type(c.x)  # should be: tuple[Divergent, None]
```

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:311 on 2025-09-08 08:01_

In the current implementation, return type inference is performed when the function is actually called, but it is also possible to calculate the return type when obtaining the signature of the function.
This would make a difference, for example, when using a function as a higher-order function.

```python
reveal_type(f)  # current: def f() -> Unknown
reveal_type(f)  # should be?: def f() -> Literal[1]
```

The reasons for currently choosing the former are as follows:
* There is a question about whether the return type not explicitly specified in the function should be exposed in the signature (pyright does expose it).
* There are many changes, such as the change in the behavior of `CallableTypeOf` (this is less important than the first reason).

---

_Comment by @mtshiba on 2025-09-08 09:26_

The causes of the false positives reported by mypy_primer are as follows:

## incomplete decorator support (sympy)

The arithmetic operations on the `Expr` class are not working correctly. Decorated function inference is not supported yet.

https://github.com/sympy/sympy/blob/5bb8ffb0dd2d21af10d2fa3c0a3710bba4c11d76/sympy/core/expr.py#L249-L252

## submodule resolution issue (scrapy)

The `scrapy.signals` module is being interpreted as a `Tuple[Unknown, Unknown, Unknown]` type. This is likely because `__getattr__` defined in `scrapy/__init__.py` returns a three-element tuple. ~~However, it is unclear to me why attribute resolution via `__getattr__` takes precedence due to function return type inference.~~
Previously, `__getattr__` defined in the module did not specify a return type, so `scrapy.signals` was interpreted as `Unknown`, but now it recognizes that `__getattr__` returns a tuple, so an error occurs.

This may be related to https://github.com/astral-sh/ty/issues/1053

## others

The following errors are consistent with the current Python type spec.

* implicit overloading
For example, a function like this:
```python
def f(type):
    if type is int:
        return 1
    elif type is str:
        return "a"

reveal_type(f(int)) # revealed: Literal[1, "a"] | None
f(int) + 1 # error
```

* `__slots__` (attrs)
Attributes defined with `__slots__` are reported as unresolved.

* `setattr` (sympy, etc.)
Attributes set with `setattr` are reported as unresolved. I think it would be possible in principle to make attributes set with a literal string be recognized as class attributes, but no other type checker does this.

---

_Comment by @mtshiba on 2025-09-08 10:15_

The main remaining concern is performance. According to the codspeed report, sympy's check time has worsened by about 30%. But I believe this is a necessary trade-off.

I believe this is due to an increase in the number of checks, that is, there is a cost to addressing errors/inferences that have been overlooked by `Unknown`. In fact, the number of errors reported in sympy has increased from about 10,000 to about 70,000 (for reference, pyright reports approximately 35,000 errors).

Here are the local measurements of the sympy check times for each type checker:

ty(main):
```
$ hyperfine -i "ty check"

Benchmark 1: ty check
  Time (mean ± σ):      1.901 s ±  0.118 s    [User: 20.211 s, System: 3.641 s]
  Range (min … max):    1.799 s …  2.188 s    10 runs
```

ty(#17371):
```
$ hyperfine -i "ty check"

Benchmark 1: ty check
  Time (mean ± σ):      7.290 s ±  1.341 s    [User: 28.048 s, System: 3.558 s]
  Range (min … max):    5.232 s …  9.058 s    10 runs
```

pyright:
```
$ hyperfine -i "pyright"

Benchmark 1: pyright
  Time (mean ± σ):     118.239 s ±  7.783 s    [User: 163.368 s, System: 4.845 s]
  Range (min … max):   110.686 s … 139.539 s    10 runs
```

mypy:
```
$ hyperfine -i "mypy -p sympy --no-incremental"

Benchmark 1: mypy -p sympy --no-incremental
  Time (mean ± σ):     35.555 s ±  2.398 s    [User: 31.747 s, System: 3.255 s]
  Range (min … max):   33.484 s … 39.311 s    10 runs
```

mypy(check-untyped-defs):
```
$ hyperfine -i "mypy -p sympy --no-incremental --check-untyped-defs"

Benchmark 1: mypy -p sympy --no-incremental --check-untyped-defs
  Time (mean ± σ):     69.795 s ±  0.740 s    [User: 66.414 s, System: 2.764 s]
  Range (min … max):   68.843 s … 70.830 s    10 runs
```

pyright is generally considered faster than mypy, but its inferiority in sympy's inspection time suggests that return type inference and its impact are heavy in some cases.

---

_@mtshiba reviewed on 2025-09-08 10:15_

---

_Marked ready for review by @mtshiba on 2025-09-08 10:21_

---

_Review requested from @carljm by @mtshiba on 2025-09-08 12:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/corpus/divergent.py`:1 on 2025-09-09 00:17_

The cases in this file look like cases that would be better to have as mdtests than just corpus tests?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:63 on 2025-09-09 00:19_

Same as above.
```suggestion
async def elements(n):
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:21 on 2025-09-09 00:20_

It's fine if async generator type inference support is a TODO, but the "semantic syntax errors" test file doesn't seem like the right place to record that TODO (better to have a failing test with TODO in the return type inference tests), and I don't understand why we need to add the explicit `-> Unknown` annotation here (the test seems to pass without it.)

```suggestion
async def elements(n):
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:341 on 2025-09-09 00:20_

```suggestion
async def elements(n):
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:311 on 2025-09-09 00:21_

It seems fine to leave this for a future PR if it results in a lot of test changes needed, but I think we should probably be consistent here. If we will infer the return type of a call to `f()` as `Literal[1]`, then we should also consider its signature to have a return type of `Literal[1]`. So I think this could be upgraded to a TODO.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:481 on 2025-09-09 00:37_

This doesn't seem to match what is implemented.

I don't think it's critical what we do in case of error (except of course that we should throw an error, which is captured above.) So I think this line could probably just be deleted?
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6568 on 2025-09-09 00:53_

Can this method be replaced by using the existing `any_over_type` function, which searches a type given any predicate function?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7031 on 2025-09-09 00:59_

This is really useful and I think we have a lot of other urgent needs for it. Do you think it could be worthwhile to extract this type as a separate PR in which we make use of it in a simpler case (like the one you show here), to simplify review?

I do think this PR for function return-type inference is close, and it will be great to land it soon, but there are a few things here we may still want to iterate on, and it would be great to have access to the `Divergent` type as soon as possible to help with addressing other recursion panics comprehensively.

---

_@carljm reviewed on 2025-09-09 01:30_

---

_@mtshiba reviewed on 2025-09-09 04:35_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:7031 on 2025-09-09 04:35_

OK, I created #20312.

---

_@mtshiba reviewed on 2025-09-09 12:04_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/corpus/divergent.py`:1 on 2025-09-09 12:04_

These cases are essentially subtle variations of functions already described in [return_type.md](https://github.com/mtshiba/ruff/blob/4f68d19e4e213720891a3089b44f64e958060da3/crates/ty_python_semantic/resources/mdtest/function/return_type.md#free-function) or things that cause panics discovered during debugging, added to prevent regressions rather than to explain ty's behavior, so they may be a bit verbose to describe in mdtest.



---

_@mtshiba reviewed on 2025-09-09 13:05_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:21 on 2025-09-09 13:05_

This appears to be a temporary workaround that was necessary before adding the handling to intentionally set the return type of async functions to `Unknown`. This annotation is indeed no longer needed, and I have added to `return_type.md` that return type inference for async functions is a TODO.

---

_Review requested from @carljm by @mtshiba on 2025-09-09 17:34_

---

_Comment by @mtshiba on 2025-09-11 10:10_

Wait, we might be able to get a better result.

Currently, we converge the return type by replacing "types containing `Divergent` types" (let's call these recursive types) with `Divergent` types. However, this is a bit unsatisfactory because it loses some of the structure of the return type.

So instead, we replace the recursive type contained within the "type containing a recursive type" (let's call this a nested recursive type) with `Divergent` types.

For example, consider the following function:

```python
def f(n):
    if n == 0:
        return 0
    elif n % 2 == 0:
        return [f(n-1)]
    else:
        return (f(n-1),)
```

The return type of this function is `μ X. Literal[0] | list[X] | tuple[X]` according to recursive type theory.
The type variable `X` in this recursive type corresponds to our `Divergent`.

At the 0th iteration, we obtain `Literal[0] | tuple[Divergent] | list[Divergent]`. We leave the recursive type as is.
At the 1st iteration, we obtain `Literal[0] | tuple[Literal[0] | tuple[Divergent] | list[Divergent]] | list[Literal[0] | tuple[Divergent] | list[Divergent]]`. Since this is a nested recursive type, we will "lower" it.

```
Literal[0] | tuple[Literal[0] | tuple[Divergent] | list[Divergent]] | list[Literal[0] | tuple[Divergent] | list[Divergent]]
=> Literal[0] | tuple[Literal[0] | Divergent | Divergent] | list[Literal[0] | Divergent | Divergent]
=> Literal[0] | tuple[Literal[0] | Divergent] | list[Literal[0] | Divergent]
=> Literal[0] | tuple[Divergent] | list[Divergent]
```

Since this is the same as the value from the 0th iteration, it is converged. We have obtained a type with the exact same structure as the recursive type!

The final conversion can be achieved with the following rule: `G[T | Divergent] = G[Divergent]`
This is allowed because `T` is expected to appear at the top level of the return type, as in `T | G[T | Divergent]`.

To summarize the rules here:
* `T | Divergent = T (T is a non-recursive type)`
* `G[R] = G[Divergent] (R is a recursive type, e.g. G[Divergent])`
* `G[T | Divergent] = G[Divergent] (T is a non-recursive type)`

---

_Converted to draft by @mtshiba on 2025-09-11 10:10_

---

_Comment by @carljm on 2025-09-11 14:03_

Yes, I think that makes sense, and corresponds to what I did in the final version of https://github.com/astral-sh/ruff/pull/20333 for tuple literals: if any element of the tuple we are constructing contains Divergent (is a recursive type), it is replaced by Divergent. This allowed me to get the result `Unknown | tuple[Divergent, Literal[0]` in the test in that PR (we can ignore the `Unknown`, it comes from our general policy of unioning un-annotated attribute types with `Unknown`) rather than just `Unknown | Divergent`.

I was planning to next apply this approach to all generic specializations (not just tuple literals) so as to solve Salsa "too many iterations" panics on many more cases of recursive type aliases. I don't want to conflict if you are doing similar work in this PR, but I do think we can and should implement this as a separate PR from return type inference.

---

_Comment by @carljm on 2025-09-12 16:48_

> * `T | Divergent = T (T is a non-recursive type)`
> 
> * `G[R] = G[Divergent] (R is a recursive type, e.g. G[Divergent])`
> 
> * `G[T | Divergent] = G[Divergent] (T is a non-recursive type)`

It seems challenging to implement both rule 1 and rule 3, as they seem contradictory (or at least require some kind of context-sensitive handling). Do you have a plan for this?

---

_Comment by @mtshiba on 2025-09-12 18:14_

> > * `T | Divergent = T (T is a non-recursive type)`
> > * `G[R] = G[Divergent] (R is a recursive type, e.g. G[Divergent])`
> > * `G[T | Divergent] = G[Divergent] (T is a non-recursive type)`
> 
> It seems challenging to implement both rule 1 and rule 3, as they seem contradictory (or at least require some kind of context-sensitive handling). Do you have a plan for this?

I realized that this could actually be implemented as normalization rules for recursive types rather than rules for union types.
In 69953ff67d57a15500511ac9c53a6a53271e9d21, I added a mode to `NormalizedVisitor` that normalizes recursive types. `NormalizedVisitor` retains the nesting level of the type currently visiting, and changes the normalization handling depending on the nesting level (the nesting level increases by one inside a tuple, etc.; the nesting level remains unchanged inside a union).
* If `Divergent` appears at level 0, it is replaced with `Never`. (e.g. `Divergent | int => int`)
* If a recursive type appears at level 1 or above, it is replaced with `Divergent`. (e.g. `tuple[tuple[Divergent]] => tuple[Divergent], tuple[Divergent | int] => tuple[Divergent]`)

---

_Comment by @carljm on 2025-09-12 19:28_

I see how that could work, as a type transform that we would use in place of the simpler "if ty.has_divergent_type(div): return div" type approach. I'm not sure it's a good idea to combine it with the existing `Type::normalized` function. That function transforms types extensively and we never use it on types that we plan to persist, it's only used for checking type equivalence.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3050 on 2025-09-15 16:34_

I wonder if we should build this into Salsa (so that cycle recovery functions receive the previous two values, if any, not just the latest value, and can "fallback" to the union of the previous two), rather than requiring the query implementations to handle it internally.

This way of handling it seems problematic because it means that every single use of `class_member_with_policy` always creates a self-cycle, even if it otherwise wouldn't have created any cycle at all.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-09-15 17:04_

I am seeing that sometimes we have cycles in a query that doesn't have access to any scope (e.g. `Type::member_lookup_with_policy`). I think it might be too restrictive to require a `ScopeId` to always be present as the discriminant for a `DivergentType`?

---

_@carljm reviewed on 2025-09-15 17:04_

---

_@mtshiba reviewed on 2025-09-16 13:06_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/tests/corpus.rs`:174 on 2025-09-16 13:06_

Good news!
I've successfully generalized the recursive function type inference technique and applied it to other divergent type inference (without using `CycleRecoveryAction::Fallback`), that is, I basically solved [#256](https://github.com/astral-sh/ty/issues/256). However, the current implementation seems to significantly degrade performance. I'll investigate this a bit more, then split this PR into a new one organized around closing [#256](https://github.com/astral-sh/ty/issues/256).

---

_@mtshiba reviewed on 2025-09-16 13:09_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:3050 on 2025-09-16 13:09_

Yes, I looked into whether we could do this using salsa's public API, but I couldn't find a safe way to do it other than actually calling the query.

---

_@mtshiba reviewed on 2025-09-16 13:14_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-09-16 13:14_

I've come to believe that the correct modeling here is that the `Divergent` type holds the entire argument set of the divergent query.

The correct mechanism for suppressing divergence is:
* When a tracked function that returns a "struct containing types" enters a cycle, the initial value of the cycle is set not to `Never` (or, a struct containing it), but to the `Divergent` type that contains information that uniquely identifies the point of divergence. (Until now, I have only considered function return type inference, so I thought that scope information was sufficient, but to apply this technique more generally to various tracked functions, `DivergentType` should hold all of the arguments passed by the `cycle_initial` function.)
* From the next cycle, the `Divergent` type becomes the fallback type within that inference.
* To deal with type inference that "oscillates" periodically, the type from the previous cycle is unioned with the current type. This ensures that if the type converges, it will grow monotonically and will no longer oscillate.
* To prevent recursive types from exploding, all types are normalized at the end of the process using the `recursive_type_normalized` method.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/cycle.md`:34 on 2025-09-16 17:18_

Why does `Divergent` not turn into `Never` and disappear in the union on normalization here?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:154 on 2025-09-16 17:25_

The point of this test is simply that we don't apply the special-case narrowing for `builtins.type` just because a custom callable happens to have the name `type`. Having an annotated return type on the custom callable confuses this. Let's remove the `-> TypeOf[int]` here, and restore the test assertions below to what they were originally.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:2451 on 2025-09-16 17:26_

```suggestion
    # TODO invalid override error
    def flip(self) -> "Base":
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3050 on 2025-09-16 17:48_

We are salsa maintainers and should feel free to add/extend the public API as needed to support valid use cases. I think we could easily make it so that cycle-recovery functions receive the last two values instead of just the last one.

Relatedly, one case I've been looking at which still has trouble in this PR is this one:

```py
from typing import Generic, TypeVar

B = TypeVar("B", bound="Base")

class Base(Generic[B]):
    pass
```

The simple "union last two" strategy does not work well here, because we end up inferring `B` as the union of two typevars (with different upper bounds), and that union doesn't simplify. I think we would need to be aware that the divergence is in the upper bound, specifically, and union the upper bound types, not the type of the entire typevar?

(For PEP 695 typevars the upper bounds/constraints are lazy, and I intend to do the same for legacy typevars, which I think would resolve this.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3916 on 2025-09-16 17:51_

As discussed before, this makes every member lookup always a self-cycle, requiring iteration to resolve, which is expensive. I suspect this is where a lot of the performance regression in this PR comes from. I think we need to build this into Salsa such that we do this unioning in the cycle-recovery function, not inside the query function itself, so the query function doesn't have to unconditionally call itself.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-09-16 20:08_

Incorporating a full unique identifier for the current query instance (that is, the query plus its arguments) into the `Divergent` type makes sense to me. I think we could potentially make this simpler / more efficient by having Salsa provide us a unique u32 identifier for the current query instance, to the initial/recovery functions. Salsa already has this ID internally.

Unioning to prevent oscillation makes sense, but I think we need to have Salsa provide the previous value and do this unioning in the cycle-recover function -- we can't create self-cycles in every query function.

There will also be challenges here to do the unioning at the right level. I gave an example in another comment where we can't just union two TypeVar together because we are iterating on the type of their bound. Similarly, if in one iteration we get `list[Never]` and the next iteration we get `list[int]`, we can't just union those together to get `list[Never] | list[int]`, which doesn't simplify. We need to do the unioning at the correct level so we get `list[Never | int] == list[int]`. Still thinking about how we can reliably identify the correct level at which to do this union.

---

_@carljm reviewed on 2025-09-16 20:10_

---

_@mtshiba reviewed on 2025-09-17 08:21_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:3050 on 2025-09-17 08:21_

The code no longer panics since 6b9c889c127dc23d4e1c882697b6f936f83ec029.

In fact, the divergence appears to have been caused by a different reason.

What I did in 6b9c889c127dc23d4e1c882697b6f936f83ec029 was to make all functions that use `{Scope, Expression, Definition}Inference` cycle-safe.
This means that when using these structs, we need to make sure that `infer_{scope, expression, definition}_types` actually converge. If they don't converge, and we're using a value in the middle of the cycle, we need to make sure that the Divergent type propagates.

---

_@mtshiba reviewed on 2025-09-17 08:27_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/cycle.md`:34 on 2025-09-17 08:27_

Fixed in 8593f11170298855490196e7593eed44b6cc6f39.

The cause is related to this [comment](https://github.com/astral-sh/ruff/pull/17371#discussion_r2354717565): `Divergent` types were not propagating properly.

---

_@mtshiba reviewed on 2025-09-17 11:34_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:3050 on 2025-09-17 11:34_

Ah, well, it seems there are still some cases of divergence related to this.

```python
from typing import TypeVar

Rec = TypeVar("Rec", bound="Rec")
```

As you pointed out, the cause is `TypeVar`. `Rec | Rec` is not simplified, resulting in divergence.

Can't we remove `TypeVar`'s dependency on `TypeVarBoundOrConstraintsEvaluation` and `TypeVarDefaultEvaluation` or replace them with stable IDs or methods?

---

_@carljm reviewed on 2025-09-17 17:58_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3050 on 2025-09-17 17:58_

> Can't we remove `TypeVar`'s dependency on `TypeVarBoundOrConstraintsEvaluation` and `TypeVarDefaultEvaluation` or replace them with stable IDs or methods?

Yes, this is what I mentioned above:

> (For PEP 695 typevars the upper bounds/constraints are lazy, and I intend to do the same for legacy typevars, which I think would resolve this.)

It may be the best thing for this particular TypeVar case is just to move forward with that. I can prioritize it as soon as I finish fixing and land the PEP 613 TypeAlias support.



---

_@carljm reviewed on 2025-09-17 18:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-09-17 18:02_

So there are two potential Salsa changes discussed here:

1. Have Salsa provide the two most recent values, not just the most recent value, to the cycle recovery function.
2. Have Salsa provide a unique ID for the current query+arguments to the cycle recovery function.

I think both are technically pretty easy; the latter may just be slightly tricky due to privacy rules: it will probably require either exposing some internal Salsa types, or converting Salsa's internal ID into an opaque u32.

I can work on either or both of these if you agree that they would be useful here.

---

_@mtshiba reviewed on 2025-09-18 13:58_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-09-18 13:58_

Yes, I think both are useful.

But, even if the cycle recovery function has the two most recent values, I think the following oscillating case will not converge.

```python
class C:
    def flip(self) -> "D":
        return D()

class D(C):
    def flip(self) -> "C":
        return C()

def c_or_d(n: int):
    if n == 0:
        return D()
    else:
        # 0th: Divergent.flip() == Divergent
        # 1st: D.flip() == C
        # not converged, fallback: previous | current == Divergent | C == C
        # 2nd: C.flip() == D
        # not converged, fallback: previous | current == C | D == C
        # 3rd: C.flip() == D
        # not converged, fallback: previous | current == C | D == C
        # ...
        return c_or_d(n - 1).flip()
```

The reason the return type ​​doen't converge here is that the types ​​are unioned after the convergence test.
The value joining (union) operation must be performed before the convergence test (and I think the value joining operation and the operation of selecting `CycleRecovery::Iterate` or `CycleRecovery::Fallback` as the cycle recovery are independent).

Therefore, I think it's necessary to specify an additional value joining operation in the tracked function. For example, like this:

```rust
fn return_type_cycle_join<'db>(
    db: &'db dyn Db,
    previous: &Type<'db>,
    current: &Type<'db>
    _self: FunctionType<'db>,
) -> Type<'db> {
    // Recursive type normalization is also required, in reality
    UnionType::from_elements(db, [previous, current])
}

#[tracked]
impl<'db> FunctionType<'db> {
    #[salsa::tracked(cycle_fn=return_type_cycle_recover, cycle_initial=return_type_cycle_initial, cycle_join=return_type_cycle_join)]
    pub(crate) fn infer_return_type(self, db: &'db dyn Db) -> Type<'db> {
        ...
    }
}
```

---

_@mtshiba reviewed on 2025-09-18 14:19_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-09-18 14:19_

Also, while this is just a thought and not required here, we could consider adding a trait called `JoinSemiLattice` to salsa. (The term is borrowed from set theory and is sometimes mentioned in the context of type theory. That is, a type can be thought of as a "join-semilattice", a partially ordered structure with a minimal element ⊥ = Never(Divergent) and a join operation = union)
In our use case, `Type` is clearly `JoinSemiLattice`, and it could also be implemented to `{Scope, Expression, Definition}Inference`.

```rust
trait JoinSemiLattice<'db> {
    // Operation equivalent to `cycle_initial`
    // To generate a `Divergent` type, information about the query input is required.
    // Here, it is treated as an imaginary data type called `QueryId`.
    fn bottom(db: &'db dyn Db, id: QueryId) -> Self; 
    // Operation equivalent to `cycle_join`
    fn join(db: &'db dyn Db, id: QueryId, left: &Self, right: &Self) -> Self;
}
````

```rust
impl<'db> JoinSemiLattice<'db> for Type<'db> { 
    fn bottom(db: &'db dyn Db, id: QueryId) -> Self { 
        Type::divergent(DivergentType::new(db, id)) 
    }
    fn join(db: &'db dyn Db, id: QueryId, left: &Self, right: &Self) -> Self { 
        let visitor = RecursiveTypeNormalizedVisitor::new(db, Self::bottom(db, id));
        UnionType::from_elements(db, [left, right]).recursive_type_normalized(db, &visitor)
    }
}
```

The advantage of implementing this trait is that it allows the result type to define standard handling when a cycle occurs, eliminating the need to specify `cycle_initial/cycle_join` each time we create a tracked function. Another advantage is that it allows us to use the Rust type system's checks without relying on macros. However, since the query inputs are abstracted, this approach is probably optional.

---

_Comment by @mtshiba on 2025-09-25 06:00_

Parts not directly related to this PR have been moved to #20566, which aims to make recursive type inference converge more generally.

The handling of recursive type inference in this PR should be replaced with something more sophisticated, but I think it's best to land this PR first and complete it in #20566.

---

_@mtshiba reviewed on 2025-09-25 06:22_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/builder.rs`:211 on 2025-09-25 06:22_

The reason we had to lower the limit here is that the addition of new query function `infer_return_type` (which can form cycles) has caused delays in convergence for existing query functions (such as `infer_scope_types`).
Specifically, I have found that a panic can occur unless `MAX_UNION_LITERALS+ITERATIONS_BEFORE_FALLBACK<199`.

---

_Marked ready for review by @mtshiba on 2025-09-25 06:23_

---

_Review requested from @carljm by @mtshiba on 2025-09-25 06:25_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:630 on 2025-09-25 22:02_

Why do we need this method if it's just an alias for `fallback_type()` method?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:669 on 2025-09-25 22:04_

```suggestion
            // TODO: this means that every `infer_return_type` creates a self-cycle that must be iterated.
            // Instead, have Salsa provide both previous and current value so we can do this in the
            // recovery function.
            union = union.add(previous_cycle_value.recursive_type_normalized(db, &visitor));
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:1537 on 2025-09-25 22:14_

We could probably implement `Into<ExpressionNodeKey>` for `&Box<Expr>` to avoid the need for this?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:4774 on 2025-09-25 22:21_

Why do we need to store the expression key instead of storing the type directly?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:1957 on 2025-09-25 22:23_

Why would we fallback to `Type::none` here?

It seems like we should just `expect` here, we should always have inferred a type for every returnee expression.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1350 on 2025-09-25 22:29_

If the level >= 1 and `self` does not contain `visitor.div`, do we need to keep walking `self` recursively, or could we just short-circuit and return `self`? It seems we will not change anything in the visit.

It's not really clear to me that we even need all the many added `recursive_type_normalized` methods in this PR, and all the `level` and `visit_no_shift` stuff either. Wouldn't it be an equivalent implementation if we just match on unions here, visit all their elements, and for any other type, check `has_divergent_type` and return `div` if so?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9515 on 2025-09-25 22:30_

Does this need to be a separate Salsa query, or could we just infer the return type of `self.function(db)`?

We could add the union with `Unknown` here rather than inside `ScopeInference::infer_return_type`.

---

_@carljm reviewed on 2025-09-25 22:42_

This is looking good! Some comments and questions inline.

I will be a bit hesitant to land this before type-of-self lands, because type-of-self is critical, and the more new type information we introduce, the more blockers we encounter there. (This already happened with the dict literal inference PR.)

---

_@MichaReiser reviewed on 2025-10-22 10:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7151 on 2025-10-22 10:45_

The reason we don't have a trait is because we want to pass the query arguments to the cycle recovery function and those are variadic. 

I don't see any reason why we can't have that trait inside ty

---

_Comment by @mtshiba on 2025-12-06 09:40_

Now we have proper cycle handling, Liskov checks, and type-of-self. I think it's a good time to reboot this PR.

---

_Converted to draft by @mtshiba on 2025-12-15 10:46_

---

_Comment by @mtshiba on 2025-12-18 15:50_

It appears that abnormal memory consumption was occurring during the scipy inspection and ty was killed.

[The profiling result](https://profiler.firefox.com/public/x0rzn43ca8jg0wvy4es9c0abb7a5v21z8xdeyf8/flame-graph/?globalTrackOrder=0&hiddenLocalTracksByPid=30516-0w57wlnwx8&symbolServer=http%3A%2F%2F127.0.0.1%3A3000%2F1yskjwj2ya41kgmsrmr9yj2diwwq0nbxxb6kmzg&thread=n&v=12) seem to suggest that `IntersectionBuilder` is a hotspot.
In particular, cloning and dropping `IntersectionBuilder` take a long time and huge amounts of memory.

---

_Comment by @mtshiba on 2025-12-18 16:11_

Hmm, in fact, this is a result of fixed-point iterations not converging and the intersection type growing without bound?

---

_Comment by @mtshiba on 2025-12-18 16:47_

The handling of unions in `IntersectionBuilder::add_positive_impl` seems to exponentially increase the number of `intersections`, which seems inherently problematic?

---

_Comment by @mtshiba on 2025-12-18 17:35_

mypy_primer: https://github.com/astral-sh/ruff/pull/17371#issuecomment-2799694845
codspeed: https://github.com/astral-sh/ruff/pull/17371#issuecomment-2993362046
type conformance test: https://github.com/astral-sh/ruff/pull/17371#issuecomment-3145365663
ruff_ecosystem: https://github.com/astral-sh/ruff/pull/17371#issuecomment-3044655961

---

_Comment by @carljm on 2025-12-18 17:38_

> The handling of unions in `IntersectionBuilder::add_positive_impl` seems to exponentially increase the number of `intersections`, which seems inherently problematic?

Yes, it could be problematic, but it's required to keep set-theoretic types in disjunctive normal form, which is valuable, and in most practical cases, unions/intersections don't grow large enough for it to be a problem. We could set some size-maximums with fallback to a less precise type, would have to look at the specific case that causes a blowup. If it's due to fixpoint iteration not converging, then that seems like the key issue to fix.

---

_Comment by @mtshiba on 2025-12-18 17:46_

Fixed `IntersectionBuilder` to not have multiple identical `InnerIntersectionBuilder`s.
[The pydantic result](https://github.com/astral-sh/ruff/pull/17371#issuecomment-2993362046) is interesting. It might be worth breaking out as a separate PR.

---

_Comment by @mtshiba on 2025-12-18 18:14_

Opened #22055. Thanks to this change, scipy no longer seems to be killed.

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-19 16:02_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:08_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 35,575 | 0 | 73 |
| `possibly-missing-attribute` | 15,046 | 86 | 339 |
| `invalid-argument-type` | 3,481 | 82 | 239 |
| `non-subscriptable` | 2,222 | 0 | 0 |
| `invalid-assignment` | 1,638 | 11 | 33 |
| `not-iterable` | 1,341 | 0 | 8 |
| `unresolved-attribute` | 1,209 | 12 | 3 |
| `no-matching-overload` | 728 | 0 | 0 |
| `call-non-callable` | 703 | 2 | 0 |
| `invalid-return-type` | 363 | 2 | 26 |
| `too-many-positional-arguments` | 290 | 3 | 0 |
| `unknown-argument` | 146 | 8 | 0 |
| `unsupported-base` | 145 | 0 | 0 |
| `invalid-await` | 115 | 0 | 6 |
| `invalid-key` | 115 | 0 | 0 |
| `unused-ignore-comment` | 20 | 83 | 0 |
| `missing-argument` | 44 | 4 | 0 |
| `index-out-of-bounds` | 43 | 4 | 0 |
| `parameter-already-assigned` | 2 | 19 | 0 |
| `division-by-zero` | 8 | 0 | 0 |
| `invalid-context-manager` | 7 | 0 | 0 |
| `deprecated` | 3 | 2 | 0 |
| `invalid-type-form` | 2 | 0 | 0 |
| `possibly-unresolved-reference` | 0 | 2 | 0 |
| `invalid-exception-caught` | 1 | 0 | 0 |
| `invalid-method-override` | 1 | 0 | 0 |
| **Total** | **63,248** | **320** | **727** |




---

_Marked ready for review by @mtshiba on 2025-12-19 17:25_

---

_Review requested from @carljm by @mtshiba on 2025-12-19 17:26_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-23 04:03_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-23 04:03_

---

_Label `ecosystem-analyzer` removed by @mtshiba on 2025-12-23 05:56_

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-23 05:56_

---

_Comment by @MichaReiser on 2025-12-23 07:29_

Given the many new diagnostics, I think we should consider adding a new `analysis.<option>` to control whether return type inference is on. 

I also suggest running the `ty-benchmarks` on this o get a better sense for the performance impact (note that `ty-benchmarks` turns off return type inference for all other type checkers that support it for a fair comparison) 

---

_@mtshiba reviewed on 2025-12-23 09:10_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/call/bind.rs`:3160 on 2025-12-23 09:10_

This addresses [a pattern](https://github.com/astral-sh/ruff/pull/17371/files#diff-8c9ae4384d5808e43fce19d9835dd6e49048297bd4989c5379ebe0f476d34e4f) that was not considered in #22068, but I'm not sure if this is the best approach.

---
