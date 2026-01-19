```yaml
number: 22654
title: "[ty] Add support for subscripts on intersections"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/sub
created_at: 2026-01-17T17:50:23Z
updated_at: 2026-01-19T18:19:08Z
url: https://github.com/astral-sh/ruff/pull/22654
synced_at: 2026-01-19T18:27:46Z
```

# [ty] Add support for subscripts on intersections

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2072.


---

_@charliermarsh reviewed on 2026-01-17 17:51_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:433 on 2026-01-17 17:51_

I will look into this.

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 17:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-17 17:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
+ pyp.py:429:34: error[unresolved-attribute] Object of type `stmt` has no attribute `body`
+ pyp.py:432:27: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
+ pyp.py:433:26: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
+ pyp.py:439:75: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
- Found 5 diagnostics
+ Found 9 diagnostics

bidict (https://github.com/jab/bidict)
+ bidict/_iter.py:28:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Maplike[KT@iteritems, VT@iteritems].__getitem__(__key: KT@iteritems, /) -> VT@iteritems` cannot be called with key of type `object` on object of type `Maplike[KT@iteritems, VT@iteritems]`
+ bidict/_iter.py:28:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Maplike[Unknown, object]].__getitem__(__key: Never, /) -> object` cannot be called with key of type `object` on object of type `Top[Maplike[Unknown, object]]`
+ bidict/_iter.py:28:25: error[not-subscriptable] Cannot subscript object of type `Iterable[tuple[KT@iteritems, VT@iteritems]]` with no `__getitem__` method
- Found 16 diagnostics
+ Found 19 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | bytes | ... omitted 3 union elements`

parso (https://github.com/davidhalter/parso)
- parso/python/parser.py:120:50: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `@Todo | None`
+ parso/python/parser.py:120:50: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | None`
- parso/python/parser.py:121:26: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `@Todo | None`
+ parso/python/parser.py:121:26: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | None`
+ parso/python/tokenize.py:447:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[int, int]`, found `Unknown | None`
+ parso/python/tokenize.py:633:17: error[invalid-argument-type] Argument is incorrect: Expected `tuple[int, int]`, found `Unknown | None`
- Found 201 diagnostics
+ Found 203 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/mirrors/mirror.py:301:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | str`
+ lib/spack/spack/mirrors/mirror.py:301:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `Unknown | str`
+ lib/spack/spack/solver/asp.py:3269:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- lib/spack/spack/solver/requirements.py:251:49: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `@Todo | list[Unknown]`
+ lib/spack/spack/solver/requirements.py:251:49: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `Unknown | list[Unknown]`
+ lib/spack/spack/vendor/jinja2/compiler.py:1516:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
- Found 4344 diagnostics
+ Found 4346 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/msgpack/fallback.py:426:13: error[invalid-assignment] Not enough values to unpack: Expected 3
+ src/pip/_vendor/msgpack/fallback.py:437:13: error[invalid-assignment] Not enough values to unpack: Expected 3
+ src/pip/_vendor/msgpack/fallback.py:445:13: error[invalid-assignment] Too many values to unpack: Expected 2
+ src/pip/_vendor/msgpack/fallback.py:453:13: error[invalid-assignment] Not enough values to unpack: Expected 3
+ src/pip/_vendor/msgpack/fallback.py:460:13: error[invalid-assignment] Not enough values to unpack: Expected 3
+ src/pip/_vendor/msgpack/fallback.py:471:13: error[invalid-assignment] Not enough values to unpack: Expected 3
+ src/pip/_vendor/msgpack/fallback.py:478:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 594 diagnostics
+ Found 601 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/_check/code/codescope.py:448:13: error[invalid-argument-type] Argument to function `add_func_scope_type` is incorrect: Expected `type`, found `object`
+ beartype/_util/kind/sequence/utilseqmake.py:44:17: error[invalid-assignment] Object of type `Sequence[Unknown]` is not assignable to `list[Unknown]`
- beartype/_util/text/utiltextjoin.py:123:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_util/text/utiltextjoin.py:130:20: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 495 diagnostics
+ Found 497 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/sansio/response.py:608:50: error[unresolved-attribute] Object of type `object` has no attribute `to_header`
+ src/werkzeug/sansio/response.py:610:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> object, (s: slice[Never, Never, Never], /) -> Top[list[Unknown]]]` cannot be called with key of type `slice[Literal[1], None, None]` on object of type `Top[list[Unknown]]`
+ src/werkzeug/sansio/response.py:610:25: error[invalid-argument-type] Method `__getitem__` of type `bound method WWWAuthenticate.__getitem__(key: str) -> str | None` cannot be called with key of type `slice[Literal[1], None, None]` on object of type `WWWAuthenticate`
+ src/werkzeug/test.py:361:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str | None] | None`, found `dict[str, object]`
- Found 407 diagnostics
+ Found 411 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/compiler.py:1534:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
- Found 180 diagnostics
+ Found 181 diagnostics

black (https://github.com/psf/black)
+ src/black/ranges.py:404:28: error[invalid-argument-type] Argument to function `first_leaf` is incorrect: Expected `Leaf | Node`, found `object`
+ src/black/ranges.py:405:26: error[invalid-argument-type] Argument to function `last_leaf` is incorrect: Expected `Leaf | Node`, found `object`
+ src/blib2to3/pgen2/parse.py:392:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`
- Found 50 diagnostics
+ Found 53 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | @Todo`
+ paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | Unknown`
+ paasta_tools/paastaapi/api_client.py:720:21: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]`
+ paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
+ paasta_tools/tron_tools.py:192:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/tron_tools.py:192:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 1103 diagnostics
+ Found 1107 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/pyutils/test_ref_map.py:35:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:52:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:72:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 641 diagnostics
+ Found 638 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/runner.py:551:43: error[not-subscriptable] Cannot subscript object of type `Item` with no `__getitem__` method
- Found 413 diagnostics
+ Found 414 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/structs/dicts.py:65:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Mapping[Unknown, object]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[Mapping[Unknown, object]]`
+ kopf/_cogs/structs/dicts.py:65:30: error[not-subscriptable] Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
- kopf/_cogs/structs/patches.py:116:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kopf/_kits/hierarchies.py:178:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:178:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:178:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `object`
+ kopf/_kits/hierarchies.py:178:59: error[not-subscriptable] Cannot subscript object of type `_dummy` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:178:59: error[not-subscriptable] Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:182:28: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["generateName"]` and `object`
+ kopf/_kits/hierarchies.py:182:46: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:182:46: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:182:46: error[not-subscriptable] Cannot subscript object of type `_dummy` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:182:46: error[not-subscriptable] Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:183:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:183:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:183:33: error[not-subscriptable] Cannot subscript object of type `_dummy` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:183:33: error[not-subscriptable] Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:183:33: error[not-subscriptable] Cannot delete subscript on object of type `object` with no `__delitem__` method
+ kopf/_kits/hierarchies.py:186:28: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["name"]` and `object`
+ kopf/_kits/hierarchies.py:186:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:186:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:186:38: error[not-subscriptable] Cannot subscript object of type `_dummy` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:186:38: error[not-subscriptable] Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:187:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:187:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:187:33: error[not-subscriptable] Cannot subscript object of type `_dummy` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:187:33: error[not-subscriptable] Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
+ kopf/_kits/hierarchies.py:187:33: error[not-subscriptable] Cannot delete subscript on object of type `object` with no `__delitem__` method
- Found 268 diagnostics
+ Found 294 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/boost.py:366:35: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` with no `__getitem__` method
- Found 21 diagnostics
+ Found 22 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/ignore.py:150:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:153:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:173:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
+ dulwich/object_store.py:2923:29: error[not-subscriptable] Cannot subscript object of type `() -> dict[ObjectID, ObjectID]` with no `__getitem__` method
- Found 229 diagnostics
+ Found 227 diagnostics

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: warning[possibly-missing-attribute] Attribute `item` may be missing on object of type `Unknown | int | float | str`
+ tests/ignite/test_utils.py:49:28: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["a"]` on object of type `Top[dict[Unknown, Unknown]]`
+ tests/ignite/test_utils.py:49:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Unknown, (index: slice[Any, Any, Any]) -> Sequence[Unknown]]` cannot be called with key of type `Literal["a"]` on object of type `Sequence[Unknown]`
+ tests/ignite/test_utils.py:50:28: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["b"]` on object of type `Top[dict[Unknown, Unknown]]`
+ tests/ignite/test_utils.py:50:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Unknown, (index: slice[Any, Any, Any]) -> Sequence[Unknown]]` cannot be called with key of type `Literal["b"]` on object of type `Sequence[Unknown]`
- Found 2033 diagnostics
+ Found 2040 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/AdvisoryCredit.py:97:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:97:31: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:97:84: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:97:84: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:98:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["type"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:98:31: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:98:53: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["type"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:98:53: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:109:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:109:31: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:109:84: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:109:84: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:110:21: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:110:21: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryCredit.py:113:20: error[invalid-return-type] Return type does not match returned value: expected `SimpleCredit`, found `dict[Unknown | str, object]`
+ github/AdvisoryCredit.py:115:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["type"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:115:25: error[invalid-argument-type] Invalid argument to key "type" with declared type `str` on TypedDict `SimpleCredit`: value of type `object`
+ github/AdvisoryCredit.py:115:25: error[not-subscriptable] Cannot subscript object of type `AdvisoryCredit` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:129:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["package"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:129:59: error[invalid-assignment] Object of type `object` is not assignable to `SimpleAdvisoryVulnerabilityPackage`
+ github/AdvisoryVulnerability.py:129:59: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:136:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["patched_versions"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:136:31: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:138:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:138:31: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:141:51: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:141:51: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:142:20: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:142:20: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:146:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_version_range"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:146:31: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:158:73: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["package"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:158:73: error[invalid-assignment] Object of type `object` is not assignable to `SimpleAdvisoryVulnerabilityPackage`
+ github/AdvisoryVulnerability.py:158:73: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:159:20: error[invalid-return-type] Return type does not match returned value: expected `SimpleAdvisoryVulnerability`, found `dict[Unknown | str, object]`
+ github/AdvisoryVulnerability.py:164:37: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["patched_versions"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:164:37: error[invalid-argument-type] Invalid argument to key "patched_versions" with declared type `str | None` on TypedDict `SimpleAdvisoryVulnerability`: value of type `object`
+ github/AdvisoryVulnerability.py:164:37: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:165:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:165:41: error[invalid-argument-type] Invalid argument to key "vulnerable_functions" with declared type `list[str] | None` on TypedDict `SimpleAdvisoryVulnerability`: value of type `object`
+ github/AdvisoryVulnerability.py:165:41: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
+ github/AdvisoryVulnerability.py:166:45: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_version_range"]` on object of type `Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:166:45: error[invalid-argument-type] Invalid argument to key "vulnerable_version_range" with declared type `str | None` on TypedDict `SimpleAdvisoryVulnerability`: value of type `object`
+ github/AdvisoryVulnerability.py:166:45: error[not-subscriptable] Cannot subscript object of type `AdvisoryVulnerability` with no `__getitem__` method
- Found 299 diagnostics
+ Found 343 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` may be missing on object of type `@Todo | bool`
+ porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` may be missing on object of type `Any | bool`

ppb-vector (https://github.com/ppb/ppb-vector)
+ ppb_vector/__init__.py:189:26: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Mapping[Unknown, object]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["x"]` on object of type `Top[Mapping[Unknown, object]]`
+ ppb_vector/__init__.py:189:26: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> SupportsFloat, (index: slice[Any, Any, Any]) -> Sequence[SupportsFloat]]` cannot be called with key of type `Literal["x"]` on object of type `Sequence[SupportsFloat]`
+ ppb_vector/__init__.py:189:26: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `object`
+ ppb_vector/__init__.py:189:45: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Mapping[Unknown, object]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["y"]` on object of type `Top[Mapping[Unknown, object]]`
+ ppb_vector/__init__.py:189:45: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> SupportsFloat, (index: slice[Any, Any, Any]) -> Sequence[SupportsFloat]]` cannot be called with key of type `Literal["y"]` on object of type `Sequence[SupportsFloat]`
+ ppb_vector/__init__.py:189:45: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `object`
- Found 21 diagnostics
+ Found 27 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/config/_diff_base.py:44:39: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `object` on object of type `Top[dict[Unknown, Unknown]]`
- Found 282 diagnostics
+ Found 283 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/proxy/tunnel.py:195:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mitmproxy/proxy/tunnel.py:199:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2143 diagnostics
+ Found 2141 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/routing.py:355:56: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> object, (s: slice[Never, Never, Never], /) -> Top[list[Unknown]]]` cannot be called with key of type `slice[Literal[1], None, None]` on object of type `Top[list[Unknown]]`
+ tornado/routing.py:355:56: error[not-subscriptable] Cannot subscript object of type `Rule` with no `__getitem__` method
- Found 328 diagnostics
+ Found 330 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/typing/common.py:236:35: error[invalid-argument-type] Argument to function `signature` is incorrect: Expected `(...) -> Any`, found `@Todo | (tuple[Any, ...] & ~AlwaysFalsy & ~AlwaysTruthy) | None`
+ pandera/typing/common.py:236:35: error[invalid-argument-type] Argument to function `signature` is incorrect: Expected `(...) -> Any`, found `Any | (tuple[Any, ...] & ~AlwaysFalsy & ~AlwaysTruthy) | None`

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/avro/schema.py:801:34: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["names"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:801:34: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:801:34: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:803:44: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["names"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:803:44: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:803:44: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:805:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["extends"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:805:33: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:808:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["items"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:808:38: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:808:38: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:808:57: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["items"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:808:57: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:808:57: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:810:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["values"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:810:38: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:810:38: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:810:58: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["values"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:810:58: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:810:58: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:812:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["symbols"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:812:38: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:812:38: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:812:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["symbols"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:812:59: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:812:59: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:814:57: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["fields"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:814:57: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/avro/schema.py:816:66: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["fields"]` on object of type `Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:816:66: error[not-subscriptable] Cannot subscript object of type `Schema` with no `__getitem__` method
+ schema_salad/jsonld_context.py:54:24: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["symbol"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:54:24: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["symbol"]` on object of type `str`
+ schema_salad/jsonld_context.py:55:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["predicate"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:55:29: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["predicate"]` on object of type `str`
+ schema_salad/jsonld_context.py:55:29: error[invalid-assignment] Object of type `object` is not assignable to `dict[str, str | None] | str | None`
+ schema_salad/jsonld_context.py:211:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:211:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:211:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:211:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `str` on object of type `str`
+ schema_salad/jsonld_context.py:211:30: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ schema_salad/jsonld_context.py:211:30: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ schema_salad/jsonld_context.py:229:19: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["@id"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:229:19: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/metaschema.py:1029:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/metaschema.py:1029:41: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` cannot be called with key of type `str` on object of type `MutableSequence[Any]`
+ schema_salad/metaschema.py:1033:23: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["$base"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/metaschema.py:1033:23: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` cannot be called with key of type `Literal["$base"]` on object of type `MutableSequence[Any]`
+ schema_salad/metaschema.py:1033:23: error[invalid-assignment] Object of type `object` is not assignable to `str`
+ schema_salad/metaschema.py:1053:29: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` cannot be called with key of type `Literal["$graph"]` on object of type `MutableSequence[Any]`
+ schema_salad/metaschema.py:1053:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["$graph"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/python_codegen_support.py:1026:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/python_codegen_support.py:1026:41: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` cannot be called with key of type `str` on object of type `MutableSequence[Any]`
+ schema_salad/python_codegen_support.py:1030:23: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["$base"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/python_codegen_support.py:1030:23: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` cannot be called with key of type `Literal["$base"]` on object of type `MutableSequence[Any]`
+ schema_salad/python_codegen_support.py:1030:23: error[invalid-assignment] Object of type `object` is not assignable to `str`
+ schema_salad/python_codegen_support.py:1050:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["$graph"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/python_codegen_support.py:1050:29: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` cannot be called with key of type `Literal["$graph"]` on object of type `MutableSequence[Any]`
+ schema_salad/ref_resolver.py:375:25: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, int]`
+ schema_salad/ref_resolver.py:375:55: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["refScope"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:375:55: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/ref_resolver.py:383:25: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, int]`
+ schema_salad/ref_resolver.py:383:55: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["refScope"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:383:55: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/ref_resolver.py:394:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:394:39: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["mapSubject"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:394:39: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/ref_resolver.py:397:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:397:46: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["mapPredicate"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:397:46: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/ref_resolver.py:400:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:400:39: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["@id"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:400:39: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/ref_resolver.py:403:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:403:43: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["subscope"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:403:43: error[not-subscriptable] Cannot subscript object of type `Iterable[str]` with no `__getitem__` method
+ schema_salad/ref_resolver.py:496:32: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["$graph"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:496:32: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["$graph"]` on object of type `str`
+ schema_salad/ref_resolver.py:496:32: error[invalid-return-type] Return type does not match returned value: expected `tuple[int | float | str | ... omitted 3 union elements, CommentedMap]`, found `tuple[object, CommentedMap]`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `str` on object of type `str`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Argument to bound method `validate_link` is incorrect: Expected `str | CommentedSeq | CommentedMap`, found `object`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:1135:61: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ schema_salad/ref_resolver.py:1135:61: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ schema_salad/ref_resolver.py:1154:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:1154:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on ob

... (truncated 1056 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-17 18:20_

(Needs work.)

---

_Comment by @charliermarsh on 2026-01-17 19:52_

I can't tell if the pip errors are expected. Here's a smaller example:
```python
TYPE_IMMEDIATE = 0
TYPE_ARRAY = 1
TYPE_MAP = 2
TYPE_RAW = 3
TYPE_BIN = 4
TYPE_EXT = 5

_NO_FORMAT_USED = ""
_MSGPACK_HEADERS = {
    0xC4: (1, _NO_FORMAT_USED, TYPE_BIN),
    0xC5: (2, ">H", TYPE_BIN),
    0xC6: (4, ">I", TYPE_BIN),
    0xC7: (2, "Bb", TYPE_EXT),
    0xC8: (3, ">Hb", TYPE_EXT),
    0xC9: (5, ">Ib", TYPE_EXT),
    0xCA: (4, ">f"),
    0xCB: (8, ">d"),
    0xCC: (1, _NO_FORMAT_USED),
}

b: int

if 0xC4 <= b <= 0xC6:
    size, fmt, typ = _MSGPACK_HEADERS[b]
elif 0xCA <= b <= 0xCB:
    size, fmt, typ = _MSGPACK_HEADERS[b]
```

Mypy infers `_MSGPACK_HEADERS` as `builtins.dict[builtins.int, builtins.tuple[builtins.int | builtins.str, ...]]`, and Pyright as `dict[int, Unknown]`. ty infers as `dict[Unknown | int, Unknown | tuple[int, str, int] | tuple[int, str]]`.

---

_Comment by @charliermarsh on 2026-01-17 19:53_

I guess that actually matches our behavior on `main`, and this change is just surfacing it because we now properly infer the subscript type?

---

_Comment by @charliermarsh on 2026-01-17 19:55_

I _think_ the `ignite` errors are also true positives.

---

_Comment by @charliermarsh on 2026-01-17 20:02_

I think the `parso` diagnostics are also correct (we went from `@Todo | None` to `Unknown | None`).

---

_Marked ready for review by @charliermarsh on 2026-01-17 20:06_

---

_Review requested from @carljm by @charliermarsh on 2026-01-17 20:06_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-17 20:06_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-17 20:06_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-17 20:06_

---

_Renamed from "[ty] Remove TODO type for subscripting an intersection" to "[ty] Add support for subscripts on intersections" by @charliermarsh on 2026-01-17 20:33_

---

_Label `ty` added by @AlexWaygood on 2026-01-17 21:20_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-17 21:20_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 21:26_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 578 | 0 | 33 |
| `not-subscriptable` | 235 | 0 | 0 |
| `invalid-assignment` | 34 | 3 | 11 |
| `possibly-missing-attribute` | 21 | 0 | 12 |
| `unused-ignore-comment` | 0 | 25 | 0 |
| `invalid-return-type` | 7 | 2 | 9 |
| `unsupported-operator` | 4 | 3 | 9 |
| `unresolved-attribute` | 15 | 0 | 0 |
| `call-non-callable` | 9 | 0 | 0 |
| `no-matching-overload` | 5 | 0 | 0 |
| `not-iterable` | 5 | 0 | 0 |
| `type-assertion-failure` | 0 | 1 | 1 |
| `invalid-key` | 1 | 0 | 0 |
| **Total** | **914** | **34** | **75** |


**[Full report with detailed diff](https://214ce04a.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://214ce04a.ty-ecosystem-ext.pages.dev/timing))



---

_Converted to draft by @charliermarsh on 2026-01-17 22:28_

---

_Comment by @charliermarsh on 2026-01-17 22:28_

I’d like to do more investigation of the ecosystem changes (unless this is obviously correct).

---

_Comment by @charliermarsh on 2026-01-18 00:52_

I think the `paasta_tools/tron_tools.py` diagnostic is correct.

---

_Comment by @charliermarsh on 2026-01-18 00:53_

I think this spack diagnostic is correct, with: `Cannot subscript object of type ~AlwaysFalsy with no __getitem__ method`?

```python
# fetch from first entry in urls to save time
if hasattr(self, "urls") and self.urls:
    urls.append(self.urls[0])
```

---

_Marked ready for review by @charliermarsh on 2026-01-18 01:04_

---

_Comment by @charliermarsh on 2026-01-18 01:05_

It's a lot of new diagnostics... but my impression from checking around is that this is merely surfacing existing ty behavior in more contexts now. I don't see anything obviously wrong from my spot checks.

---

_Comment by @AlexWaygood on 2026-01-18 15:31_

This leads to one new false positive on docstring-adder (currently _full_ of intersection-subscript `@Todo` types), which is not emitted by mypy or pyright:

```
error[not-subscriptable]: Cannot subscript object of type `Else` with no `__getitem__` method
   --> add_docstrings.py:480:17
    |
478 |             and self.if_stack[-1][-1]
479 |             and not isinstance(self.if_stack[-1][-1], libcst.Else)
480 |             and self.if_stack[-1][-1][0].orelse is node
    |                 ^^^^^^^^^^^^^^^^^^^^^^^^
481 |         ):
482 |             self.if_stack[-1].append((node, parsed_condition))
    |
info: rule `not-subscriptable` is enabled by default

Found 1 diagnostic
```

But I agree that this seems like a pre-existing issue with our narrowing behaviour, not an issue with this PR

---

_Comment by @charliermarsh on 2026-01-18 15:44_

I will fix that in a separate PR.

---

_@AlexWaygood reviewed on 2026-01-18 15:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:14622 on 2026-01-18 15:46_

This works well for most cases, but not for slices on tuple types, because that's a special case that can't be modeled in `try_call_dunder` and is instead handled in `infer_subscript_expression_types`. Here's the types we reveal on your branch:

```py
from ty_extensions import Intersection

class A: ...
class B: ...
class C: ...
class D: ...

def f(
    x: tuple[A, B],
    y: tuple[C, D],
    z: Intersection[tuple[A, B], tuple[C, D]]
):
    reveal_type(x[1:])  # tuple[B]
    reveal_type(y[1:])  # tuple[D]
    reveal_type(z[1:])  # tuple[A | B, ...] & tuple[C | D, ...]
```

Ideally the final type there would be `tuple[B] & tuple[D]` (or `tuple[B & D]`).

Handling this properly would probably require refactoring the `infer_subscript_expression_types` method so that it's a method on `Type` that returns a `Result` (rather than eagerly emitting diagnostics when encountering errors) so that it can be recursively called for each intersection element without necessarily emitting an error on each invalid intersection element

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:14622 on 2026-01-18 15:49_

A similar repro on this branch:

```py
class A: ...
class B: ...
class C: ...

def f(
    x: tuple[A, B],
):
    reveal_type(x[1:])  # tuple[B]
    if isinstance(x, C):
        reveal_type(x[1:])  # tuple[A | B, ...]
```

if we're confident that this isn't causing any false positives in the ecosystem, we could possibly land this PR as-is for now. But I do think we need to do the big refactor of `infer_subscript_expression_types` at _some_ point

---

_@AlexWaygood reviewed on 2026-01-18 15:49_

---

_Converted to draft by @charliermarsh on 2026-01-18 15:59_

---

_@charliermarsh reviewed on 2026-01-18 16:00_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:14622 on 2026-01-18 16:00_

(Marking as draft while I look into it + consider.)

---

_@charliermarsh reviewed on 2026-01-19 00:14_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:14622 on 2026-01-19 00:14_

Gave this a try.

---

_Marked ready for review by @charliermarsh on 2026-01-19 00:52_

---

_Comment by @AlexWaygood on 2026-01-19 18:19_

looks like the number of diagnostics went up by around 500 when you did the refactor to make it a method on `Type` -- is that expected? (I haven't looked through them at all myself yet, just wondered if you had)

---
