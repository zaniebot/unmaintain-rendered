```yaml
number: 22682
title: "[ty] Narrow on negative subscript indexing"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: charlie/sub
head: charlie/neg-sub
created_at: 2026-01-18T15:47:13Z
updated_at: 2026-01-18T17:13:06Z
url: https://github.com/astral-sh/ruff/pull/22682
synced_at: 2026-01-18T17:13:54Z
```

# [ty] Narrow on negative subscript indexing

---

_@charliermarsh_

## Summary

Negative subscripts are also indicative of a thing being subcriptable:

```python
class Subscriptable:
    def __getitem__(self, key: int) -> int:
        return 42

class NotSubscriptable: ...

def _(x: list[Subscriptable | NotSubscriptable]):
    if not isinstance(x[-1], NotSubscriptable):
        # After narrowing, x[-1] excludes NotSubscriptable, which means subscripting works
        reveal_type(x[-1])  # revealed: Subscriptable & ~NotSubscriptable
        reveal_type(x[-1][0])  # revealed: int
```


---

_Label `ty` added by @charliermarsh on 2026-01-18 15:47_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 15:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 15:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
+ pyp.py:429:34: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `list[stmt]`, found `object`
- Found 5 diagnostics
+ Found 6 diagnostics

bidict (https://github.com/jab/bidict)
+ bidict/_iter.py:28:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Maplike[KT@iteritems, VT@iteritems].__getitem__(__key: KT@iteritems, /) -> VT@iteritems` cannot be called with key of type `object` on object of type `Maplike[KT@iteritems, VT@iteritems] & ~Top[Mapping[Unknown, object]]`
+ bidict/_iter.py:28:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[Maplike[Unknown, object]].__getitem__(__key: Never, /) -> object` cannot be called with key of type `object` on object of type `Iterable[tuple[KT@iteritems, VT@iteritems]] & Top[Maplike[Unknown, object]] & ~Top[Mapping[Unknown, object]]`
- Found 16 diagnostics
+ Found 18 diagnostics

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
+ lib/spack/spack/vendor/jinja2/compiler.py:1516:33: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `str`
- lib/spack/spack/version/version_types.py:1269:63: error[invalid-argument-type] Argument to function `_next_version_str_component` is incorrect: Expected `VersionStrComponent`, found `int | VersionStrComponent`
- lib/spack/spack/version/version_types.py:1271:35: error[unsupported-operator] Operator `+` is not supported between objects of type `int | VersionStrComponent` and `Literal[1]`
- lib/spack/spack/version/version_types.py:1291:63: error[invalid-argument-type] Argument to function `_prev_version_str_component` is incorrect: Expected `VersionStrComponent`, found `int | VersionStrComponent`
- lib/spack/spack/version/version_types.py:1293:35: error[unsupported-operator] Operator `-` is not supported between objects of type `int | VersionStrComponent` and `Literal[1]`
- Found 4344 diagnostics
+ Found 4343 diagnostics

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

jinja (https://github.com/pallets/jinja)
+ src/jinja2/compiler.py:1534:33: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `str`
- Found 180 diagnostics
+ Found 181 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/sansio/response.py:608:50: error[unresolved-attribute] Object of type `object` has no attribute `to_header`
+ src/werkzeug/test.py:361:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str | None] | None`, found `dict[str, object]`
- Found 407 diagnostics
+ Found 409 diagnostics

black (https://github.com/psf/black)
- src/black/linegen.py:1610:9: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
+ src/black/ranges.py:404:28: error[invalid-argument-type] Argument to function `first_leaf` is incorrect: Expected `Leaf | Node`, found `object`
+ src/black/ranges.py:405:26: error[invalid-argument-type] Argument to function `last_leaf` is incorrect: Expected `Leaf | Node`, found `object`
- src/blib2to3/pgen2/parse.py:367:13: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/runner.py:551:43: error[not-subscriptable] Cannot subscript object of type `Item & ~AlwaysTruthy & ~AlwaysFalsy` with no `__getitem__` method
- Found 413 diagnostics
+ Found 414 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/boost.py:366:35: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]] & ~AlwaysFalsy` with no `__getitem__` method
- Found 21 diagnostics
+ Found 22 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/_check/code/codescope.py:448:13: error[invalid-argument-type] Argument to function `add_func_scope_type` is incorrect: Expected `type`, found `object`
+ beartype/_util/kind/sequence/utilseqmake.py:44:17: error[invalid-assignment] Object of type `Sequence[Unknown]` is not assignable to `list[Unknown]`
- beartype/_util/text/utiltextjoin.py:123:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_util/text/utiltextjoin.py:130:20: error[no-matching-overload] No overload of bound method `join` matches arguments
- beartype/bite/collection/infercollectionitems.py:321:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/bite/collection/infercollectionitems.py:504:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | @Todo`
+ paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | Unknown`
+ paasta_tools/paastaapi/api_client.py:720:21: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]`
+ paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
+ paasta_tools/tron_tools.py:192:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 1103 diagnostics
+ Found 1106 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/contrib/swift.py:842:56: error[not-subscriptable] Cannot subscript object of type `~None` with no `__getitem__` method
- dulwich/ignore.py:150:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:153:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:173:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
+ dulwich/object_store.py:2923:29: error[not-subscriptable] Cannot subscript object of type `(() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy` with no `__getitem__` method
- Found 229 diagnostics
+ Found 228 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/pyutils/test_ref_map.py:35:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:52:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:72:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 641 diagnostics
+ Found 638 diagnostics

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

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ tests/ignite/distributed/utils/__init__.py:484:24: warning[possibly-missing-attribute] Attribute `item` may be missing on object of type `Unknown | int | float | str`
- Found 2033 diagnostics
+ Found 2036 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` may be missing on object of type `@Todo | bool`
+ porcupine/plugins/restart.py:64:15: warning[possibly-missing-attribute] Attribute `from_state` may be missing on object of type `Any | bool`

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
+ src/check_jsonschema/format_errors.py:41:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 27 diagnostics
+ Found 28 diagnostics

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
- mypy/checker.py:7614:69: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `(list[PartialTypeScope] & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo & ~AlwaysFalsy) | bool`
+ mypy/checker.py:7614:69: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `(list[PartialTypeScope] & ~AlwaysTruthy & ~AlwaysFalsy) | bool`
- mypy/checker_shared.py:317:20: error[invalid-return-type] Return type does not match returned value: expected `TypeInfo | None`, found `TypeInfo | FuncItem | MypyFile`
- mypy/semanal.py:4504:24: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `SymbolTable | None`
- mypy/semanal.py:5613:52: error[unsupported-operator] Operator `in` is not supported between objects of type `str` and `SymbolTable | None`
- mypy/semanal.py:6943:20: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `SymbolTable | None`
- mypy/server/astdiff.py:157:67: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `object`
+ mypy/server/astdiff.py:157:67: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `Top[dict[Unknown, Unknown]]`
- mypy/server/astdiff.py:157:78: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `object`
+ mypy/server/astdiff.py:157:78: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `Top[dict[Unknown, Unknown]]`
- mypyc/analysis/dataflow.py:131:17: error[unresolved-attribute] Object of type `Op` has no attribute `label`
- mypyc/ir/pprint.py:422:17: error[unresolved-attribute] Object of type `Op` has no attribute `label`
- Found 1742 diagnostics
+ Found 1736 diagnostics

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

vision (https://github.com/pytorch/vision)
+ torchvision/transforms/functional.py:152:19: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | numpy.bool[builtins.bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | numpy.bool[builtins.bool]]], ...], /) -> ndarray[tuple[Any, ...], dtype[object]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], dtype[object]], (key: str, /) -> ndarray[tuple[object, ...], dtype[Any]], (key: list[str], /) -> ndarray[tuple[object, ...], Unknown]]` cannot be called with key of type `tuple[slice[None, None, None], slice[None, None, None], None]` on object of type `Image & ndarray[tuple[object, ...], dtype[object]]`
- torchvision/transforms/functional.py:575:23: error[invalid-assignment] Object of type `tuple[@Todo, @Todo]` is not assignable to `list[int]`
+ torchvision/transforms/functional.py:575:23: error[invalid-assignment] Object of type `tuple[int, int]` is not assignable to `list[int]`
- torchvision/transforms/functional.py:801:16: error[invalid-assignment] Object of type `tuple[@Todo, @Todo]` is not assignable to `list[int]`
+ torchvision/transforms/functional.py:801:16: error[invalid-assignment] Object of type `tuple[int, int]` is not assignable to `list[int]`
- torchvision/transforms/functional.py:852:16: error[invalid-assignment] Object of type `tuple[@Todo, @Todo]` is not assignable to `list[int]`
+ torchvision/transforms/functional.py:852:16: error[invalid-assignment] Object of type `tuple[int, int]` is not assignable to `list[int]`
- Found 1403 diagnostics
+ Found 1404 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/builders/latex/transforms.py:118:22: error[not-subscriptable] Cannot subscript object of type `Node & ~AlwaysTruthy` with no `__getitem__` method
- Found 344 diagnostics
+ Found 345 diagnostics

manticore (https://github.com/trailofbits/manticore)
+ manticore/wasm/structure.py:1670:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `F32` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1670:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `F64` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1670:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `BitVec` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1670:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `Label` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1670:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `Activation` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1672:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `F32` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1672:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `F64` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1672:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `BitVec` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1672:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `Label` on object of type `list[FuncAddr | None]`
+ manticore/wasm/structure.py:1672:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> FuncAddr | None, (s: slice[Any, Any, Any], /) -> list[FuncAddr | None]]` cannot be called with key of type `Activation` on object of type `list[FuncAddr | None]`
- Found 11070 diagnostics
+ Found 11080 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/abc.py:1808:24: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["items"], /) -> list[MessagePin], (key: Literal["has_more"], /) -> bool]` cannot be called with key of type `Literal[-1]` on object of type `ChannelPins & ~AlwaysTruthy & ~AlwaysFalsy`
+ discord/channel.py:632:31: error[unresolved-attribute] Object of type `object` has no attribute `id`
+ discord/channel.py:1276:31: error[unresolved-attribute] Object of type `object` has no attribute `id`
- discord/poll.py:406:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/threads.py:491:26: error[unresolved-attribute] Object of type `object` has no attribute `id`
- Found 542 diagnostics
+ Found 545 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/config/config_options_tests.py:1484:9: error[type-assertion-failure] Type `int | None` does not match asserted type `@Todo | int | None`
+ mkdocs/tests/config/config_options_tests.py:1484:9: error[type-assertion-failure] Type `int | None` does not match asserted type `Unknown | int | None`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:1002:9: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `@Todo` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
+ bson/json_util.py:1002:9: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `(Any, JSONOptions, /) -> Any` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
- pymongo/helpers_shared.py:193:13: error[invalid-assignment] Invalid s

... (truncated 613 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 15:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:213 on 2026-01-18 16:04_

For completeness (though it's obviously unlikely to come up), shall we also handle unary `+`? `x[+5]`, etc.

---

_@AlexWaygood approved on 2026-01-18 16:04_

I can't believe this went unnoticed for so long 😅 Thank you!

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/semantic_index/member.rs`:213 on 2026-01-18 16:47_

Yeah will do. I was also gonna put some other variants in a separate PR.

---

_@charliermarsh reviewed on 2026-01-18 16:47_

---

_@AlexWaygood reviewed on 2026-01-18 17:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:213 on 2026-01-18 17:13_

yes, I guess ideally we'd also handle things like `++42` and `--42`... definitely puntable, this fixes loads of false positives as-is!

---
