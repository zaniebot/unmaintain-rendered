```yaml
number: 21737
title: "[ty] Improve diagnostics for unsupported comparison operations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/operator-disambiguate
created_at: 2025-12-01T18:37:01Z
updated_at: 2025-12-02T20:12:16Z
url: https://github.com/astral-sh/ruff/pull/21737
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Improve diagnostics for unsupported comparison operations

---

_@AlexWaygood_

## Summary

On `main`:

<img width="3106" height="684" alt="image" src="https://github.com/user-attachments/assets/25e28810-282d-469b-a1b1-b823075c8936" />

Concise diagnostics on `main`:

```
foo.py:1:1: error[unsupported-operator] Operator `<` is not supported for types `str` and `int`, in comparing `tuple[Literal[1], Literal["foo"]]` with `tuple[Literal[1], Literal[2]]`
foo.py:2:1: error[unsupported-operator] Operator `<` is not supported for types `object` and `object`, in comparing `tuple[object]` with `tuple[object]`
```

With PR:

<img width="2158" height="1446" alt="image" src="https://github.com/user-attachments/assets/f30adbc2-a123-40b6-8f5f-f04ae9287a04" />

Concise diagnostics with PR:

```
foo.py:1:1: error[unsupported-operator] Operator `<` is not supported between objects of type `tuple[Literal[1], Literal["foo"]]` and `tuple[Literal[1], Literal[2]]`
foo.py:2:1: error[unsupported-operator] Operator `<` is not supported between two objects of type `tuple[object]`
```

## Test Plan

Mdtests and snapshots


---

_Review requested from @carljm by @AlexWaygood on 2025-12-01 18:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-01 18:37_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-01 18:37_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-01 18:37_

---

_Label `ty` added by @AlexWaygood on 2025-12-01 18:37_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-01 18:37_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 18:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-02 19:54:53.533433718 +0000
+++ new-output.txt	2025-12-02 19:54:57.115460740 +0000
@@ -293,7 +293,7 @@
 dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_match_args.py:42:1: error[unresolved-attribute] Class `DC4` has no attribute `__match_args__`
 dataclasses_match_args.py:49:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `Unknown | tuple[()]`
-dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
+dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported between objects of type `DC1` and `DC2`
 dataclasses_postinit.py:28:7: error[unresolved-attribute] Object of type `DC1` has no attribute `x`
 dataclasses_postinit.py:29:7: error[unresolved-attribute] Object of type `DC1` has no attribute `y`
 dataclasses_slots.py:66:1: error[unresolved-attribute] Class `DC6` has no attribute `__slots__`
@@ -301,7 +301,7 @@
 dataclasses_transform_class.py:63:1: error[invalid-assignment] Property `id` defined in `Customer1` is read-only
 dataclasses_transform_class.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_class.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
-dataclasses_transform_class.py:72:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
+dataclasses_transform_class.py:72:6: error[unsupported-operator] Operator `<` is not supported between two objects of type `Customer1`
 dataclasses_transform_class.py:82:8: error[missing-argument] No argument provided for required parameter `id`
 dataclasses_transform_class.py:82:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_class.py:122:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
@@ -343,7 +343,7 @@
 dataclasses_transform_field.py:75:1: error[missing-argument] No argument provided for required parameter `name`
 dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 dataclasses_transform_func.py:56:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
-dataclasses_transform_func.py:60:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
+dataclasses_transform_func.py:60:6: error[unsupported-operator] Operator `<` is not supported between two objects of type `Customer1`
 dataclasses_transform_func.py:64:36: error[unknown-argument] Argument `salary` does not match any known parameter
 dataclasses_transform_func.py:70:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_func.py:70:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
@@ -352,7 +352,7 @@
 dataclasses_transform_meta.py:63:1: error[invalid-assignment] Property `id` defined in `Customer1` is read-only
 dataclasses_transform_meta.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_meta.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
-dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
+dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported between two objects of type `Customer1`
 dataclasses_transform_meta.py:83:8: error[missing-argument] No argument provided for required parameter `id`
 dataclasses_transform_meta.py:83:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:103:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-01 18:42_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_functional.py:632:13: error[unsupported-operator] Operator `<` is not supported for types `C` and `C`
+ tests/test_functional.py:632:13: error[unsupported-operator] Operator `<` is not supported between two objects of type `C`
- tests/test_next_gen.py:79:13: error[unsupported-operator] Operator `<` is not supported for types `C` and `C`
+ tests/test_next_gen.py:79:13: error[unsupported-operator] Operator `<` is not supported between two objects of type `C`

more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:3466:31: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None | Literal[0]` with `int`
+ more_itertools/more.py:3466:31: error[unsupported-operator] Operator `<=` is not supported between objects of type `Unknown | None | Literal[0]` and `int`
- more_itertools/more.py:3466:43: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None | int`
+ more_itertools/more.py:3466:43: error[unsupported-operator] Operator `<=` is not supported between objects of type `int` and `Unknown | None | int`
- more_itertools/more.py:3471:27: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None | Literal[0]` with `int`
+ more_itertools/more.py:3471:27: error[unsupported-operator] Operator `<=` is not supported between objects of type `Unknown | None | Literal[0]` and `int`
- more_itertools/more.py:3471:39: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None | int`
+ more_itertools/more.py:3471:39: error[unsupported-operator] Operator `<=` is not supported between objects of type `int` and `Unknown | None | int`

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/connection.py:933:14: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `int`, in comparing `Literal[b" "]` with `bytes | memoryview[int] | int`
+ aioredis/connection.py:933:14: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b" "]` and `bytes | memoryview[int] | int`

python-chess (https://github.com/niklasf/python-chess)
- chess/engine.py:506:20: error[unsupported-operator] Operator `<` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/engine.py:512:20: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/engine.py:518:20: error[unsupported-operator] Operator `>` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/engine.py:524:20: error[unsupported-operator] Operator `>=` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:506:20: error[unsupported-operator] Operator `<` is not supported between two objects of type `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:512:20: error[unsupported-operator] Operator `<=` is not supported between two objects of type `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:518:20: error[unsupported-operator] Operator `>` is not supported between two objects of type `tuple[bool, bool, bool, int, int | None]`
+ chess/engine.py:524:20: error[unsupported-operator] Operator `>=` is not supported between two objects of type `tuple[bool, bool, bool, int, int | None]`

pip (https://github.com/pypa/pip)
- src/pip/_internal/network/auth.py:557:22: error[unsupported-operator] Operator `<` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[400]`
+ src/pip/_internal/network/auth.py:557:22: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | None` and `Literal[400]`
- src/pip/_internal/network/utils.py:45:8: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `Literal[400]` with `Unknown | None`
+ src/pip/_internal/network/utils.py:45:8: error[unsupported-operator] Operator `<=` is not supported between objects of type `Literal[400]` and `Unknown | None`
- src/pip/_internal/network/utils.py:45:15: error[unsupported-operator] Operator `<` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[500]`
+ src/pip/_internal/network/utils.py:45:15: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | None` and `Literal[500]`
- src/pip/_internal/network/utils.py:50:10: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `Literal[500]` with `Unknown | None`
+ src/pip/_internal/network/utils.py:50:10: error[unsupported-operator] Operator `<=` is not supported between objects of type `Literal[500]` and `Unknown | None`
- src/pip/_internal/network/utils.py:50:17: error[unsupported-operator] Operator `<` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[600]`
+ src/pip/_internal/network/utils.py:50:17: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | None` and `Literal[600]`
- src/pip/_vendor/cachecontrol/controller.py:149:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["Range"]` with `Unknown | None | CaseInsensitiveDict`
+ src/pip/_vendor/cachecontrol/controller.py:149:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["Range"]` and `Unknown | None | CaseInsensitiveDict`
- src/pip/_vendor/requests/models.py:567:34: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["content-type"]` with `Unknown | None | CaseInsensitiveDict`
+ src/pip/_vendor/requests/models.py:567:34: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["content-type"]` and `Unknown | None | CaseInsensitiveDict`
- src/pip/_vendor/requests/models.py:1015:12: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `Literal[400]` with `Unknown | None`
+ src/pip/_vendor/requests/models.py:1015:12: error[unsupported-operator] Operator `<=` is not supported between objects of type `Literal[400]` and `Unknown | None`
- src/pip/_vendor/requests/models.py:1015:19: error[unsupported-operator] Operator `<` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[500]`
+ src/pip/_vendor/requests/models.py:1015:19: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | None` and `Literal[500]`
- src/pip/_vendor/requests/models.py:1020:14: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `Literal[500]` with `Unknown | None`
+ src/pip/_vendor/requests/models.py:1020:14: error[unsupported-operator] Operator `<=` is not supported between objects of type `Literal[500]` and `Unknown | None`
- src/pip/_vendor/requests/models.py:1020:21: error[unsupported-operator] Operator `<` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[600]`
+ src/pip/_vendor/requests/models.py:1020:21: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | None` and `Literal[600]`
- src/pip/_vendor/rich/prompt.py:361:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `list[str] | None`
+ src/pip/_vendor/rich/prompt.py:361:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `str` and `list[str] | None`
- src/pip/_vendor/urllib3/util/retry.py:466:32: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `object`, in comparing `Unknown` with `~AlwaysFalsy`
+ src/pip/_vendor/urllib3/util/retry.py:466:32: error[unsupported-operator] Operator `not in` is not supported between objects of type `Unknown` and `~AlwaysFalsy`

spack (https://github.com/spack/spack)
- lib/spack/spack/ci/__init__.py:959:8: error[unsupported-operator] Operator `in` is not supported for types `Any` and `None`, in comparing `Any` with `None | Unknown`
+ lib/spack/spack/ci/__init__.py:959:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Any` and `None | Unknown`
- lib/spack/spack/ci/__init__.py:968:8: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["image"]` with `None | Unknown`
+ lib/spack/spack/ci/__init__.py:968:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["image"]` and `None | Unknown`
- lib/spack/spack/ci/__init__.py:1050:8: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["tags"]` with `None | Unknown`
+ lib/spack/spack/ci/__init__.py:1050:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["tags"]` and `None | Unknown`
- lib/spack/spack/graph.py:131:31: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | list[Unknown | list[Unknown]]`
+ lib/spack/spack/graph.py:131:31: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | list[Unknown | list[Unknown]]`
- lib/spack/spack/test/llnl/util/lang.py:208:12: error[unsupported-operator] Operator `<` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:208:12: error[unsupported-operator] Operator `<` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:209:12: error[unsupported-operator] Operator `>` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:209:12: error[unsupported-operator] Operator `>` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:211:12: error[unsupported-operator] Operator `<=` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:211:12: error[unsupported-operator] Operator `<=` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:212:12: error[unsupported-operator] Operator `>=` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:212:12: error[unsupported-operator] Operator `>=` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:214:12: error[unsupported-operator] Operator `<=` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:214:12: error[unsupported-operator] Operator `<=` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:215:12: error[unsupported-operator] Operator `<=` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:215:12: error[unsupported-operator] Operator `<=` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:216:12: error[unsupported-operator] Operator `>=` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:216:12: error[unsupported-operator] Operator `>=` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/test/llnl/util/lang.py:217:12: error[unsupported-operator] Operator `>=` is not supported for types `KeyComparable` and `KeyComparable`
+ lib/spack/spack/test/llnl/util/lang.py:217:12: error[unsupported-operator] Operator `>=` is not supported between two objects of type `KeyComparable`
- lib/spack/spack/vendor/typing_extensions.py:113:42: error[unsupported-operator] Operator `>` is not supported for types `int` and `object`, in comparing `int` with `~AlwaysFalsy | int`
+ lib/spack/spack/vendor/typing_extensions.py:113:42: error[unsupported-operator] Operator `>` is not supported between objects of type `int` and `~AlwaysFalsy | int`

beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics

stone (https://github.com/dropbox/stone)
- stone/frontend/lexer.py:69:34: error[unsupported-operator] Operator `>` is not supported for types `None` and `int`, in comparing `Unknown | None | Literal[0]` with `Literal[0]`
+ stone/frontend/lexer.py:69:34: error[unsupported-operator] Operator `>` is not supported between objects of type `Unknown | None | Literal[0]` and `Literal[0]`
- stone/ir/data_types.py:1095:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | dict[Unknown, Unknown]`
+ stone/ir/data_types.py:1095:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | dict[Unknown, Unknown]`
- stone/ir/data_types.py:1263:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | OrderedDict[Unknown, Unknown]`
+ stone/ir/data_types.py:1263:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | OrderedDict[Unknown, Unknown]`
- stone/ir/data_types.py:1291:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | OrderedDict[Unknown, Unknown]`
+ stone/ir/data_types.py:1291:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | OrderedDict[Unknown, Unknown]`
- stone/ir/data_types.py:1344:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | OrderedDict[Unknown, Unknown]`
+ stone/ir/data_types.py:1344:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | OrderedDict[Unknown, Unknown]`
- stone/ir/data_types.py:1487:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | OrderedDict[Unknown, Unknown]`
+ stone/ir/data_types.py:1487:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | OrderedDict[Unknown, Unknown]`
- stone/ir/data_types.py:1527:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | OrderedDict[Unknown, Unknown]`
+ stone/ir/data_types.py:1527:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown` and `Unknown | None | OrderedDict[Unknown, Unknown]`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/utils.py:501:12: error[unsupported-operator] Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `(int & ~(() -> object)) | (((str | None, /) -> int | None) & ~(() -> object)) | (@Todo & ~None)` with `Literal[0]`
+ src/werkzeug/utils.py:501:12: error[unsupported-operator] Operator `>` is not supported between objects of type `(int & ~(() -> object)) | (((str | None, /) -> int | None) & ~(() -> object)) | (@Todo & ~None)` and `Literal[0]`
- tests/middleware/test_http_proxy.py:45:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["HTTP_X_SPECIAL"]` with `Any | None`
+ tests/middleware/test_http_proxy.py:45:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["HTTP_X_SPECIAL"]` and `Any | None`
- tests/test_datastructures.py:863:16: error[unsupported-operator] Operator `not in` is not supported for types `int` and `EnvironHeaders`, in comparing `Literal[42]` with `(Unknown & ~AlwaysFalsy) | (EnvironHeaders & ~AlwaysFalsy)`
+ tests/test_datastructures.py:863:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal[42]` and `(Unknown & ~AlwaysFalsy) | (EnvironHeaders & ~AlwaysFalsy)`

jinja (https://github.com/pallets/jinja)
- tests/test_loader.py:94:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["one"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["one"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:95:16: error[unsupported-operator] Operator `not in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["two"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["two"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:96:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["three"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["three"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:94:16: error[unsupported-operator] Operator `in` is not supported between objects of type `tuple[ReferenceType[DictLoader], Literal["one"]]` and `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:95:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `tuple[ReferenceType[DictLoader], Literal["two"]]` and `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:96:16: error[unsupported-operator] Operator `in` is not supported between objects of type `tuple[ReferenceType[DictLoader], Literal["three"]]` and `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rate.py:498:15: error[unsupported-operator] Operator `<` is not supported for types `None` and `int`, in comparing `int | None` with `Unknown | int`
+ src/aiortc/rate.py:498:15: error[unsupported-operator] Operator `<` is not supported between objects of type `int | None` and `Unknown | int`
- src/aiortc/rtcrtpreceiver.py:222:14: error[unsupported-operator] Operator `<` is not supported for types `int` and `None`, in comparing `int` with `int | None`
+ src/aiortc/rtcrtpreceiver.py:222:14: error[unsupported-operator] Operator `<` is not supported between objects of type `int` and `int | None`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_loader.py:64:8: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["key"]` with `Unknown | None`
+ tests/test_loader.py:64:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["key"]` and `Unknown | None`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/autoscale.py:245:12: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `Literal[200]` with `Unknown | None`
+ paasta_tools/cli/cmds/autoscale.py:245:12: error[unsupported-operator] Operator `<=` is not supported between objects of type `Literal[200]` and `Unknown | None`
- paasta_tools/cli/cmds/autoscale.py:245:19: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[299]`
+ paasta_tools/cli/cmds/autoscale.py:245:19: error[unsupported-operator] Operator `<=` is not supported between objects of type `Unknown | None` and `Literal[299]`
- paasta_tools/paastaapi/model_utils.py:275:29: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown & ~None` with `None | Unknown`
+ paasta_tools/paastaapi/model_utils.py:275:29: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown & ~None` and `None | Unknown`
- paasta_tools/setup_prometheus_adapter_config.py:816:8: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["seriesQuery"]` with `dict[Unknown, Unknown] | None`
+ paasta_tools/setup_prometheus_adapter_config.py:816:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["seriesQuery"]` and `dict[Unknown, Unknown] | None`

pytest (https://github.com/pytest-dev/pytest)
- testing/test_assertrewrite.py:140:28: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[Literal[3], Literal[0]]` with `Unknown | tuple[int | None, int | None]`
- testing/test_assertrewrite.py:140:38: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | tuple[int | None, int | None]` with `tuple[Literal[6], Literal[3]]`
+ testing/test_assertrewrite.py:140:28: error[unsupported-operator] Operator `<=` is not supported between objects of type `tuple[Literal[3], Literal[0]]` and `Unknown | tuple[int | None, int | None]`
+ testing/test_assertrewrite.py:140:38: error[unsupported-operator] Operator `<=` is not supported between objects of type `Unknown | tuple[int | None, int | None]` and `tuple[Literal[6], Literal[3]]`

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/credentials.py:161:16: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `dict[str, object] | None`
+ kopf/_cogs/structs/credentials.py:161:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `str` and `dict[str, object] | None`
- kopf/_cogs/structs/credentials.py:163:24: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `dict[str, object] | None`
+ kopf/_cogs/structs/credentials.py:163:24: error[unsupported-operator] Operator `not in` is not supported between objects of type `str` and `dict[str, object] | None`
- kopf/_cogs/structs/references.py:304:45: error[unsupported-operator] Operator `in` is not supported for types `str` and `Marker`, in comparing `Literal["."]` with `str | Marker | None`
+ kopf/_cogs/structs/references.py:304:45: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["."]` and `str | Marker | None`
- kopf/_cogs/structs/references.py:309:42: error[unsupported-operator] Operator `in` is not supported for types `str` and `Marker`, in comparing `Literal["/"]` with `str | Marker | None`
+ kopf/_cogs/structs/references.py:309:42: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["/"]` and `str | Marker | None`

rich (https://github.com/Textualize/rich)
- rich/prompt.py:361:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `str` with `list[str] | None`
+ rich/prompt.py:361:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `str` and `list[str] | None`

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/unicode.py:171:12: error[unsupported-operator] Operator `not in` is not supported for types `_StrLike@_map_positions_to_result` and `_StrLike@_map_positions_to_result`
+ pylint/checkers/unicode.py:171:12: error[unsupported-operator] Operator `not in` is not supported between two objects of type `_StrLike@_map_positions_to_result`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/object_store.py:2813:12: error[unsupported-operator] Operator `in` is not supported for types `ObjectID` and `() -> dict[ObjectID, ObjectID]`, in comparing `ObjectID` with `Unknown | ((() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ dulwich/object_store.py:2813:12: error[unsupported-operator] Operator `in` is not supported between objects of type `ObjectID` and `Unknown | ((() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy) | dict[Unknown, Unknown]`
- dulwich/objectspec.py:330:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `RefsContainer`, in comparing `Unknown | bytes` with `Repo | RefsContainer`
+ dulwich/objectspec.py:330:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Unknown | bytes` and `Repo | RefsContainer`

ignite (https://github.com/pytorch/ignite)
- ignite/engine/events.py:94:57: error[unsupported-operator] Operator `>` is not supported for types `list[Unknown]` and `int`, in comparing `(int & Integral) | (list[Unknown] & Integral)` with `Literal[0]`
+ ignite/engine/events.py:94:57: error[unsupported-operator] Operator `>` is not supported between objects of type `(int & Integral) | (list[Unknown] & Integral)` and `Literal[0]`
- ignite/handlers/lr_finder.py:186:16: error[unsupported-operator] Operator `<` is not supported for types `int` and `None`, in comparing `Unknown | int | float` with `Unknown | None | int | float`
+ ignite/handlers/lr_finder.py:186:16: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | int | float` and `Unknown | None | int | float`
- tests/ignite/distributed/test_launcher.py:281:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["nproc_per_node"]` with `Unknown | None | dict[Unknown, Unknown]`
+ tests/ignite/distributed/test_launcher.py:281:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["nproc_per_node"]` and `Unknown | None | dict[Unknown, Unknown]`
- tests/ignite/distributed/test_launcher.py:282:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["start_method"]` with `Unknown | None | dict[Unknown, Unknown]`
+ tests/ignite/distributed/test_launcher.py:282:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["start_method"]` and `Unknown | None | dict[Unknown, Unknown]`
- tests/ignite/distributed/utils/__init__.py:131:8: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:131:8: error[unsupported-operator] Operator `in` is not supported between objects of type `str | None` and `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:143:8: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:143:8: error[unsupported-operator] Operator `in` is not supported between objects of type `str | None` and `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:154:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:154:10: error[unsupported-operator] Operator `in` is not supported between objects of type `None | str` and `Literal["xla-tpu"]`
- tests/ignite/distributed/utils/__init__.py:157:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:157:10: error[unsupported-operator] Operator `in` is not supported between objects of type `None | str` and `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:255:8: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:255:8: error[unsupported-operator] Operator `in` is not supported between objects of type `str | None` and `Literal["xla-tpu"]`
- tests/ignite/distributed/utils/__init__.py:258:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:258:10: error[unsupported-operator] Operator `in` is not supported between objects of type `None | str` and `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:286:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:286:10: error[unsupported-operator] Operator `in` is not supported between objects of type `None | str` and `Literal["xla-tpu"]`
- tests/ignite/distributed/utils/__init__.py:417:40: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:417:40: error[unsupported-operator] Operator `in` is not supported between objects of type `str | None` and `Literal["xla-tpu"]`
- tests/ignite/distributed/utils/__init__.py:419:40: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:419:40: error[unsupported-operator] Operator `in` is not supported between objects of type `str | None` and `Literal["horovod"]`
- tests/ignite/handlers/test_time_profilers.py:289:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_start"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:289:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_start"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:331:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_complete"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:331:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_complete"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:377:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_epoch_start"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:377:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_epoch_start"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:427:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_epoch_complete"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:427:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_epoch_complete"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:477:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_iter_start"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:477:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_iter_start"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:527:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_iter_complete"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:527:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_iter_complete"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:577:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_get_batch_started"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:577:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_get_batch_started"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:627:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["delay_get_batch_completed"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:627:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["delay_get_batch_completed"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:653:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["do_something_once_on_2_epoch"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:653:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["do_something_once_on_2_epoch"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:674:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["do_something_once_on_2_epoch"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:674:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["do_something_once_on_2_epoch"]` and `str | int | float | tuple[str | int | float, str | int | float]`
- tests/ignite/handlers/test_time_profilers.py:709:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `int`, in comparing `Literal["on_custom_event"]` with `str | int | float | tuple[str | int | float, str | int | float]`
+ tests/ignite/handlers/test_time_profilers.py:709:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["on_custom_event"]` and `str | int | float | tuple[str | int | float, str | int | float]`

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:1740:13: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `Literal["258"]`
+ mkosi/__init__.py:1740:13: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["258"]`
- mkosi/__init__.py:1800:13: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `Literal["256"]`
+ mkosi/__init__.py:1800:13: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["256"]`
- mkosi/__init__.py:1807:17: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion & ~AlwaysFalsy` with `Literal["256"]`
+ mkosi/__init__.py:1807:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion & ~AlwaysFalsy` and `Literal["256"]`
- mkosi/__init__.py:3386:17: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[256]`
+ mkosi/__init__.py:3386:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[256]`
- mkosi/__init__.py:3391:17: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[258]`
+ mkosi/__init__.py:3391:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[258]`
- mkosi/bootloader.py:739:83: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `Literal["257"]`
+ mkosi/bootloader.py:739:83: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["257"]`
- mkosi/config.py:1559:8: error[unsupported-operator] Operator `>` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `Literal["26~devel"]`
+ mkosi/config.py:1559:8: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `Literal["26~devel"]`
- mkosi/config.py:1565:21: error[unsupported-operator] Operator `>` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `str & ~AlwaysFalsy`
+ mkosi/config.py:1565:21: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `str & ~AlwaysFalsy`
- mkosi/distribution/centos.py:61:13: error[unsupported-operator] Operator `>` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[9]`
+ mkosi/distribution/centos.py:61:13: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `Literal[9]`
- mkosi/distribution/centos.py:70:12: error[unsupported-operator] Operator `<=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[8]`
+ mkosi/distribution/centos.py:70:12: error[unsupported-operator] Operator `<=` is not supported between objects of type `GenericVersion` and `Literal[8]`
- mkosi/distribution/centos.py:255:12: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[10]`
+ mkosi/distribution/centos.py:255:12: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[10]`
- mkosi/distribution/opensuse.py:60:60: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[16]`
+ mkosi/distribution/opensuse.py:60:60: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[16]`
- mkosi/distribution/opensuse.py:224:20: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[16]`
+ mkosi/distribution/opensuse.py:224:20: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[16]`
- mkosi/qemu.py:277:24: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `Literal["0.10.0"]`
+ mkosi/qemu.py:277:24: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["0.10.0"]`
- mkosi/qemu.py:962:42: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `int`, in comparing `GenericVersion` with `Literal[254]`
+ mkosi/qemu.py:962:42: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[254]`
- mkosi/qemu.py:1122:9: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `GenericVersion`
+ mkosi/qemu.py:1122:9: error[unsupported-operator] Operator `>=` is not supported between two objects of type `GenericVersion`
- mkosi/qemu.py:1231:12: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `GenericVersion`
+ mkosi/qemu.py:1231:12: error[unsupported-operator] Operator `>=` is not supported between two objects of type `GenericVersion`
- mkosi/tree.py:144:69: error[unsupported-operator] Operator `>=` is not supported for types `GenericVersion` and `str`, in comparing `GenericVersion` with `Literal["9.5"]`
+ mkosi/tree.py:144:69: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["9.5"]`

PyGithub (https://github.com/PyGithub/PyGithub)
- tests/GithubRetry.py:102:53: error[unsupported-operator] Operator `>` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[0]`
+ tests/GithubRetry.py:102:53: error[unsupported-operator] Operator `>` is not supported between objects of type `Unknown | None` and `Literal[0]`

urllib3 (https://github.com/urllib3/urllib3)
- test/test_exceptions.py:58:20: error[unsupported-operator] Operator `in` is not supported for types `object` and `str`
+ test/test_exceptions.py:58:20: error[unsupported-operator] Operator `in` is not supported between objects of type `object` and `str`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_clientplayback.py:175:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["live flow"]` with `str | None`
+ test/mitmproxy/addons/test_clientplayback.py:175:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["live flow"]` and `str | None`
- test/mitmproxy/addons/test_clientplayback.py:179:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["intercepted flow"]` with `str | None`
+ test/mitmproxy/addons/test_clientplayback.py:179:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["intercepted flow"]` and `str | None`
- test/mitmproxy/addons/test_clientplayback.py:183:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["missing request"]` with `str | None`
+ test/mitmproxy/addons/test_clientplayback.py:183:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["missing request"]` and `str | None`
- test/mitmproxy/addons/test_clientplayback.py:187:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["missing content"]` with `str | None`
+ test/mitmproxy/addons/test_clientplayback.py:187:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["missing content"]` and `str | None`
- test/mitmproxy/addons/test_clientplayback.py:191:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["Can only replay HTTP"]` with `str | None`
+ test/mitmproxy/addons/test_clientplayback.py:191:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["Can only replay HTTP"]` and `str | None`
- test/mitmproxy/addons/test_cut.py:58:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `bytes`, in comparing `Literal["CERTIFICATE"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:58:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["CERTIFICATE"]` and `str | bytes`
- test/mitmproxy/addons/test_cut.py:65:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"hello binary"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:65:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b"hello binary"]` and `str | bytes`
- test/mitmproxy/addons/test_cut.py:66:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"hello text"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:66:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b"hello text"]` and `str | bytes`
- test/mitmproxy/addons/test_cut.py:67:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"it's me"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:67:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b"it's me"]` and `str | bytes`
- test/mitmproxy/addons/test_cut.py:68:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"hello binary"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:68:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b"hello binary"]` and `str | bytes`
- test/mitmproxy/addons/test_cut.py:69:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"hello text"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:69:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b"hello text"]` and `str | bytes`
- test/mitmproxy/addons/test_cut.py:70:12: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"it's me"]` with `str | bytes`
+ test/mitmproxy/addons/test_cut.py:70:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal[b"it's me"]` and `str | bytes`
- test/mitmproxy/addons/test_proxyserver.py:209:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["Request destination unknown"]` with `str | None`
+ test/mitmproxy/addons/test_proxyserver.py:209:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["Request destination unknown"]` and `str | None`
- test/mitmproxy/test_http.py:1119:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["鏄庝集"]` with `str | None`
+ test/mitmproxy/test_http.py:1119:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["鏄庝集"]` and `str | None`
- test/mitmproxy/test_http.py:1128:16: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["鏄庝集"]` with `str | None`
+ test/mitmproxy/test_http.py:1128:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["鏄庝集"]` and `str | None`
- test/mitmproxy/test_http.py:1137:16: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["鏄庝集"]` with `str | None`
+ test/mitmproxy/test_http.py:1137:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["鏄庝集"]` and `str | None`
- test/mitmproxy/test_http.py:1146:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["鏄庝集"]` with `str | None`
+ test/mitmproxy/test_http.py:1146:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["鏄庝集"]` and `str | None`

vision (https://github.com/pytorch/vision)
- test/datasets_utils.py:370:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["download"]` with `Unknown | None`
+ test/datasets_utils.py:370:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["download"]` and `Unknown | None`
- test/datasets_utils.py:564:16: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown | str` with `Unknown | None`
+ test/datasets_utils.py:564:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Unknown | str` and `Unknown | None`
- test/test_image.py:221:8: error[unsupported-operator] Operator `>=` is not supported for types `str` and `tuple[Literal[8], Literal[3]]`, in comparing `Literal["12.0.0"]` with `tuple[Literal[8], Literal[3]]`
+ test/test_image.py:221:8: error[unsupported-operator] Operator `>=` is not supported between objects of type `Literal["12.0.0"]` and `tuple[Literal[8], Literal[3]]`
- test/test_utils.py:127:8: error[unsupported-operator] Operator `>=` is not supported for types `str` and `tuple[Literal[10], Literal[1]]`, in comparing `Literal["12.0.0"]` with 

... (truncated 1443 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-01 18:50_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 19:00_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 0 | 0 | 814 |
| `unsupported-base` | 0 | 1 | 0 |
| **Total** | **0** | **1** | **814** |

**[Full report with detailed diff](https://alex-operator-disambiguate.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-operator-disambiguate.ecosystem-663.pages.dev/timing))




---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:343 on 2025-12-02 08:56_

❤️ 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:53 on 2025-12-02 08:57_

I'd remove this message. This is true for almost all ty diagnostics and it doesn't provide any additional information

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:64 on 2025-12-02 08:58_

I would remove the `Right-hand` side and `Left-hand side`. This is viusally appearant and we also don't refer to the left-hand or right-hand side in the rest of the diagnostic message.
```suggestion
   |                         -    - has type `NonContainer1 & NonContainer2`
   |                         |
   |                         has type `Literal[2]`
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:00_

Do we need to add the `info` message here? Can't we add a secondary annotation to the main diagnostic? I'm asking because I don't find `Operand types` a very helpful info message

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:4117 on 2025-12-02 09:02_

How's this comparison different from the one on line 4078?

---

_@MichaReiser reviewed on 2025-12-02 09:02_

Nice, I mainly reviewed the diagnostics and I think we should make them more concise

---

_@AlexWaygood reviewed on 2025-12-02 09:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:06_

I tried it with the secondary-annotation approach initially but found the overlapping ranges very confusing to look at (the primary range covers the whole expression — correctly, in my view)

---

_@AlexWaygood reviewed on 2025-12-02 09:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:53 on 2025-12-02 09:08_

Fair 

---

_@AlexWaygood reviewed on 2025-12-02 09:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:4117 on 2025-12-02 09:08_

I'll add a comment 

---

_@MichaReiser reviewed on 2025-12-02 09:10_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:10_

Hmm. Repeating the same code frame adds a lot of redundancy (beyond the message itself, like each diagnostic repeating the code twice). 

But I can see how overlapping ranges can look weird. The only other solution I see is to put the operand types into the primary message. Unless @BurntSushi has an idea

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:13_

> The only other solution I see is to put the operand types into the primary message.

Right, but it can make for an extremely long primary message. Which was exactly what this PR was trying to fix :-)

---

_@AlexWaygood reviewed on 2025-12-02 09:13_

---

_@AlexWaygood reviewed on 2025-12-02 09:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:18_

If we had the ability to optionally specify a separate primary-highlight range that is different from the primary suppression range (which we've discussed before in various places, e.g. https://github.com/astral-sh/ruff/pull/18050#discussion_r2085275592, though I don't think we have an issue for it yet), then I would have the primary-highlight range cover only the operator in between the operands. But as it is, I'm pretty sure that would do chaotic things to people's suppression comments for this error code

---

_@MichaReiser reviewed on 2025-12-02 09:27_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:27_

> Right, but it can make for an extremely long primary message. Which was exactly what this PR was trying to fix :-)

Not the diagnostic message, move it into the annotation. It might still be long, though. 

The alternative is to have to `info` messages, but I worry that this buries important information.

> If we had the ability to optionally specify a separate primary-highlight range that is different from the primary suppression range (which we've discussed before in various places, 

The case here seems different enough to me where I'd say a separate suppression range isn't the solution. In most instances where we wanted that feature is to allow a wider suppression range but without necessarily changing the line number on which the diagnostic gets reported. Here, we don't want to change the suppression range OR the line number of the diagnostic, we just want more sane rendering of overlapping ranges 😆 



---

_@AlexWaygood reviewed on 2025-12-02 09:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:31_

> Not the diagnostic message, move it into the annotation. It might still be long, though.

Yeah, this actually makes the "long sentence" problem even worse because if it's the primary annotation message then now the whole sentence is indented significantly before it even starts 

---

_@MichaReiser reviewed on 2025-12-02 09:33_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:33_

I think I prefer the old rendering then, given that it also matches the concise rendering. It simply presents the same information with less repetition

---

_@AlexWaygood reviewed on 2025-12-02 09:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:36_

> The case here seems different enough to me where I'd say a separate suppression range isn't the solution. In most instances where we wanted that feature is to allow a wider suppression range but without necessarily changing the line number on which the diagnostic gets reported. Here, we don't want to change the suppression range OR the line number of the diagnostic, we just want more sane rendering of overlapping ranges 😆

If I changed the primary range of the diagnostic to be just the operator (so that it doesn't overlap with the ranges of the secondary annotations) then we _would_ want to change the suppression range. That feels exactly the same as the situation discussed in https://github.com/astral-sh/ruff/pull/18050#discussion_r2085275592, where we wanted to make the primary range of the diagnostic smaller for rendering purposes but we held off because we didn't have a way to separately specify a larger suppression range 

---

_@MichaReiser reviewed on 2025-12-02 09:42_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 09:42_

Oh I see. I'm not sure if I would do that. It can be very hard to spot the operator and, similar to the attribute expression, the error isn't the operator, it's the combination of left hand side, right hand side, and operator (you all sort of convinced me that the attribute expression ranges are correct, don't start arguing the other way ;))

---

_@BurntSushi reviewed on 2025-12-02 13:29_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 13:29_

I like the rendering in this PR over the old rendering, even with the duplication.

But how bad does the overlapping ranges in one code frame look? That would be my instinct here.

---

_@AlexWaygood reviewed on 2025-12-02 15:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 15:14_

I pushed some updates so that the diagnostic is now much more concise in the (common?) case where both operands have the same type, e.g.:

<img width="1808" height="450" alt="image" src="https://github.com/user-attachments/assets/829ec9f3-c142-42ab-b4d9-966508bacbe2" />

But if the two operands have different types, I've kept the separate code frame -- it looks much more readable to me. Here's what it looks like on this PR branch currently:

<img width="2162" height="718" alt="image" src="https://github.com/user-attachments/assets/b53c752a-08dc-4de2-8fc6-91939b3d4f6f" />

and here's what it looks like if I push it all into one codeframe:

<img width="2154" height="504" alt="image" src="https://github.com/user-attachments/assets/f50449aa-17e1-4b5e-8942-9ede502e8f75" />

This is the patch I applied to my PR branch to push it all into one codeframe:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index 3493f62d17..cc98f709bb 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -4075,44 +4075,35 @@ pub(super) fn report_unsupported_comparison<'db>(
         [error.left_ty, error.right_ty, left_ty, right_ty],
     );
 
-    let mut diagnostic = if left_ty == right_ty {
-        let mut diag = diagnostic_builder
-            .into_diagnostic(format_args!("Unsupported `{}` operation", error.op));
-        diag.set_primary_message(format_args!(
+    let mut diagnostic =
+        diagnostic_builder.into_diagnostic(format_args!("Unsupported `{}` operation", error.op));
+
+    if left_ty == right_ty {
+        diagnostic.set_primary_message(format_args!(
             "Both operands have type `{}`",
             left_ty.display_with(db, display_settings.clone())
         ));
-        diag.annotate(context.secondary(left));
-        diag.annotate(context.secondary(right));
-        diag.set_concise_message(format_args!(
+        diagnostic.annotate(context.secondary(left));
+        diagnostic.annotate(context.secondary(right));
+        diagnostic.set_concise_message(format_args!(
             "Operator `{}` is not supported between two objects of type `{}`",
             error.op,
             left_ty.display_with(db, display_settings.clone())
         ));
-        diag
     } else {
-        let mut diag = diagnostic_builder.into_diagnostic("Unsupported comparison operation");
-
-        diag.set_primary_message(format_args!("`{}` is not supported here", error.op));
-
-        let mut sub = SubDiagnostic::new(SubDiagnosticSeverity::Info, "Operand types");
         for (ty, expr) in [(left_ty, left), (right_ty, right)] {
-            sub.annotate(context.secondary(expr).message(format_args!(
+            diagnostic.annotate(context.secondary(expr).message(format_args!(
                 "Has type `{}`",
                 ty.display_with(db, display_settings.clone())
             )));
         }
-        diag.sub(sub);
-
-        diag.set_concise_message(format_args!(
+        diagnostic.set_concise_message(format_args!(
             "Operator `{}` is not supported between objects of type `{}` and `{}`",
             error.op,
             left_ty.display_with(db, display_settings.clone()),
             right_ty.display_with(db, display_settings.clone())
         ));
-
-        diag
-    };
+    }
 
     // For non-atomic types like unions and tuples, we now provide context
     // on the underlying elements that caused the error.
```

</details>

---

_@BurntSushi reviewed on 2025-12-02 15:24_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 15:24_

I'm fine to go with what you have here personally. I think it's better than the status quo.

I am not offended by the overlapping ranges in one code frame though.

---

_Comment by @codspeed-hq[bot] on 2025-12-02 15:29_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Foperator-disambiguate?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21737 will **not alter performance**

<sub>Comparing <code>alex/operator-disambiguate</code> (a68fafc) with <code>main</code> (515de2d)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Foperator-disambiguate?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@AlexWaygood reviewed on 2025-12-02 16:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 16:42_

Eh, I guess it's okay. It still feels a bit like a wall of text to me, but I do agree that concision is important on the CLI. I also find the tiny red `^^^` primary diagnostic in between the two yellow secondary diagnostics to look very strange, but hey.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 16:42_

(I'll switch to a single codeframe)

---

_@AlexWaygood reviewed on 2025-12-02 16:42_

---

_@MichaReiser reviewed on 2025-12-02 18:10_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:58 on 2025-12-02 18:10_

I'm still not convinced by this change but I also don't think it should be blocking. I still think it makes the diagnostics unnecessarily verbose, especially in cases where the types are short, which is not uncommon. We then expand double the vertical size of the diagnostic without any real added benefit.

---

_@MichaReiser approved on 2025-12-02 18:11_

---

_@MichaReiser reviewed on 2025-12-02 19:20_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:53 on 2025-12-02 19:20_

I think I actually got this wrong and think we should keep it. Sorry. Did it show the elements that were incompatible if the type were a union or intersection? If so, I think that was useful. I misread it as that it always repeats the left and right operand types

---

_@AlexWaygood reviewed on 2025-12-02 19:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/intersections.md_-_Comparison___Intersec…_-_Diagnostics_-_Unsupported_operator…_(27f95f68d1c826ec).snap`:53 on 2025-12-02 19:22_

no, you were right, it didn't show anything useful. It just said "comparison may raise an exception". I do still have an `info` subdiagnostic that tells you the incompatible elements if it's a union or tuple. (Intersections don't have the same issue, because they're much more forgiving than unions or tuples.)

---

_Comment by @AlexWaygood on 2025-12-02 19:46_

Some final screenshots now that I've addressed review comments. Much more concise than the initial version in this PR:

<img width="2604" height="1952" alt="image" src="https://github.com/user-attachments/assets/a9bed9ec-4bb2-419e-aaa3-062e358bd2e2" />

and the concise diagnostics for the same errors:

```
foo.py:1:1: error[unsupported-operator] Operator `<` is not supported between objects of type `Literal[1]` and `Literal["foo"]`
foo.py:2:1: error[unsupported-operator] Operator `<` is not supported between two objects of type `tuple[Literal[1], Literal[2], Literal[3], Literal[4], object]`
foo.py:3:1: error[unsupported-operator] Operator `<` is not supported between objects of type `tuple[Literal[1], Literal[2], Literal[3], Literal[4], Literal["foo"]]` and `tuple[Literal[1], Literal[2], Literal[3], Literal[4], Literal[5]]`
foo.py:7:5: error[unsupported-operator] Operator `<` is not supported between objects of type `int | str` and `int | list[str]`
```

---

_Merged by @AlexWaygood on 2025-12-02 19:58_

---

_Closed by @AlexWaygood on 2025-12-02 19:58_

---

_Branch deleted on 2025-12-02 19:58_

---
