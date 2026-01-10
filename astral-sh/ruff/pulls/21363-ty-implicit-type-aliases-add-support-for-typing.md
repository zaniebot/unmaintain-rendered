```yaml
number: 21363
title: "[ty] Implicit type aliases: Add support for `typing.Union`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-type-aliases-typing-union
created_at: 2025-11-10T13:33:55Z
updated_at: 2025-11-12T11:59:17Z
url: https://github.com/astral-sh/ruff/pull/21363
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Implicit type aliases: Add support for `typing.Union`

---

_Pull request opened by @sharkdp on 2025-11-10 13:33_

## Summary

Add support for `typing.Union` in implicit type aliases / in value position.

## Typing conformance tests

Two new tests are passing

## Ecosystem impact

* The 2k new `invalid-key` diagnostics on pydantic are caused by https://github.com/astral-sh/ty/issues/1479#issuecomment-3513854645.
* Everything else I've checked is either a known limitation (often related to type narrowing, because union types are often narrowed down to a subset of options), or a true positive.

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-11-10 13:33_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-10 13:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 13:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-12 11:47:18.914860495 +0000
+++ new-output.txt	2025-11-12 11:47:22.351880046 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19385)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19b85)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -6,7 +6,6 @@
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_explicit.py:45:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
-aliases_explicit.py:49:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
 aliases_explicit.py:52:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_explicit.py:53:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_explicit.py:54:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
@@ -18,7 +17,6 @@
 aliases_explicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
 aliases_implicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_implicit.py:64:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_implicit.py:65:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
@@ -1003,5 +1001,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1005 diagnostics
+Found 1003 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-10 13:40_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-key` | 2,160 | 0 | 0 |
| `unused-ignore-comment` | 7 | 443 | 0 |
| `invalid-argument-type` | 264 | 5 | 116 |
| `invalid-assignment` | 134 | 12 | 13 |
| `possibly-missing-attribute` | 98 | 8 | 4 |
| `no-matching-overload` | 78 | 0 | 0 |
| `non-subscriptable` | 24 | 18 | 20 |
| `invalid-return-type` | 32 | 5 | 7 |
| `unresolved-attribute` | 40 | 1 | 2 |
| `invalid-type-form` | 24 | 0 | 0 |
| `unsupported-operator` | 12 | 1 | 7 |
| `call-non-callable` | 18 | 1 | 0 |
| `possibly-missing-implicit-call` | 3 | 0 | 10 |
| `invalid-context-manager` | 0 | 0 | 10 |
| `not-iterable` | 7 | 0 | 2 |
| `invalid-parameter-default` | 5 | 0 | 0 |
| `too-many-positional-arguments` | 4 | 0 | 0 |
| `redundant-cast` | 2 | 0 | 0 |
| `deprecated` | 0 | 1 | 0 |
| `index-out-of-bounds` | 1 | 0 | 0 |
| `possibly-unresolved-reference` | 0 | 1 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **2,913** | **497** | **191** |

**[Full report with detailed diff](https://david-implicit-type-aliases-n4gp.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases-n4gp.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-11-10 13:45_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-typing-union?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21363 will **degrade performances by 91.64%**

<sub>Comparing <code>david/implicit-type-aliases-typing-union</code> (a4ebe5d) with <code>main</code> (f5cf672)</sub>



### Summary

`❌ 3 (👁 3)` regressions  
`✅ 49` untouched  
`⏩ 2` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-typing-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 931.6 ms | 1,025.5 ms | -9.16% |
| 👁 | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-typing-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 3.3 s | 39.1 s | -91.64% |
| 👁 | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-typing-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 13.4 s | 14.7 s | -8.69% |
[^skipped]: 2 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-typing-union?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-10 15:28_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-10 15:28_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-10 15:53_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-10 15:53_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 09:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_typing.py:40:39: error[non-subscriptable] Cannot subscript object of type `<class 'Iterable[tuple[typing.TypeVar, typing.TypeVar]]'>` with no `__class_getitem__` method
- Found 26 diagnostics
+ Found 25 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3430:13: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `Sequence[@Todo] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:13: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `Sequence[bytes | str | memoryview[Unknown]] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3898:38: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | bytes | memoryview[Unknown] | ... omitted 3 union elements`
+ aioredis/client.py:3899:13: error[no-matching-overload] No overload of bound method `match` matches arguments
+ aioredis/client.py:4250:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ aioredis/client.py:4252:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[Unknown] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[Unknown] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:933:14: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `int`, in comparing `Literal[b" "]` with `bytes | memoryview[Unknown] | int`
+ aioredis/connection.py:934:26: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `bytes | memoryview[Unknown] | int`
- Found 17 diagnostics
+ Found 24 diagnostics

kornia (https://github.com/kornia/kornia)
+ kornia/augmentation/container/augment.py:313:84: error[invalid-argument-type] Argument to bound method `_preproc_dict_data` is incorrect: Expected `dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (Boxes & Top[dict[Unknown, Unknown]]) | (Keypoints & Top[dict[Unknown, Unknown]]) | dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`
+ kornia/augmentation/container/augment.py:439:84: error[invalid-argument-type] Argument to bound method `_preproc_dict_data` is incorrect: Expected `dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (Boxes & Top[dict[Unknown, Unknown]]) | (Keypoints & Top[dict[Unknown, Unknown]]) | dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`
+ kornia/contrib/extract_patches.py:411:47: error[not-iterable] Object of type `(int & ~AlwaysTruthy & ~AlwaysFalsy) | tuple[int, int, int, int]` may not be iterable
+ kornia/contrib/extract_patches.py:416:81: error[not-iterable] Object of type `(int & ~AlwaysTruthy & ~AlwaysFalsy) | tuple[int, int, int, int]` may not be iterable
- Found 759 diagnostics
+ Found 763 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/metadata/pkg_resources.py:128:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IResourceProvider | None`, found `InMemoryMetadata`
+ src/pip/_internal/metadata/pkg_resources.py:149:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IResourceProvider | None`, found `InMemoryMetadata`
- src/pip/_internal/metadata/pkg_resources.py:179:25: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | EmptyProvider`
+ src/pip/_internal/metadata/pkg_resources.py:179:25: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | (IResourceProvider & ~AlwaysFalsy) | EmptyProvider`
+ src/pip/_vendor/pkg_resources/__init__.py:3090:20: warning[possibly-missing-attribute] Attribute `_get_metadata_path` may be missing on object of type `Unknown | (IResourceProvider & ~AlwaysFalsy) | EmptyProvider`
+ src/pip/_vendor/pkg_resources/__init__.py:3466:40: error[invalid-argument-type] Argument to bound method `contains` is incorrect: Expected `Version | str`, found `(str & ~Distribution) | (tuple[str, ...] & ~Distribution) | Unknown`
+ src/pip/_vendor/rich/console.py:1325:31: error[call-non-callable] Object of type `object` is not callable
+ src/pip/_vendor/rich/padding.py:69:13: error[invalid-assignment] Not enough values to unpack: Expected 2
+ src/pip/_vendor/rich/padding.py:69:13: error[invalid-assignment] Too many values to unpack: Expected 2
+ src/pip/_vendor/rich/padding.py:72:13: error[invalid-assignment] Not enough values to unpack: Expected 4
+ src/pip/_vendor/rich/padding.py:72:13: error[invalid-assignment] Not enough values to unpack: Expected 4
- src/pip/_vendor/rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: @Todo) -> Table`
+ src/pip/_vendor/rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: int | tuple[int] | tuple[int, int] | tuple[int, int, int, int]) -> Table`
- Found 575 diagnostics
+ Found 584 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/config.py:1928:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, ConfigScope]`, found `(ConfigScope & tuple[object, ...]) | tuple[int, ConfigScope]`
- Found 7914 diagnostics
+ Found 7915 diagnostics

yarl (https://github.com/aio-libs/yarl)
- tests/test_update_query.py:63:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:168:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:219:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:247:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:309:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:315:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:321:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_update_query.py:355:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_url.py:2009:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ yarl/_query.py:51:66: error[invalid-argument-type] Argument to function `query_var` is incorrect: Expected `str | SupportsInt`, found `object`
- tests/test_url_query.py:219:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_url_query.py:225:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_url_query.py:237:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 47 diagnostics
+ Found 36 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ src/aiortc/contrib/signaling.py:44:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | None | int]` is not assignable to `dict[str, int | str]`
- Found 190 diagnostics
+ Found 191 diagnostics

black (https://github.com/psf/black)
- src/black/__init__.py:1486:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/black/__init__.py:1378:23: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Node | Leaf`
+ src/black/linegen.py:222:41: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Node | Leaf`
+ src/black/linegen.py:1576:9: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Leaf | Node`
+ src/black/linegen.py:1577:9: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
+ src/black/linegen.py:1757:25: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Node | Leaf`
+ src/black/linegen.py:1795:13: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
+ src/black/linegen.py:1796:13: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
+ src/black/nodes.py:752:32: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Leaf | Node`
- src/blib2to3/pgen2/parse.py:367:13: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[@Todo] | None`
+ src/blib2to3/pgen2/parse.py:367:13: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`
+ src/blib2to3/pytree.py:149:13: error[unresolved-attribute] Unresolved attribute `parent` on type `object`.
- Found 55 diagnostics
+ Found 63 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
+ htmlgen/form.py:456:34: error[unresolved-attribute] Object of type `str | bytes | Generator` has no attribute `children`
- Found 21 diagnostics
+ Found 22 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/_check/convert/_reduce/_pep/redpep484612646.py:323:13: error[invalid-argument-type] Argument to function `get_hint_pep484_typevar_bounded_constraints_or_none` is incorrect: Expected `TypeVar`, found `Unknown | Iota`
+ beartype/_check/metadata/hint/hintsane.py:397:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
+ beartype/_data/typing/datatypingport.py:249:26: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
+ beartype/_data/typing/datatypingport.py:276:24: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
+ beartype/_util/hint/pep/proposal/pep484585/generic/pep484585genget.py:745:28: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- Found 500 diagnostics
+ Found 505 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/incremental_graph.py:161:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/execution/incremental_graph.py:162:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/execution/incremental_graph.py:168:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/execution/incremental_graph.py:170:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/graphql/language/parser.py:266:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Source`, found `Source | str | Unknown`
- src/graphql/type/definition.py:1670:17: warning[possibly-missing-attribute] Attribute `of_type` may be missing on object of type `@Todo | GraphQLNonNull[Unknown] | None`
+ src/graphql/type/definition.py:1670:17: warning[possibly-missing-attribute] Attribute `of_type` may be missing on object of type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 6 union elements`
+ src/graphql/utilities/find_breaking_changes.py:110:12: error[invalid-return-type] Return type does not match returned value: expected `list[BreakingChange]`, found `list[BreakingChange | DangerousChange | Unknown]`
+ src/graphql/utilities/find_breaking_changes.py:125:12: error[invalid-return-type] Return type does not match returned value: expected `list[DangerousChange]`, found `list[BreakingChange | DangerousChange | Unknown]`
+ src/graphql/utilities/lexicographic_sort_schema.py:53:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLList[Unknown] | GraphQLNonNull[Unknown] | GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/utilities/type_info.py:200:21: error[unresolved-attribute] Object of type `None` has no attribute `of_type`
+ src/graphql/validation/rules/stream_directive_on_list_field.py:42:38: warning[possibly-missing-attribute] Attribute `of_type` may be missing on object of type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 5 union elements`
+ tests/type/test_validation.py:74:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
- Found 391 diagnostics
+ Found 394 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/console.py:1325:31: error[call-non-callable] Object of type `object` is not callable
+ rich/padding.py:69:13: error[invalid-assignment] Not enough values to unpack: Expected 2
+ rich/padding.py:69:13: error[invalid-assignment] Too many values to unpack: Expected 2
+ rich/padding.py:72:13: error[invalid-assignment] Not enough values to unpack: Expected 4
+ rich/padding.py:72:13: error[invalid-assignment] Not enough values to unpack: Expected 4
- rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: @Todo) -> Table`
+ rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: int | tuple[int] | tuple[int, int] | tuple[int, int, int, int]) -> Table`
+ tests/test_console.py:367:29: error[invalid-argument-type] Argument to bound method `render` is incorrect: Expected `ConsoleRenderable | RichCast | str`, found `list[Unknown]`
+ tests/test_measure.py:19:51: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `ConsoleRenderable | RichCast | str`, found `None`
+ tests/test_padding.py:28:24: error[invalid-argument-type] Argument to function `unpack` is incorrect: Expected `int | tuple[int] | tuple[int, int] | tuple[int, int, int, int]`, found `tuple[Literal[1], Literal[2], Literal[3]]`
+ tests/test_table.py:102:23: error[invalid-argument-type] Argument to bound method `add_row` is incorrect: Expected `ConsoleRenderable | RichCast | str | None`, found `Foo`
- Found 333 diagnostics
+ Found 342 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/api.py:1559:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dedupe/api.py:1563:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dedupe/core.py:196:41: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `int | str`
- Found 57 diagnostics
+ Found 56 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/AdvisoryVulnerability.py:170:30: warning[possibly-missing-attribute] Attribute `package` may be missing on object of type `(SimpleAdvisoryVulnerability & ~Top[dict[Unknown, Unknown]]) | (AdvisoryVulnerability & ~Top[dict[Unknown, Unknown]])`
+ github/AdvisoryVulnerability.py:171:25: warning[possibly-missing-attribute] Attribute `package` may be missing on object of type `(SimpleAdvisoryVulnerability & ~Top[dict[Unknown, Unknown]]) | (AdvisoryVulnerability & ~Top[dict[Unknown, Unknown]])`
+ github/AdvisoryVulnerability.py:173:33: warning[possibly-missing-attribute] Attribute `patched_versions` may be missing on object of type `(SimpleAdvisoryVulnerability & ~Top[dict[Unknown, Unknown]]) | (AdvisoryVulnerability & ~Top[dict[Unknown, Unknown]])`
+ github/AdvisoryVulnerability.py:174:37: warning[possibly-missing-attribute] Attribute `vulnerable_functions` may be missing on object of type `(SimpleAdvisoryVulnerability & ~Top[dict[Unknown, Unknown]]) | (AdvisoryVulnerability & ~Top[dict[Unknown, Unknown]])`
+ github/AdvisoryVulnerability.py:175:41: warning[possibly-missing-attribute] Attribute `vulnerable_version_range` may be missing on object of type `(SimpleAdvisoryVulnerability & ~Top[dict[Unknown, Unknown]]) | (AdvisoryVulnerability & ~Top[dict[Unknown, Unknown]])`
+ github/RepositoryAdvisory.py:153:13: error[invalid-argument-type] Argument to bound method `add_vulnerabilities` is incorrect: Expected `Iterable[SimpleAdvisoryVulnerability | AdvisoryVulnerability]`, found `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | None] | str | None | list[str]]]`
+ github/RepositoryAdvisory.py:199:28: error[invalid-argument-type] Argument to bound method `offer_credits` is incorrect: Expected `Iterable[SimpleCredit | AdvisoryCredit]`, found `list[Unknown | dict[Unknown | str, Unknown | str | NamedUser]]`
+ tests/RepositoryAdvisory.py:141:13: error[invalid-argument-type] Argument to bound method `offer_credits` is incorrect: Expected `Iterable[SimpleCredit | AdvisoryCredit]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
- Found 283 diagnostics
+ Found 291 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ mkosi/__init__.py:1626:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | str | None]`
+ mkosi/__init__.py:1719:17: error[invalid-argument-type] Argument to function `systemd_tool_version` is incorrect: Expected `Path | str`, found `Unknown | Path | None`
+ mkosi/__init__.py:1779:17: error[invalid-argument-type] Argument to function `systemd_tool_version` is incorrect: Expected `Path | str`, found `Unknown | Path | None`
+ mkosi/__init__.py:2505:17: error[invalid-argument-type] Argument to bound method `sandbox` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | str | None | Path]`
+ mkosi/__init__.py:4320:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
+ mkosi/qemu.py:343:5: error[invalid-assignment] Object of type `list[Unknown | Path | None | str]` is not assignable to `list[Path | str]`
+ mkosi/sysupdate.py:70:9: error[invalid-assignment] Object of type `list[Unknown | Path | None | str]` is not assignable to `list[Path | str]`
- Found 88 diagnostics
+ Found 95 diagnostics

nox (https://github.com/wntrblm/nox)
+ nox/_parametrize.py:152:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Param | Iterable[Any | Param | Iterable[Any]]]`, found `(Iterable[Param | Iterable[Any]] & tuple[object, ...]) | (Param & tuple[object, ...]) | (Iterable[Any] & tuple[object, ...]) | ... omitted 3 union elements`
- Found 28 diagnostics
+ Found 29 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:894:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 330 diagnostics
+ Found 329 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/test_yaml.py:177:52: error[invalid-argument-type] Argument to function `validate` is incorrect: Expected `dict[Unknown, Unknown] | list[Unknown] | str`, found `Literal["pooh", 1]`
- Found 204 diagnostics
+ Found 205 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/_request_methods.py:138:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urllib3/_request_methods.py:269:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urllib3/connectionpool.py:768:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/urllib3/fields.py:228:17: error[invalid-assignment] Not enough values to unpack: Expected 3
+ src/urllib3/fields.py:230:17: error[invalid-assignment] Too many values to unpack: Expected 2
- src/urllib3/fields.py:281:13: error[invalid-assignment] Object of type `dict_items[object, object]` is not assignable to `Iterable[tuple[str, @Todo | None]]`
+ src/urllib3/fields.py:281:13: error[invalid-assignment] Object of type `dict_items[object, object]` is not assignable to `Iterable[tuple[str, str | bytes | None]]`
+ src/urllib3/filepost.py:40:9: error[invalid-assignment] Object of type `ItemsView[object, object]` is not assignable to `Iterable[RequestField | tuple[str, str | bytes | tuple[str, str | bytes] | tuple[str, str | bytes, str]]]`
+ src/urllib3/filepost.py:48:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `str`, found `str | bytes | tuple[str, str | bytes] | tuple[str, str | bytes, str]`
+ src/urllib3/filepost.py:48:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `((str, str | bytes, /) -> str) | None`, found `str | bytes | tuple[str, str | bytes] | tuple[str, str | bytes, str]`
- src/urllib3/response.py:319:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urllib3/response.py:594:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urllib3/response.py:632:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urllib3/util/timeout.py:243:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_collections.py:311:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_collections.py:422:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:175:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:1206:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:614:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:616:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_chunked_transfer.py:49:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_chunked_transfer.py:77:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:263:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:302:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_poolmanager.py:793:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ test/with_dummyserver/test_socketlevel.py:1807:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Timeout | int | float | _TYPE_DEFAULT | None`, found `Unknown | str`
+ test/with_dummyserver/test_socketlevel.py:2688:46: error[invalid-argument-type] Argument to bound method `request` is incorrect: Expected `bytes | IO[Any] | Iterable[bytes] | str | None`, found `Generator[bytes, None, None] | BytesIO | StringIO | bytearray | bytes`
- Found 355 diagnostics
+ Found 344 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/certs.py:360:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mitmproxy/certs.py:717:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ test/mitmproxy/proxy/tutils.py:69:16: error[invalid-argument-type] Argument to function `_eq` is incorrect: Expected `Command | Event`, found `Command | Event | Iterable[Command | Event]`
+ test/mitmproxy/proxy/tutils.py:69:19: error[invalid-argument-type] Argument to function `_eq` is incorrect: Expected `Command | Event`, found `Command | Event | Iterable[Command | Event]`
+ test/mitmproxy/proxy/tutils.py:74:5: error[invalid-assignment] Object of type `str` is not assignable to `Command | Event`
+ test/mitmproxy/proxy/tutils.py:75:9: error[no-matching-overload] No overload of function `sub` matches arguments
+ test/mitmproxy/proxy/tutils.py:76:5: error[invalid-assignment] Object of type `str` is not assignable to `Command | Event`
+ test/mitmproxy/proxy/tutils.py:77:5: error[invalid-assignment] Object of type `str` is not assignable to `Command | Event`
+ test/mitmproxy/proxy/tutils.py:77:25: error[invalid-argument-type] Argument to function `indent` is incorrect: Expected `str`, found `Command | Event`
- test/mitmproxy/proxy/tutils.py:83:6: error[invalid-return-type] Return type does not match returned value: expected `list[@Todo]`, found `types.GeneratorType`
+ test/mitmproxy/proxy/tutils.py:83:6: error[invalid-return-type] Return type does not match returned value: expected `list[Command | Event]`, found `types.GeneratorType`
+ test/mitmproxy/proxy/tutils.py:187:13: error[unresolved-attribute] Object of type `Command | Event` has no attribute `data`
+ test/mitmproxy/proxy/tutils.py:204:48: error[call-non-callable] Object of type `object` is not callable
+ test/mitmproxy/proxy/tutils.py:204:48: error[call-non-callable] Object of type `object` is not callable
+ test/mitmproxy/proxy/tutils.py:229:17: error[invalid-assignment] Object of type `list[Command | Event]` is not assignable to `list[Command]`
+ test/mitmproxy/proxy/tutils.py:262:41: error[unresolved-attribute] Object of type `Command | Event` has no attribute `name`
+ test/mitmproxy/proxy/tutils.py:343:17: error[invalid-assignment] Object of type `Command | Event` is not assignable to attribute `to` of type `Command | type[Command] | int`
- Found 1822 diagnostics
+ Found 1833 diagnostics

vision (https://github.com/pytorch/vision)
+ references/depth/stereo/transforms.py:54:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:54:43: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:55:89: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:56:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:57:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:58:84: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:59:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ references/depth/stereo/transforms.py:60:86: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- references/depth/stereo/transforms.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo, @Todo], tuple[@Todo, @Todo]]`, found `tuple[tuple[Unknown, Unknown], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
+ references/depth/stereo/transforms.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]]`, found `tuple[tuple[Unknown, Unknown], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
+ references/depth/stereo/transforms.py:477:25: warning[possibly-missing-attribute] Attribute `unsqueeze` may be missing on object of type `(Unknown & ~None) | ndarray[@Todo, Unknown]`
- references/depth/stereo/transforms.py:485:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo, @Todo], tuple[@Todo, @Todo]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
+ references/depth/stereo/transforms.py:485:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
- references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...]` is not assignable to `tuple[@Todo, @Todo]`
+ references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...] | tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]` is not assignable to `tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]`
+ references/depth/stereo/transforms.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]]`, found `tuple[tuple[Unknown, Unknown], tuple[None | Unknown, None | Unknown], tuple[None | Unknown, ...]]`
+ test/test_transforms_v2.py:1804:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | Sequence[int | float] | None | dict[type | str, int | float | Sequence[int | float] | None]`, found `Literal["fill"]`
+ test/test_transforms_v2.py:2364:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: E

... (truncated 3765 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-11 09:19_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-11 09:19_

---

_Marked ready for review by @sharkdp on 2025-11-11 09:28_

---

_Review requested from @carljm by @sharkdp on 2025-11-11 09:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-11 09:28_

---

_Review requested from @dcreager by @sharkdp on 2025-11-11 09:28_

---

_Comment by @AlexWaygood on 2025-11-11 09:30_

The pydantic benchmark is also panicking:

```
Expected between 1 and 1000 diagnostics but got 2716
```

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 09:37_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @AlexWaygood on 2025-11-11 13:20_

@woodruffw, I see a huge diff in the mypy_primer logs [here](https://github.com/astral-sh/ruff/actions/runs/19261360735/job/55066696930?pr=21363#step:6:1063) but the comment is reporting

> No ecosystem changes detected ✅

which looks to me like a bug in astral-sh-bot? Possibly in the truncation logic? Previously we used to have it post as much of the diff as it was able to in the comment, but have the last line be something like

```
[...full diff truncated]
```

to indicate that we weren't able to post the full thing there

---

_Comment by @woodruffw on 2025-11-11 13:22_

Hmmm yeah, could be a truncation bug or maybe a mismatch in how the artifact is pulled down -- I can look at it today. 

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-11 14:28_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-11 14:29_

---

_Comment by @woodruffw on 2025-11-11 18:48_

> Hmmm yeah, could be a truncation bug or maybe a mismatch in how the artifact is pulled down -- I can look at it today.

Yeah, this looks like exactly it -- I'm deploying a fix to the bot momentarily 🙂 

Edit: Deploying now.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:578 on 2025-11-11 19:49_

👍 I don't know why this raises an error at runtime, seems silly to me.

---

_@carljm approved on 2025-11-11 19:54_

Nice! Personally I think we can land this even with the big perf regression on pydantic, especially if that regression still leaves us on par with pyrefly. But of course if we can optimize, that's better!

---

_Review requested from @MichaReiser by @sharkdp on 2025-11-12 08:09_

---

_Review requested from @dhruvmanila by @sharkdp on 2025-11-12 08:09_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-12 08:10_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-12 08:10_

---

_Merged by @sharkdp on 2025-11-12 11:59_

---

_Closed by @sharkdp on 2025-11-12 11:59_

---

_Branch deleted on 2025-11-12 11:59_

---
