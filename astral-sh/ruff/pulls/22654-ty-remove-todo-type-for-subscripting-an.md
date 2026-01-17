```yaml
number: 22654
title: "[ty] Remove TODO type for subscripting an intersection"
type: pull_request
state: open
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/sub
created_at: 2026-01-17T17:50:23Z
updated_at: 2026-01-17T20:06:54Z
url: https://github.com/astral-sh/ruff/pull/22654
synced_at: 2026-01-17T20:10:51Z
```

# [ty] Remove TODO type for subscripting an intersection

---

_@charliermarsh_

## Summary

AFAICT, the call infrastructure already supports this?

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
bidict (https://github.com/jab/bidict)
+ bidict/_iter.py:28:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Maplike[KT@iteritems, VT@iteritems].__getitem__(__key: KT@iteritems, /) -> VT@iteritems` cannot be called with key of type `object` on object of type `Maplike[KT@iteritems, VT@iteritems] & ~Top[Mapping[Unknown, object]]`
+ bidict/_iter.py:28:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Maplike[Unknown, object]].__getitem__(__key: Never, /) -> object` cannot be called with key of type `object` on object of type `Iterable[tuple[KT@iteritems, VT@iteritems]] & Top[Maplike[Unknown, object]] & ~Top[Mapping[Unknown, object]]`
- Found 16 diagnostics
+ Found 18 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
+ pyp.py:429:34: error[unresolved-attribute] Object of type `stmt` has no attribute `body`
+ pyp.py:432:27: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
+ pyp.py:433:26: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
+ pyp.py:439:75: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
- Found 5 diagnostics
+ Found 9 diagnostics

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
+ lib/spack/spack/package_base.py:2298:25: error[not-subscriptable] Cannot subscript object of type `~AlwaysFalsy` with no `__getitem__` method
+ lib/spack/spack/solver/asp.py:3269:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `ConcreteVersion & ~AlwaysFalsy` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- lib/spack/spack/solver/requirements.py:251:49: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `@Todo | list[Unknown]`
+ lib/spack/spack/solver/requirements.py:251:49: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `Unknown | list[Unknown]`
+ lib/spack/spack/vendor/jinja2/compiler.py:1516:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
- Found 4344 diagnostics
+ Found 4347 diagnostics

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
- beartype/_util/text/utiltextjoin.py:123:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_util/text/utiltextjoin.py:130:20: error[no-matching-overload] No overload of bound method `join` matches arguments
- beartype/bite/collection/infercollectionitems.py:321:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/bite/collection/infercollectionitems.py:504:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 495 diagnostics
+ Found 494 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/compiler.py:1534:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
- Found 180 diagnostics
+ Found 181 diagnostics

black (https://github.com/psf/black)
+ src/black/ranges.py:404:28: error[invalid-argument-type] Argument to function `first_leaf` is incorrect: Expected `Leaf | Node`, found `object`
+ src/black/ranges.py:405:26: error[invalid-argument-type] Argument to function `last_leaf` is incorrect: Expected `Leaf | Node`, found `object`
+ src/blib2to3/pgen2/parse.py:392:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`
- Found 53 diagnostics
+ Found 56 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/pyutils/test_ref_map.py:35:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:52:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:72:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 641 diagnostics
+ Found 638 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/boost.py:366:35: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]] & ~AlwaysFalsy` with no `__getitem__` method
- Found 21 diagnostics
+ Found 22 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | @Todo`
+ paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | Unknown`
+ paasta_tools/paastaapi/api_client.py:720:21: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]`
+ paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
+ paasta_tools/tron_tools.py:192:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 1103 diagnostics
+ Found 1106 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/runner.py:551:43: error[not-subscriptable] Cannot subscript object of type `Item & ~AlwaysTruthy & ~AlwaysFalsy` with no `__getitem__` method
- Found 413 diagnostics
+ Found 414 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/structs/dicts.py:65:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Mapping[Unknown, object]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `KubernetesModel & Top[Mapping[Unknown, object]]`
- kopf/_cogs/structs/patches.py:116:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kopf/_kits/hierarchies.py:178:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `_dummy & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:178:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `KubernetesModel & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:178:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `object`
+ kopf/_kits/hierarchies.py:182:28: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["generateName"]` and `object`
+ kopf/_kits/hierarchies.py:182:46: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `KubernetesModel & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:182:46: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `_dummy & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:183:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `_dummy & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:183:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `KubernetesModel & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:183:33: error[not-subscriptable] Cannot delete subscript on object of type `object` with no `__delitem__` method
+ kopf/_kits/hierarchies.py:186:28: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["name"]` and `object`
+ kopf/_kits/hierarchies.py:186:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `_dummy & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:186:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `KubernetesModel & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:187:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `_dummy & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:187:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["metadata"]` on object of type `KubernetesModel & Top[MutableMapping[Unknown, Unknown]]`
+ kopf/_kits/hierarchies.py:187:33: error[not-subscriptable] Cannot delete subscript on object of type `object` with no `__delitem__` method
- Found 268 diagnostics
+ Found 283 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` may be missing on object of type `@Todo | bool`
+ porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` may be missing on object of type `Any | bool`

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: warning[possibly-missing-attribute] Attribute `item` may be missing on object of type `Unknown | int | float | str`
- Found 2033 diagnostics
+ Found 2036 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/contrib/swift.py:842:56: error[not-subscriptable] Cannot subscript object of type `~None` with no `__getitem__` method
- dulwich/ignore.py:150:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:153:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:173:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
+ dulwich/object_store.py:2923:29: error[not-subscriptable] Cannot subscript object of type `(() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy` with no `__getitem__` method
- Found 230 diagnostics
+ Found 229 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/AdvisoryCredit.py:97:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:97:84: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:98:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["type"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:98:53: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["type"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:109:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:109:84: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:110:21: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["login"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:113:20: error[invalid-return-type] Return type does not match returned value: expected `SimpleCredit`, found `dict[Unknown | str, object]`
+ github/AdvisoryCredit.py:115:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["type"]` on object of type `AdvisoryCredit & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryCredit.py:115:25: error[invalid-argument-type] Invalid argument to key "type" with declared type `str` on TypedDict `SimpleCredit`: value of type `object`
+ github/AdvisoryVulnerability.py:129:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["package"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:129:59: error[invalid-assignment] Object of type `object` is not assignable to `SimpleAdvisoryVulnerabilityPackage`
+ github/AdvisoryVulnerability.py:136:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["patched_versions"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:138:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:141:51: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:142:20: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:146:31: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_version_range"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:158:73: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["package"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:158:73: error[invalid-assignment] Object of type `object` is not assignable to `SimpleAdvisoryVulnerabilityPackage`
+ github/AdvisoryVulnerability.py:159:20: error[invalid-return-type] Return type does not match returned value: expected `SimpleAdvisoryVulnerability`, found `dict[Unknown | str, object]`
+ github/AdvisoryVulnerability.py:164:37: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["patched_versions"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:164:37: error[invalid-argument-type] Invalid argument to key "patched_versions" with declared type `str | None` on TypedDict `SimpleAdvisoryVulnerability`: value of type `object`
+ github/AdvisoryVulnerability.py:165:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_functions"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:165:41: error[invalid-argument-type] Invalid argument to key "vulnerable_functions" with declared type `list[str] | None` on TypedDict `SimpleAdvisoryVulnerability`: value of type `object`
+ github/AdvisoryVulnerability.py:166:45: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["vulnerable_version_range"]` on object of type `AdvisoryVulnerability & Top[dict[Unknown, Unknown]]`
+ github/AdvisoryVulnerability.py:166:45: error[invalid-argument-type] Invalid argument to key "vulnerable_version_range" with declared type `str | None` on TypedDict `SimpleAdvisoryVulnerability`: value of type `object`
- Found 299 diagnostics
+ Found 325 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/config/_diff_base.py:44:39: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `object` on object of type `Top[dict[Unknown, Unknown]] & ~DataclassInstance`
- Found 282 diagnostics
+ Found 283 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
- tornado/routing.py:355:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | dict[str, Any] | str`
+ tornado/routing.py:355:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `object`
- tornado/routing.py:355:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `@Todo | dict[str, Any] | str`
+ tornado/routing.py:355:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
+ tornado/routing.py:355:56: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> object, (s: slice[Never, Never, Never], /) -> Top[list[Unknown]]]` cannot be called with key of type `slice[Literal[1], None, None]` on object of type `Rule & Top[list[Unknown]]`
- Found 328 diagnostics
+ Found 329 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/proxy/tunnel.py:195:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mitmproxy/proxy/tunnel.py:199:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2143 diagnostics
+ Found 2141 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/typing/common.py:236:35: error[invalid-argument-type] Argument to function `signature` is incorrect: Expected `(...) -> Any`, found `@Todo | (tuple[Any, ...] & ~AlwaysFalsy & ~AlwaysTruthy) | None`
+ pandera/typing/common.py:236:35: error[invalid-argument-type] Argument to function `signature` is incorrect: Expected `(...) -> Any`, found `Any | (tuple[Any, ...] & ~AlwaysFalsy & ~AlwaysTruthy) | None`

mypy (https://github.com/python/mypy)
- mypy/checker.py:7612:69: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `(list[PartialTypeScope] & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo & ~AlwaysFalsy) | bool`
+ mypy/checker.py:7612:69: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `(list[PartialTypeScope] & ~AlwaysTruthy & ~AlwaysFalsy) | bool`
+ mypyc/ir/ops.py:118:16: error[invalid-return-type] Return type does not match returned value: expected `ControlOp`, found `Op`
- Found 1740 diagnostics
+ Found 1741 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/avro/schema.py:801:34: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["names"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:801:34: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:803:44: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["names"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:803:44: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:805:33: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["extends"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:808:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["items"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:808:38: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:808:57: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["items"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:808:57: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:810:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["values"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:810:38: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:810:58: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["values"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:810:58: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:812:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["symbols"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:812:38: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:812:59: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["symbols"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:812:59: error[invalid-argument-type] Argument to function `is_subtype` is incorrect: Expected `None | str | int | ... omitted 6 union elements`, found `object`
+ schema_salad/avro/schema.py:814:57: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["fields"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/avro/schema.py:816:66: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["fields"]` on object of type `Schema & Top[dict[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:211:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:211:30: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/jsonld_context.py:229:19: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["@id"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:375:25: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, int]`
+ schema_salad/ref_resolver.py:375:55: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["refScope"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:383:25: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, int]`
+ schema_salad/ref_resolver.py:383:55: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["refScope"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:394:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:394:39: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["mapSubject"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:397:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:397:46: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["mapPredicate"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:400:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:400:39: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["@id"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:403:21: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:403:43: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["subscope"]` on object of type `Iterable[str] & Top[MutableMapping[Unknown, Unknown]]`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1135:61: error[invalid-argument-type] Argument to bound method `validate_link` is incorrect: Expected `str | CommentedSeq | CommentedMap`, found `object`
+ schema_salad/ref_resolver.py:1154:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1154:29: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1155:50: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, str].__getitem__(key: str, /) -> str` cannot be called with key of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:1155:62: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1155:62: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1159:33: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, str].__getitem__(key: str, /) -> str` cannot be called with key of type `object` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:1159:45: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1159:45: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1161:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1161:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1161:41: error[invalid-argument-type] Argument to function `relname` is incorrect: Expected `str`, found `object`
+ schema_salad/ref_resolver.py:1164:29: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `str` on object of type `dict[str, str]`
+ schema_salad/ref_resolver.py:1164:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/ref_resolver.py:1164:41: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]`
+ schema_salad/sourceline.py:211:26: error[invalid-argument-type] Argument to function `cmap` is incorrect: Expected `int | float | str | ... omitted 3 union elements`, found `object`
- Found 243 diagnostics
+ Found 295 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/data_drawer.py:383:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `@Todo` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:383:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/config/config_options_tests.py:1484:9: error[type-assertion-failure] Type `int | None` does not match asserted type `@Todo | int | None`
+ mkdocs/tests/config/config_options_tests.py:1484:9: error[type-assertion-failure] Type `int | None` does not match asserted type `Unknown | int | None`

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/builders/latex/transforms.py:118:22: error[not-subscriptable] Cannot subscript object of type `Node & ~AlwaysTruthy` with no `__getitem__` method
+ sphinx/environment/collectors/toctree.py:170:41: error[unresolved-attribute] Object of type `Node` has no attribute `append`
- Found 344 diagnostics
+ Found 346 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/annotations.py:2520:30: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `Self@add_to_slash_cmds`
+ tanjun/commands/slash.py:1435:22: error[invalid-assignment] Object of type `<special-form 'typing.Self'>` is not assignable to `CommandInteractionOption | None`
+ tanjun/commands/slash.py:1441:42: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `CommandInteractionOption | None`
+ tanjun/commands/slash.py:1458:22: error[invalid-assignment] Object of type `<special-form 'typing.Self'>` is not assignable to `AutocompleteInteractionOption | None`
+ tanjun/commands/slash.py:1464:38: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `AutocompleteInteractionOption | None`
+ tanjun/commands/slash.py:1466:45: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `AutocompleteInteractionOption | None`
+ tanjun/commands/slash.py:1811:64: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `object`
+ tanjun/commands/slash.py:1811:80: error[invalid-argument-type] Argument is incorrect: Expected `str | int | float`, found `object`
+ tanjun/commands/slash.py:2094:64: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `object`
+ tanjun/commands/slash.py:2094:80: error[invalid-argument-type] Argument is incorrect: Expected `str | int | float`, found `object`
- Found 131 diagnostics
+ Found 141 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/backend/ninjabackend.py:858:9: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `@Todo | str | list[str] | list[Unknown]`
+ mesonbuild/backend/ninjabackend.py:858:9: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | str | list[str] | list[Unknown]`
- mesonbuild/backend/ninjabackend.py:859:9: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `@Todo | str | list[str] | list[Unknown]`
+ mesonbuild/backend/ninjabackend.py:859:9: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | str | list[str] | list[Unknown]`
- mesonbuild/backend/ninjabackend.py:861:13: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `@Todo | str | list[str] | list[Unknown]`
+ mesonbuild/backend/ninjabackend.py:861:13: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | str | list[str] | list[Unknown]`
+ mesonbuild/backend/ninjabackend.py:1219:27: error[unresolved-attribute] Object of type `object` has no attribute `get_outputs`
+ mesonbuild/backend/ninjabackend.py:3472:16: warning[possibly-missing-attribute] Attribute `is_aix` may be missing on object of type `Unknown | MachineInfo | None`
+ mesonbuild/backend/ninjabackend.py:3535:16: warning[possibly-missing-attribute] Attribute `is_windows` may be missing on object of type `Unknown | MachineInfo | None`
+ mesonbuild/backend/ninjabackend.py:3535:34: warning[possibly-missing-attribute] Attribute `is_cygwin` may be missing on object of type `Unknown | MachineInfo | None`
+ mesonbuild/dependencies/cuda.py:131:164: error[no-matching-overload] No overload of function `realpath` matches arguments
+ mesonbuild/interpreter/interpreter.py:1946:50: error[invalid-argument-type] Argument to bound method `find_program_impl` is incorrect: Expected `list[File | str]`, found `str | File`
+ mesonbuild/interpreter/interpreter.py:3611:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
+ mesonbuild/interpreterbase/interpreterbase.py:469:79: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/mconf.py:304:36: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]].__getitem__(key: str, /) -> dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]` cannot be called with key of type `None` on object of type `dict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]] & ~AlwaysFalsy`
- mesonbuild/modules/pkgconfig.py:582:43: error[invalid-assignment] Object of type `@Todo | None` is not assignable to `str | bool`
+ mesonbuild/modules/pkgconfig.py:582:43: error[invalid-assignment] Object of type `str | Literal[False] | None` is not assignable to `str | bool`
- mesonbuild/modules/pkgconfig.py:754:103: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None | Unknown`
+ mesonbuild/modules/pkgconfig.py:754:103: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
- mesonbuild/modules/python.py:536:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `str`, found `@Todo | None`
+ mesonbuild/modules/python.py:536:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `str`, found `Unknown | str | None`
- mesonbuild/modules/python.py:538:72: error[invalid-ar

... (truncated 532 lines) ...
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
