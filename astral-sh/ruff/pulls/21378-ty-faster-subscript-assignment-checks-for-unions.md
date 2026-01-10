```yaml
number: 21378
title: "[ty] Faster subscript assignment checks for (unions of) `TypedDict`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - performance
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/faster-subscript-assignment-checks
created_at: 2025-11-11T12:59:58Z
updated_at: 2025-11-12T19:16:40Z
url: https://github.com/astral-sh/ruff/pull/21378
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Faster subscript assignment checks for (unions of) `TypedDict`s

---

_Pull request opened by @sharkdp on 2025-11-11 12:59_

## Summary

We synthesize a (potentially large) set of `__setitem__` overloads for every item in a `TypedDict`. Previously, validation of subscript assignments on `TypedDict`s relied on actually calling `__setitem__` with the provided key and value types, which implied that we needed to do the full overload call evaluation for this large set of overloads. This PR improves the performance of subscript assignment checks on `TypedDict`s by validating the assignment directly instead of calling `__setitem__`.

This PR also adds better handling for assignments to subscripts on union and intersection types (but does not attempt to make it perfect). It achieves this by distributing the check over unions and intersections, instead of calling `__setitem__` on the union/intersection directly. We already do something similar when validating *attribute* assignments.

## Ecosystem impact

* A lot of diagnostics change their rule type, and/or split into multiple diagnostics. The new version is more verbose, but easier to understand, in my opinion
* Almost all of the invalid-key diagnostics come from pydantic, and they should all go away (including many more) when we implement https://github.com/astral-sh/ty/issues/1479
* Everything else looks correct to me. There may be some new diagnostics due to the fact that we now check intersections.

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-11-11 12:59_

---

_Label `performance` added by @sharkdp on 2025-11-11 13:00_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 13:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-12 15:50:26.056135771 +0000
+++ new-output.txt	2025-11-12 15:50:29.708160055 +0000
@@ -745,7 +745,7 @@
 namedtuples_usage.py:34:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:35:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:40:1: error[invalid-assignment] Cannot assign to read-only property `x` on object of type `Point`
-namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to object of type `Point` with no `__setitem__` method
+namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to a subscript on an object of type `Point` with no `__setitem__` method
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str]`
@@ -971,7 +971,7 @@
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
 typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
 typeddicts_extra_items.py:342:27: error[invalid-key] Cannot access `IntDictWithNum` with a key of type `str`. Only string literals are allowed as keys on TypedDicts.
-typeddicts_extra_items.py:343:31: error[invalid-key] Invalid key for TypedDict `IntDictWithNum` of type `str`
+typeddicts_extra_items.py:343:31: error[invalid-key] Invalid key of type `str` for TypedDict `IntDictWithNum`
 typeddicts_extra_items.py:352:5: error[invalid-assignment] Object of type `dict[str, int]` is not assignable to `IntDict`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-11 13:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:5392:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | list[Unknown | None] | None` may be missing
- more_itertools/more.py:5393:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | list[Unknown | None] | None` may be missing
+ more_itertools/more.py:5392:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ more_itertools/more.py:5393:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method

parso (https://github.com/davidhalter/parso)
- parso/pgen2/generator.py:100:9: error[invalid-assignment] Cannot assign to object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
+ parso/pgen2/generator.py:100:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
- parso/pgen2/generator.py:105:17: error[invalid-assignment] Cannot assign to object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
+ parso/pgen2/generator.py:105:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/macholib/MachO.py:405:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | list[Unknown]` may be missing
+ lib/spack/spack/vendor/macholib/MachO.py:405:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method

stone (https://github.com/dropbox/stone)
- stone/ir/data_types.py:1121:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown]` may be missing
- stone/ir/data_types.py:1223:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | OrderedDict[Unknown, Unknown]` may be missing
- stone/ir/data_types.py:1259:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | OrderedDict[Unknown, Unknown]` may be missing
+ stone/ir/data_types.py:1121:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1223:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1259:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- stone/ir/data_types.py:1275:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | OrderedDict[Unknown, Unknown]` may be missing
+ stone/ir/data_types.py:1275:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- stone/ir/data_types.py:1483:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | OrderedDict[Unknown, Unknown]` may be missing
+ stone/ir/data_types.py:1483:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- stone/ir/data_types.py:1509:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | OrderedDict[Unknown, Unknown]` may be missing
- stone/ir/data_types.py:1515:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | OrderedDict[Unknown, Unknown]` may be missing
+ stone/ir/data_types.py:1509:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1515:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/utils.py:91:13: error[invalid-assignment] Cannot assign to object of type `object` with no `__setitem__` method
+ src/werkzeug/utils.py:91:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `object` with no `__setitem__` method
- tests/test_routing.py:604:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | Mapping[str, Any] | None` may be missing
+ tests/test_routing.py:604:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, Any]` with no `__setitem__` method
+ tests/test_routing.py:604:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- Found 408 diagnostics
+ Found 409 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/requests/models.py:549:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | CaseInsensitiveDict` may be missing
- src/pip/_vendor/requests/models.py:551:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | CaseInsensitiveDict` may be missing
+ src/pip/_vendor/requests/models.py:549:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:551:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- src/pip/_vendor/requests/models.py:568:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | CaseInsensitiveDict` may be missing
- src/pip/_vendor/requests/models.py:579:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | CaseInsensitiveDict` may be missing
+ src/pip/_vendor/requests/models.py:568:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:579:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- src/pip/_vendor/requests/models.py:586:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | CaseInsensitiveDict` may be missing
- src/pip/_vendor/requests/models.py:628:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | CaseInsensitiveDict` may be missing
+ src/pip/_vendor/requests/models.py:586:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:628:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method

twine (https://github.com/pypa/twine)
+ twine/package.py:264:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 10 diagnostics
+ Found 11 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_http2_client_protocol.py:114:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown, Unknown] | str` may be missing
+ tests/test_http2_client_protocol.py:114:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- tests/test_logformatter.py:187:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `dict[str, Any] | tuple[Any, ...]` may be missing
+ tests/test_logformatter.py:187:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[Any, ...]` with no `__setitem__` method

paasta (https://github.com/yelp/paasta)
- paasta_tools/paastaapi/api_client.py:713:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
- paasta_tools/paastaapi/api_client.py:717:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
+ paasta_tools/paastaapi/api_client.py:713:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ paasta_tools/paastaapi/api_client.py:717:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- paasta_tools/paastaapi/api_client.py:722:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
- paasta_tools/paastaapi/api_client.py:725:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]` may be missing
+ paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ paasta_tools/paastaapi/api_client.py:725:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- paasta_tools/paastaapi/model_utils.py:169:9: error[invalid-assignment] Cannot assign to object of type `Self@__setattr__` with no `__setitem__` method
+ paasta_tools/paastaapi/model_utils.py:169:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Self@__setattr__` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:307:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:311:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:314:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:317:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:318:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:319:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:320:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/setup_kubernetes_cr.py:321:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown, Unknown]` may be missing
+ paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- paasta_tools/tron/client.py:50:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str]` may be missing
+ paasta_tools/tron/client.py:50:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/credentials.py:164:25: warning[possibly-missing-implicit-call] Method `__setitem__` of type `dict[str, object] | None` may be missing
+ kopf/_cogs/structs/credentials.py:164:25: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Unknown | Stash`
+ src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None` cannot be called with a key of type `StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Stash`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/log.py:131:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (bound method dict[Unknown | str, Unknown | str | None].__setitem__(key: Unknown | str, value: Unknown | str | None, /) -> None) | (bound method dict[Unknown | str, Unknown | str | int].__setitem__(key: Unknown | str, value: Unknown | str | int, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None])` cannot be called with a key of type `Literal["level"]` and a value of type `Unknown | Literal[20]` on object of type `Unknown | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown | str | None] | ... omitted 3 union elements`
+ sockeye/log.py:131:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["level"]` and a value of type `Unknown | Literal[20]` on object of type `list[Unknown | str]`
+ sockeye/log.py:131:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- Found 406 diagnostics
+ Found 407 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_sequences_and_iterators.py:157:5: error[invalid-assignment] Cannot assign to object of type `reversed[Unknown]` with no `__setitem__` method
+ tests/test_sequences_and_iterators.py:157:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `reversed[Unknown]` with no `__setitem__` method

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/httpclient_test.py:521:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None) | (bound method HTTPHeaders.__setitem__(name: str, value: str) -> None)` cannot be called with a key of type `Literal["User-Agent"]` and a value of type `Unknown | str | bytes` on object of type `Unknown | dict[Unknown, Unknown] | HTTPHeaders`
+ tornado/test/httpclient_test.py:521:17: error[invalid-assignment] Method `__setitem__` of type `bound method HTTPHeaders.__setitem__(name: str, value: str) -> None` cannot be called with a key of type `Literal["User-Agent"]` and a value of type `Unknown | str | bytes` on object of type `HTTPHeaders`

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/utils/env/mock_env.py:80:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 975 diagnostics
+ Found 976 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/contrib/emscripten/fetch.py:400:9: error[invalid-assignment] Cannot assign to object of type `Buffer` with no `__setitem__` method
+ src/urllib3/contrib/emscripten/fetch.py:400:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Buffer` with no `__setitem__` method

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job.py:121:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:122:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:127:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:128:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:129:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:130:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:131:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
- dragonchain/job_processor/contract_job.py:132:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None` may be missing
+ dragonchain/job_processor/contract_job.py:121:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:122:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:127:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:128:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:129:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:130:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:131:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:132:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]) | (bound method dict[Unknown | str, Unknown | int].__setitem__(key: Unknown | str, value: Unknown | int, /) -> None)` cannot be called with a key of type `Literal["text"]` and a value of type `str` on object of type `Unknown | int | str | list[dict[Unknown, Unknown]] | dict[Unknown | str, Unknown | int]`
+ mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["text"]` and a value of type `str` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]) | (bound method dict[Unknown | str, Unknown | int].__setitem__(key: Unknown | str, value: Unknown | int, /) -> None)` cannot be called with a key of type `Literal["encoding"]` and a value of type `Literal["base64"]` on object of type `Unknown | int | str | list[dict[Unknown, Unknown]] | dict[Unknown | str, Unknown | int]`
+ mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["encoding"]` and a value of type `Literal["base64"]` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]) | (bound method dict[Unknown | str, Unknown | int].__setitem__(key: Unknown | str, value: Unknown | int, /) -> None)` cannot be called with a key of type `Literal["text"]` and a value of type `Literal[""]` on object of type `Unknown | int | str | list[dict[Unknown, Unknown]] | dict[Unknown | str, Unknown | int]`
+ mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["text"]` and a value of type `Literal[""]` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]) | (bound method dict[Unknown | str, Unknown | int].__setitem__(key: Unknown | str, value: Unknown | int, /) -> None)` cannot be called with a key of type `Literal["text"]` and a value of type `str` on object of type `Unknown | int | str | list[dict[Unknown, Unknown]] | dict[Unknown | str, Unknown | int]`
+ mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["text"]` and a value of type `str` on object of type `list[dict[Unknown, Unknown]]`
- mitmproxy/io/compat.py:324:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown, Unknown] | str | ... omitted 3 union elements` may be missing
+ mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
- Found 1833 diagnostics
+ Found 1843 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["@id"]` and a value of type `@Todo` on object of type `(CommentedMap & Top[MutableMapping[Unknown, Unknown]]) | (int & Top[MutableMapping[Unknown, Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]]) | (CommentedSeq & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- schema_salad/jsonld_context.py:245:13: error[invalid-assignment] Cannot assign to object of type `object` with no `__setitem__` method
+ schema_salad/jsonld_context.py:245:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `object` with no `__setitem__` method
- schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Literal["@context"]` and a value of type `@Todo` on object of type `(CommentedMap & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]])`
+ schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- schema_salad/schema.py:541:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
- schema_salad/schema.py:543:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/schema.py:541:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:541:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/schema.py:543:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:543:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- schema_salad/schema.py:563:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["type", "items", "names", "values", "fields"]` and a value of type `MutableMapping[str, Any] | MutableSequence[Any] | str | MutableMapping[str, str] | list[Any | MutableMapping[str, str] | str]` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
- schema_salad/schema.py:572:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["symbols"]` and a value of type `list[str | Unknown]` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/schema.py:563:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["type", "items", "names", "values", "fields"]` and a value of type `MutableMapping[str, Any] | MutableSequence[Any] | str | MutableMapping[str, str] | list[Any | MutableMapping[str, str] | str]` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:563:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/schema.py:572:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["symbols"]` and a value of type `list[str | Unknown]` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:572:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
- Found 176 diagnostics
+ Found 184 diagnostics

mypy (https://github.com/python/mypy)
- mypy/report.py:685:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None)` cannot be called with a key of type `bytes` and a value of type `Unknown` on object of type `Unknown | dict[str, Any]`
+ mypy/report.py:685:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `bytes` and a value of type `Unknown` on object of type `dict[str, Any]`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_adapters_map.py:147:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown, Literal[False]].__setitem__(key: Unknown, value: Literal[False], /) -> None) | (bound method dict[Unknown, Literal[True]].__setitem__(key: Unknown, value: Literal[True], /) -> None)` cannot be called with a key of type `PyFormat` and a value of type `Literal[True]` on object of type `Unknown | dict[Unknown, Literal[False]] | dict[Unknown, Literal[True]]`
+ psycopg/psycopg/_adapters_map.py:147:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Unknown, Literal[False]].__setitem__(key: Unknown, value: Literal[False], /) -> None` cannot be called with a key of type `PyFormat` and a value of type `Literal[True]` on object of type `dict[Unknown, Literal[False]]`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/clients.py:2720:13: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None) | (bound method dict[Path, ModuleType].__setitem__(key: Path, value: ModuleType, /) -> None)` cannot be called with a key of type `str | Path` and a value of type `@Todo` on object of type `dict[str, ModuleType] | dict[Path, ModuleType]`
+ tanjun/clients.py:2720:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `str | Path` and a value of type `@Todo` on object of type `dict[str, ModuleType]`
+ tanjun/clients.py:2720:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Path, ModuleType].__setitem__(key: Path, value: ModuleType, /) -> None` cannot be called with a key of type `str | Path` and a value of type `@Todo` on object of type `dict[Path, ModuleType]`
- Found 153 diagnostics
+ Found 154 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/freqai_interface.py:890:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None)` cannot be called with a key of type `Unknown | str` and a value of type `Series[Any]` on object of type `Unknown | dict[str, DataFrame]`
+ freqtrade/freqai/freqai_interface.py:890:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `Unknown | str` and a value of type `Series[Any]` on object of type `dict[str, DataFrame]`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:4622:17: error[invalid-assignment] Method `__setitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None` cannot be called with a key of type `Unknown` and a value of type `int` on object of type `Mapping[Hashable, Any] & Top[MutableMapping[Unknown, Unknown]]`
+ xarray/core/dataset.py:4622:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[Hashable, Any]` with no `__setitem__` method
- xarray/core/groupby.py:772:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `DatasetCoordinates | Any | property` may be missing
+ xarray/core/groupby.py:772:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `property` with no `__setitem__` method
- xarray/core/options.py:332:35: error[invalid-key] Invalid key for TypedDict `T_Options` of type `str`
+ xarray/core/options.py:332:35: error[invalid-key] Invalid key of type `str` for TypedDict `T_Options`
- xarray/core/variable.py:1226:17: error[invalid-assignment] Cannot assign to object of type `Mapping[Any, int | float | tuple[int | float, int | float]]` with no `__setitem__` method
+ xarray/core/variable.py:1226:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[Any, int | float | tuple[int | float, int | float]]` with no `__setitem__` method
- xarray/groupers.py:637:9: error[invalid-assignment] Cannot assign to object of type `signedinteger[@Todo]` with no `__setitem__` method
+ xarray/groupers.py:637:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `signedinteger[@Todo]` with no `__setitem__` method
- xarray/groupers.py:639:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `signedinteger[@Todo] | @Todo` may be missing
+ xarray/groupers.py:639:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `signedinteger[@Todo]` with no `__setitem__` method

trio (https://github.com/python-trio/trio)
- src/trio/_wait_for_object.py:63:9: error[invalid-assignment] Cannot assign to object of type `HandleArray` with no `__setitem__` method
+ src/trio/_wait_for_object.py:63:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `HandleArray` with no `__setitem__` method

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/server.py:275:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Int64 | str] | int` may be missing
- pymongo/asynchronous/server.py:277:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Int64 | str] | int` may be missing
+ pymongo/asynchronous/server.py:275:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ pymongo/asynchronous/server.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
- pymongo/synchronous/server.py:275:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Int64 | str] | int` may be missing
- pymongo/synchronous/server.py:277:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Int64 | str] | int` may be missing
+ pymongo/synchronous/server.py:275:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ pymongo/synchronous/server.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method

discord.py (https://github.com/Rapptz/discord.py)
- discord/gateway.py:466:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | str | None | dict[Unknown | str, Unknown | str] | int]` may be missing
- discord/gateway.py:470:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | str | None | dict[Unknown | str, Unknown | str] | int]` may be missing
- discord/gateway.py:478:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | str | None | dict[Unknown | str, Unknown | str] | int]` may be missing
+ discord/gateway.py:466:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ discord/gateway.py:470:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ discord/gateway.py:478:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
- discord/gateway.py:735:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | int]` may be missing
- discord/gateway.py:738:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | int]` may be missing
- discord/gateway.py:741:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | int]` may be missing
+ discord/gateway.py:735:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ discord/gateway.py:738:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ discord/gateway.py:741:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
- discord/message.py:2404:30: error[invalid-key] Invalid key for TypedDict `Message` of type `str`
+ discord/message.py:2404:30: error[invalid-key] Invalid key of type `str` for TypedDict `Message`

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:319:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `None | Unknown` may be missing
+ references/classification/utils.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- torchvision/models/densenet.py:235:13: error[invalid-assignment] Cannot assign to object of type `Mapping[str, Any]` with no `__setitem__` method
+ torchvision/models/densenet.py:235:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, Any]` with no `__setitem__` method
- torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[dict[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method dict[type | str, int | float | Sequence[int | float] | None].__setitem__(key: type | str, value: int | float | Sequence[int | float] | None, /) -> None)` cannot be called with a key of type `object` and a value of type `list[int | float] | None` on object of type `(Sequence[int | float] & Top[dict[Unknown, Unknown]]) | dict[type | str, int | float | Sequence[int | float] | None]`
+ torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `Sequence[int | float]` with no `__setitem__` method
+ torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[type | str, int | float | Sequence[int | float] | None].__setitem__(key: type | str, value: int | float | Sequence[int | float] | None, /) -> None` cannot be called with a key of type `object` and a value of type `list[int | float] | None` on object of type `dict[type | str, int | float | Sequence[int | float] | None]`
- Found 1445 diagnostics
+ Found 1446 diagnostics

pyppeteer (https://github.com/pyppeteer/pyppeteer)
- pyppeteer/network_manager.py:727:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | None | dict[Unknown, Unknown]` may be missing
+ pyppeteer/network_manager.py:727:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ pyppeteer/network_manager.py:727:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- Found 87 diagnostics
+ Found 88 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/cmd/main.py:952:5: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown | str, Unknown | list[Unknown] | dict[Unknown, Unknown] | None]` may be missing
+ cloudinit/cmd/main.py:952:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- cloudinit/cmd/main.py:995:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown | str, Unknown | list[Unknown] | dict[Unknown, Unknown] | None]` may be missing
+ cloudinit/cmd/main.py:995:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ cloudinit/cmd/main.py:1003:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- cloudinit/cmd/main.py:1003:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | None` may be missing
+ cloudinit/cmd/main.py:1010:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- cloudinit/cmd/main.py:1010:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | list[Unknown] | dict[Unknown, Unknown] | None` may be missing
- cloudinit/net/network_state.py:340:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["dns"]` and a value of type `dict[Unknown | str, Unknown]` on object of type `(Unknown & ~AlwaysFalsy) | (list[Unknown] & ~AlwaysFalsy)`
+ cloudinit/net/network_state.py:340:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["dns"]` and a value of type `dict[Unknown | str, Unknown]` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:437:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["mac_address"]` and a value of type `Unknown` on object of type `Unknown | list[Unknown]`
+ cloudinit/net/network_state.py:437:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["mac_address"]` and a value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:456:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["vlan-raw-device"]` and a value of type `Unknown` on object of type `Unknown | list[Unknown]`
+ cloudinit/net/network_state.py:456:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["vlan-raw-device"]` and a value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:457:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["vlan_id"]` and a value of type `Unknown` on object of type `Unknown | list[Unknown]`
+ cloudinit/net/network_state.py:457:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["vlan_id"]` and a value of type `Unknown` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:507:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
- cloudinit/net/network_state.py:507:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["bond-master"]` and a value of type `Unknown` on object of type `Unknown | None | list[Unknown]`
+ cloudinit/net/network_state.py:507:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["bond-master"]` and a value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:558:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["bridge_ports"]` and a value of type `Unknown` on object of type `Unknown | list[Unknown]`
+ cloudinit/net/network_state.py:558:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["bridge_ports"]` and a value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:616:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["dns"]` and a value of type `dict[Unknown | str, Unknown]

... (truncated 3104 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~66MB
+ TOTAL MEMORY USAGE: ~63MB


```

</details>




---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-11 13:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 13:10_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-key` | 1,907 | 0 | 122 |
| `invalid-assignment` | 504 | 49 | 183 |
| `possibly-missing-implicit-call` | 0 | 272 | 0 |
| `unused-ignore-comment` | 3 | 0 | 0 |
| **Total** | **2,414** | **321** | **305** |

**[Full report with detailed diff](https://david-faster-subscript-assig.ecosystem-663.pages.dev/diff)** ([timing results](https://david-faster-subscript-assig.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-11 13:35_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-11 13:35_

---

_Marked ready for review by @sharkdp on 2025-11-11 13:45_

---

_Review requested from @carljm by @sharkdp on 2025-11-11 13:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-11 13:45_

---

_Review requested from @dcreager by @sharkdp on 2025-11-11 13:45_

---

_Comment by @AlexWaygood on 2025-11-11 14:00_

I wondered about how this might affect assignability and subtyping of `TypedDict`s to protocols, e.g.

```py
from ty_extensions import static_assert, is_subtype_of, is_assignable_to
from typing import Protocol, Literal, TypedDict

class Foo(Protocol):
    def __setitem__(self, key: Literal["x"], value: int, /) -> None: ...

class Bar(TypedDict):
    x: int

static_assert(is_assignable_to(Bar, Foo))
static_assert(is_subtype_of(Bar, Foo))
```

but I suppose it's impossible to test that right now, because `TypedDict`s are currently considered assignable to everything and subtypes of nothing, until https://github.com/astral-sh/ty/issues/1387 is fixed?

---

_Comment by @AlexWaygood on 2025-11-11 14:01_

If need be, we could always add a special case for `TypedDict -> Protocol` assignability/subtyping (we could synthesize the required method on the fly during the assignability/subtyping check?), but it's nice to keep that kind of special case to a minimum.

---

_Comment by @sharkdp on 2025-11-11 14:17_

> I wondered about how this might affect assignability and subtyping of `TypedDict`s to protocols, e.g.

Did you steal that from my comment yesterday? :smile:

I guess the PR would complicate the situation, but no other type checker generates these precise overloads either. I guess we could maybe keep these overloads and then prevent calling `__setitem__` in `validate_subscript_assignment`? But I'd rather like to postpone this discussion for now, unless you think it's important.

---

_Comment by @AlexWaygood on 2025-11-11 14:22_

> I guess we could maybe keep these overloads and then prevent calling `__setitem__` in `validate_subscript_assignment`?

I think I would prefer that? It'll probably make Jack's life easier, considering that he intends to start working on https://github.com/astral-sh/ty/issues/1387 today, I think. It doesn't seem like it'll be too hard to make that change in this PR, and it seems silly to get rid of the overloads here if there's a strong chance we'll only add them back in one or two days to fix this edge case.

And yes, other type checkers don't do great on this edge case either, but I hope that we can make ty better than other type checkers on things like this 😄



---

_Comment by @sharkdp on 2025-11-11 15:10_

> I think I would prefer that? It'll probably make Jack's life easier, considering that he intends to start working on [astral-sh/ty#1387](https://github.com/astral-sh/ty/issues/1387) today, I think. It doesn't seem like it'll be too hard to make that change in this PR, and it seems silly to get rid of the overloads here if there's a strong chance we'll only add them back in one or two days to fix this edge case.

I made the change, and I like it better. It lets us keep the overloads and improves the diagnostics in one case. But the optimization doesn't work anymore on pydantic :weary:. Looking into it.

---

_Renamed from "[ty] Faster subscript assignment checks on `TypedDict`s" to "[ty] Faster subscript assignment checks for (unions of) `TypedDict`s" by @sharkdp on 2025-11-12 12:35_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-12 12:36_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-12 12:36_

---

_Converted to draft by @sharkdp on 2025-11-12 12:37_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 13:02_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @codspeed-hq[bot] on 2025-11-12 13:02_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffaster-subscript-assignment-checks?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21378 will **improve performances by 86.14%**

<sub>Comparing <code>david/faster-subscript-assignment-checks</code> (ccb7a0c) with <code>main</code> (84c3cec)</sub>



### Summary

`⚡ 1` improvement  
`✅ 51` untouched  
`⏩ 2` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ffaster-subscript-assignment-checks?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 38.2 s | 20.5 s | +86.14% |
[^skipped]: 2 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Ffaster-subscript-assignment-checks?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-12 14:33_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-12 14:33_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-12 14:55_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-12 14:55_

---

_@sharkdp reviewed on 2025-11-12 15:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:3571 on 2025-11-12 15:27_

No, I am not proud, but it feels like there are more important things. Will add this as a personal TODO note.

---

_@sharkdp reviewed on 2025-11-12 15:28_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty_walltime.rs`:184 on 2025-11-12 15:28_

The increase here is caused by the fact that we now split diagnostic on unions into N_elements diagnostics.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-12 15:45_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-12 15:45_

---

_Marked ready for review by @sharkdp on 2025-11-12 16:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Possibly_missing_`__…_(efd3f0c02e9b89e9).snap`:30 on 2025-11-12 16:12_

This is definitely an improvement, but he summary line here still feels too long to me -- for a blanket "cannot assign to a subscript" diagnostic, there's no other reason why this would be emitted except for a missing `__setitem__` method. Ideally I'd go for something like this:

```
error[invalid-assignment]: Cannot assign to a subscript on an object of type `None`
 --> src/mdtest_snippet.py:2:5
  |
1 | def _(config: dict[str, int] | None) -> None:
2 |     config["retries"] = 3  # error: [invalid-assignment]
  |     ^^^^^^
  |
info: Subscript assignment implicitly calls `__setitem__`, but `None` has no `__setitem__` method
info: `config` has type `dict[str, int] | None`
info: rule `invalid-assignment` is enabled by default
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Possibly_missing_`__…_(efd3f0c02e9b89e9).snap`:27 on 2025-11-12 16:12_

the highlighted range here still looks incorrect -- shouldn't it be

```suggestion
2 |     config["retries"] = 3  # error: [invalid-assignment]
  |     ^^^^^^^^^^^^^^^^^^^^^
```

?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Unknown_key_for_all_…_(1c685d9d10678263).snap`:43 on 2025-11-12 16:15_

What about including the name of the unknown key in the summary line here and taking it out of the primary-annotation message?

```
error[invalid-key]: Invalid key "surname" for TypedDict `Person`
  --> src/mdtest_snippet.py:13:5
   |
11 |     # error: [invalid-key]
12 |     # error: [invalid-key]
13 |     being["surname"] = "unknown"
   |     ----- ^^^^^^^^^ Did you mean "name"?
   |     |
   |     TypedDict `Person` in union type `Person | Animal`
   |
info: rule `invalid-key` is enabled by default
```

We could also consider linking back to the definition of the `Person` typeddict in a subdiagnostic, though I guess that would be _very_ verbose if you had a union of typeddicts.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/typed_dict.md_-_`TypedDict`_-_Diagnostics_(e5289abf5c570c29).snap`:92 on 2025-11-12 16:17_

should we have an `info: TypedDicts can only be subscripted with string literals` subdiagnostic here? This might be a bit opaque for users unfamiliar with the rules around typeddicts

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:143 on 2025-11-12 16:26_

Calls to this function are becoming quite difficult to read because of how many arguments there are. Especially the third argument, where `None` is often passed, and the last argument which is a `bool`. We could consider refactoring this into a struct that holds all the options for better readability, e.g.

```rs
let validator = TypedDictValidator {
    context: &self.context,
    typed_dict,
    full_object_ty: None,
    key,
    value_ty,
    typed_dict_node,
    key_node,
    value_node,
    assignment_kind,
    emit_diagnostics: true
};

validator.validate();
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3659 on 2025-11-12 16:37_

Your logic here looks correct to me.

The question I had when reading this was "But surely it wouldn't be valid to assign a key of type `str` to an object of type `list[int] & Unknown` -- surely we know that all subtypes of `list[int]` only accept `int` keys? But no, we don't know that; this class is fine:

```py
from typing import overload, Iterable, SupportsIndex

class F(list[int]):
    @overload
    def __setitem__(self, key: str | SupportsIndex, value: int) -> None: ...
    @overload
    def __setitem__(self, key: slice, value: Iterable[int]) -> None: ...
    def __setitem__(self, key: str | SupportsIndex | slice, value: int | Iterable[int]) -> None: ...

F()["foo"] = 42  # no diagnostic here
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3703 on 2025-11-12 16:40_

nit: rustfmt gives up formatting regions if lines in string literals are too long

```suggestion
                                "Cannot access `{value_d}` with a key of type `{}`. \
                                Only string literals are allowed as keys on TypedDicts.",
```

---

_@AlexWaygood approved on 2025-11-12 16:41_

Awesome work. None of my comments below are blocking, just thoughts that occurred to me as I read through the diff

---

_@sharkdp reviewed on 2025-11-12 19:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Possibly_missing_`__…_(efd3f0c02e9b89e9).snap`:27 on 2025-11-12 19:01_

Yes, absolutely. I haven't forgotten the comment, and absolutely agree with it. I just wanted to postpone this.

---

_@sharkdp reviewed on 2025-11-12 19:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Unknown_key_for_all_…_(1c685d9d10678263).snap`:43 on 2025-11-12 19:03_

> What about including the name of the unknown key in the summary line here and taking it out of the primary-annotation message?

I can try that, but it's always a bit tricky to get both the full diagnostic and the concise diagnostic to look good. I still wish we had an API to set a separate concise message.

---

_@sharkdp reviewed on 2025-11-12 19:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:3703 on 2025-11-12 19:16_

Will also move this to a follow-up.

---

_Merged by @sharkdp on 2025-11-12 19:16_

---

_Closed by @sharkdp on 2025-11-12 19:16_

---

_Branch deleted on 2025-11-12 19:16_

---
