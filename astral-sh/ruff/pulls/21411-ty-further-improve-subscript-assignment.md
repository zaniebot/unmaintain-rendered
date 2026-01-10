```yaml
number: 21411
title: "[ty] Further improve subscript assignment diagnostics"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/more-subscript-assignment-improvements
created_at: 2025-11-12T20:37:37Z
updated_at: 2025-11-13T12:33:33Z
url: https://github.com/astral-sh/ruff/pull/21411
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Further improve subscript assignment diagnostics

---

_Pull request opened by @sharkdp on 2025-11-12 20:37_

## Summary

Further improve subscript assignment diagnostics, especially for `dict`s:

```py
config: dict[str, int] = {}

config["retries"] = "three"
```

<img width="1276" height="274" alt="image" src="https://github.com/user-attachments/assets/9762c733-8d1c-4a57-8c8a-99825071dc7d" />

I have many more ideas, but this looks like a reasonable first step. Thank you @AlexWaygood for some of the suggestions here.

## Test Plan

Update tests


---

_Label `ty` added by @sharkdp on 2025-11-12 20:37_

---

_Review requested from @carljm by @sharkdp on 2025-11-12 20:37_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-12 20:37_

---

_Review requested from @dcreager by @sharkdp on 2025-11-12 20:37_

---

_Label `diagnostics` added by @sharkdp on 2025-11-12 20:37_

---

_Converted to draft by @sharkdp on 2025-11-12 20:38_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 20:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-13 12:10:32.296955997 +0000
+++ new-output.txt	2025-11-13 12:10:35.753979066 +0000
@@ -745,7 +745,7 @@
 namedtuples_usage.py:34:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:35:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:40:1: error[invalid-assignment] Cannot assign to read-only property `x` on object of type `Point`
-namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to a subscript on an object of type `Point` with no `__setitem__` method
+namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to a subscript on an object of type `Point`
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str]`
@@ -970,8 +970,8 @@
 typeddicts_extra_items.py:337:1: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `clear`
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
 typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
-typeddicts_extra_items.py:342:27: error[invalid-key] Cannot access `IntDictWithNum` with a key of type `str`. Only string literals are allowed as keys on TypedDicts.
-typeddicts_extra_items.py:343:31: error[invalid-key] Invalid key of type `str` for TypedDict `IntDictWithNum`
+typeddicts_extra_items.py:342:27: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with a string literal key, got key of type `str`.
+typeddicts_extra_items.py:343:31: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with string literal keys, got key of type `str`
 typeddicts_extra_items.py:352:5: error[invalid-assignment] Object of type `dict[str, int]` is not assignable to `IntDict`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`

```

</details>




---

_Marked ready for review by @sharkdp on 2025-11-12 20:40_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 20:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:5392:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ more_itertools/more.py:5392:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- more_itertools/more.py:5393:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ more_itertools/more.py:5393:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`

parso (https://github.com/davidhalter/parso)
- parso/pgen2/generator.py:100:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
+ parso/pgen2/generator.py:100:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]`
- parso/pgen2/generator.py:105:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]` with no `__setitem__` method
+ parso/pgen2/generator.py:105:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]`
- parso/python/tokenize.py:109:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__setitem__(key: PythonVersionInfo, value: TokenCollection, /) -> None` cannot be called with a key of type `tuple[Unknown, ...]` and a value of type `Unknown` on object of type `dict[PythonVersionInfo, TokenCollection]`
+ parso/python/tokenize.py:109:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Unknown, ...]` and value of type `Unknown` on object of type `dict[PythonVersionInfo, TokenCollection]`

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/session.py:213:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, str].__setitem__(key: str, value: str, /) -> None` cannot be called with a key of type `str` and a value of type `str | bytes` on object of type `dict[str, str]`
+ pyinstrument/session.py:213:9: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `str | bytes` on object of type `dict[str, str]`

stone (https://github.com/dropbox/stone)
- stone/ir/data_types.py:1121:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1121:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- stone/ir/data_types.py:1223:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1223:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- stone/ir/data_types.py:1259:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1259:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- stone/ir/data_types.py:1275:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1275:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- stone/ir/data_types.py:1483:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1483:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- stone/ir/data_types.py:1509:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1509:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- stone/ir/data_types.py:1515:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ stone/ir/data_types.py:1515:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`

pip (https://github.com/pypa/pip)
- src/pip/_internal/network/auth.py:508:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, tuple[str | None, str | None]].__setitem__(key: str, value: tuple[str | None, str | None], /) -> None` cannot be called with a key of type `bytes` and a value of type `tuple[str, str]` on object of type `dict[str, tuple[str | None, str | None]]`
+ src/pip/_internal/network/auth.py:508:13: error[invalid-assignment] Invalid subscript assignment with key of type `bytes` and value of type `tuple[str, str]` on object of type `dict[str, tuple[str | None, str | None]]`
- src/pip/_vendor/requests/models.py:549:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:549:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- src/pip/_vendor/requests/models.py:551:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:551:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- src/pip/_vendor/requests/models.py:568:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:568:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- src/pip/_vendor/requests/models.py:579:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:579:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- src/pip/_vendor/requests/models.py:586:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:586:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- src/pip/_vendor/requests/models.py:628:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ src/pip/_vendor/requests/models.py:628:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/utils.py:91:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `object` with no `__setitem__` method
+ src/werkzeug/utils.py:91:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `object`
- tests/test_routing.py:604:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, Any]` with no `__setitem__` method
+ tests/test_routing.py:604:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, Any]`
- tests/test_routing.py:604:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ tests/test_routing.py:604:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rtcpeerconnection.py:108:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RTCRtpCodecParameters].__setitem__(key: int, value: RTCRtpCodecParameters, /) -> None` cannot be called with a key of type `int | None` and a value of type `RTCRtpCodecParameters` on object of type `dict[int, RTCRtpCodecParameters]`
+ src/aiortc/rtcpeerconnection.py:108:17: error[invalid-assignment] Invalid subscript assignment with key of type `int | None` and value of type `RTCRtpCodecParameters` on object of type `dict[int, RTCRtpCodecParameters]`
- src/aiortc/rtcpeerconnection.py:1245:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RTCRtpDecodingParameters].__setitem__(key: int, value: RTCRtpDecodingParameters, /) -> None` cannot be called with a key of type `int | None` and a value of type `RTCRtpDecodingParameters` on object of type `dict[int, RTCRtpDecodingParameters]`
+ src/aiortc/rtcpeerconnection.py:1245:17: error[invalid-assignment] Invalid subscript assignment with key of type `int | None` and value of type `RTCRtpDecodingParameters` on object of type `dict[int, RTCRtpDecodingParameters]`
- src/aiortc/rtcrtpreceiver.py:381:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RTCRtpCodecParameters].__setitem__(key: int, value: RTCRtpCodecParameters, /) -> None` cannot be called with a key of type `int | None` and a value of type `RTCRtpCodecParameters` on object of type `dict[int, RTCRtpCodecParameters]`
+ src/aiortc/rtcrtpreceiver.py:381:17: error[invalid-assignment] Invalid subscript assignment with key of type `int | None` and value of type `RTCRtpCodecParameters` on object of type `dict[int, RTCRtpCodecParameters]`

spack (https://github.com/spack/spack)
- lib/spack/spack/environment/environment.py:1292:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, dict[str, list[str]]].__setitem__(key: str, value: dict[str, list[str]], /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | list[Unknown] | dict[Unknown, Unknown]]` on object of type `dict[str, dict[str, list[str]]]`
+ lib/spack/spack/environment/environment.py:1292:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown | list[Unknown] | dict[Unknown, Unknown]]` on object of type `dict[str, dict[str, list[str]]]`
- lib/spack/spack/environment/environment.py:1311:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, list[str]].__setitem__(key: str, value: list[str], /) -> None` cannot be called with a key of type `Literal["include_concrete"]` and a value of type `dict[str, dict[str, list[str]]] & ~AlwaysFalsy` on object of type `dict[str, list[str]]`
+ lib/spack/spack/environment/environment.py:1311:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["include_concrete"]` and value of type `dict[str, dict[str, list[str]]] & ~AlwaysFalsy` on object of type `dict[str, list[str]]`
- lib/spack/spack/util/s3.py:75:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[str, str], Any].__setitem__(key: tuple[str, str], value: Any, /) -> None` cannot be called with a key of type `tuple[None | Unknown, Unknown | Literal["fetch"]]` and a value of type `Unknown` on object of type `dict[tuple[str, str], Any]`
+ lib/spack/spack/util/s3.py:75:5: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[None | Unknown, Unknown | Literal["fetch"]]` and value of type `Unknown` on object of type `dict[tuple[str, str], Any]`
- lib/spack/spack/vendor/macholib/MachO.py:405:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ lib/spack/spack/vendor/macholib/MachO.py:405:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/credentials.py:164:25: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ kopf/_cogs/structs/credentials.py:164:25: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/error.py:78:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, type[Error]].__setitem__(key: int, value: type[Error], /) -> None` cannot be called with a key of type `int` and a value of type `type` on object of type `dict[int, type[Error]]`
+ src/_pytest/_py/error.py:78:13: error[invalid-assignment] Invalid subscript assignment with key of type `int` and value of type `type` on object of type `dict[int, type[Error]]`

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/globals.py:81:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[T@TokenManager, Any].__setitem__(key: T@TokenManager, value: Any, /) -> None` cannot be called with a key of type `tuple[@Todo, ...] | (Any & ~Top[list[Unknown]])` and a value of type `Any` on object of type `dict[T@TokenManager, Any]`
- boostedblob/globals.py:82:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[T@TokenManager, int | float].__setitem__(key: T@TokenManager, value: int | float, /) -> None` cannot be called with a key of type `tuple[@Todo, ...] | (Any & ~Top[list[Unknown]])` and a value of type `Any` on object of type `dict[T@TokenManager, int | float]`
+ boostedblob/globals.py:81:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[@Todo, ...] | (Any & ~Top[list[Unknown]])` and value of type `Any` on object of type `dict[T@TokenManager, Any]`
+ boostedblob/globals.py:82:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[@Todo, ...] | (Any & ~Top[list[Unknown]])` and value of type `Any` on object of type `dict[T@TokenManager, int | float]`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_http2_client_protocol.py:114:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ tests/test_http2_client_protocol.py:114:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- tests/test_logformatter.py:187:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[Any, ...]` with no `__setitem__` method
+ tests/test_logformatter.py:187:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[Any, ...]`

paasta (https://github.com/yelp/paasta)
- paasta_tools/metrics/metrics_lib.py:98:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, @Todo].__setitem__(key: str, value: @Todo, /) -> None` cannot be called with a key of type `str | None` and a value of type `type[BaseMetrics]` on object of type `dict[str, @Todo]`
+ paasta_tools/metrics/metrics_lib.py:98:9: error[invalid-assignment] Invalid subscript assignment with key of type `str | None` and value of type `type[BaseMetrics]` on object of type `dict[str, @Todo]`
- paasta_tools/paastaapi/api_client.py:713:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ paasta_tools/paastaapi/api_client.py:713:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- paasta_tools/paastaapi/api_client.py:717:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ paasta_tools/paastaapi/api_client.py:717:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- paasta_tools/paastaapi/api_client.py:725:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ paasta_tools/paastaapi/api_client.py:725:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- paasta_tools/paastaapi/model_utils.py:169:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Self@__setattr__` with no `__setitem__` method
+ paasta_tools/paastaapi/model_utils.py:169:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Self@__setattr__`
- paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_job.py:313:29: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, HpaOverride].__setitem__(key: str, value: HpaOverride, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown]` on object of type `dict[str, HpaOverride]`
+ paasta_tools/setup_kubernetes_job.py:313:29: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `dict[Unknown | str, Unknown]` on object of type `dict[str, HpaOverride]`
- paasta_tools/tron/client.py:50:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ paasta_tools/tron/client.py:50:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/utils.py:222:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[Unknown, ...], TimeCacheEntry].__setitem__(key: tuple[Unknown, ...], value: TimeCacheEntry, /) -> None` cannot be called with a key of type `tuple[Any, ...]` and a value of type `dict[Unknown | str, Unknown | _CacheRetT@__call__ | int | float]` on object of type `dict[tuple[Unknown, ...], TimeCacheEntry]`
+ paasta_tools/utils.py:222:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Any, ...]` and value of type `dict[Unknown | str, Unknown | _CacheRetT@__call__ | int | float]` on object of type `dict[tuple[Unknown, ...], TimeCacheEntry]`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain.py:4689:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[bytes, tuple[int, list[bytes], str]].__setitem__(key: bytes, value: tuple[int, list[bytes], str], /) -> None` cannot be called with a key of type `bytes` and a value of type `tuple[object, object, Unknown | Literal[""]]` on object of type `dict[bytes, tuple[int, list[bytes], str]]`
+ dulwich/porcelain.py:4689:21: error[invalid-assignment] Invalid subscript assignment with key of type `bytes` and value of type `tuple[object, object, Unknown | Literal[""]]` on object of type `dict[bytes, tuple[int, list[bytes], str]]`

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- src/pywinctl/_pywinctl_linux.py:307:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, _WINDICT].__setitem__(key: str, value: _WINDICT, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | dict[Unknown, Unknown]]` on object of type `dict[str, _WINDICT]`
+ src/pywinctl/_pywinctl_linux.py:307:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `dict[Unknown | str, Unknown | dict[Unknown, Unknown]]` on object of type `dict[str, _WINDICT]`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/log.py:131:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ sockeye/log.py:131:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`

pybind11 (https://github.com/pybind/pybind11)
- tests/test_sequences_and_iterators.py:157:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `reversed[Unknown]` with no `__setitem__` method
+ tests/test_sequences_and_iterators.py:157:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `reversed[Unknown]`

nox (https://github.com/wntrblm/nox)
- nox/_resolver.py:167:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Node@lazy_stable_topo_sort, Literal[False]].__setitem__(key: Node@lazy_stable_topo_sort, value: Literal[False], /) -> None` cannot be called with a key of type `Node@lazy_stable_topo_sort` and a value of type `Literal[True]` on object of type `dict[Node@lazy_stable_topo_sort, Literal[False]]`
+ nox/_resolver.py:167:13: error[invalid-assignment] Invalid subscript assignment with key of type `Node@lazy_stable_topo_sort` and value of type `Literal[True]` on object of type `dict[Node@lazy_stable_topo_sort, Literal[False]]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/websocket.py:1022:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, bool].__setitem__(key: str, value: bool, /) -> None` cannot be called with a key of type `Literal["max_wbits"]` and a value of type `int` on object of type `dict[str, bool]`
- tornado/websocket.py:1024:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, bool].__setitem__(key: str, value: bool, /) -> None` cannot be called with a key of type `Literal["max_wbits"]` and a value of type `int` on object of type `dict[str, bool]`
- tornado/websocket.py:1025:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, bool].__setitem__(key: str, value: bool, /) -> None` cannot be called with a key of type `Literal["compression_options"]` and a value of type `dict[str, Any] | None` on object of type `dict[str, bool]`
+ tornado/websocket.py:1022:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["max_wbits"]` and value of type `int` on object of type `dict[str, bool]`
+ tornado/websocket.py:1024:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["max_wbits"]` and value of type `int` on object of type `dict[str, bool]`
+ tornado/websocket.py:1025:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["compression_options"]` and value of type `dict[str, Any] | None` on object of type `dict[str, bool]`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job.py:121:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:121:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:122:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:122:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:127:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:127:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:128:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:128:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:129:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:129:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:130:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:130:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:131:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:131:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/job_processor/contract_job.py:132:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ dragonchain/job_processor/contract_job.py:132:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- dragonchain/transaction_processor/level_2_actions.py:125:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `Unknown | None` and a value of type `bool` on object of type `dict[str, Any]`
+ dragonchain/transaction_processor/level_2_actions.py:125:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | None` and value of type `bool` on object of type `dict[str, Any]`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/contrib/emscripten/fetch.py:400:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Buffer` with no `__setitem__` method
+ src/urllib3/contrib/emscripten/fetch.py:400:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Buffer`

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ references/classification/utils.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- torchvision/models/densenet.py:235:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, Any]` with no `__setitem__` method
+ torchvision/models/densenet.py:235:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, Any]`
- torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `Sequence[int | float]` with no `__setitem__` method
+ torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `Sequence[int | float]`
- torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[type | str, int | float | Sequence[int | float] | None].__setitem__(key: type | str, value: int | float | Sequence[int | float] | None, /) -> None` cannot be called with a key of type `object` and a value of type `list[int | float] | None` on object of type `dict[type | str, int | float | Sequence[int | float] | None]`
+ torchvision/transforms/v2/_utils.py:69:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `list[int | float] | None` on object of type `dict[type | str, int | float | Sequence[int | float] | None]`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_adapters_map.py:147:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Unknown, Literal[False]].__setitem__(key: Unknown, value: Literal[False], /) -> None` cannot be called with a key of type `PyFormat` and a value of type `Literal[True]` on object of type `dict[Unknown, Literal[False]]`
+ psycopg/psycopg/_adapters_map.py:147:21: error[invalid-assignment] Invalid subscript assignment with key of type `PyFormat` and value of type `Literal[True]` on object of type `dict[Unknown, Literal[False]]`

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
- schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/jsonld_context.py:211:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- schema_salad/jsonld_context.py:245:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `object` with no `__setitem__` method
+ schema_salad/jsonld_context.py:245:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `object`
- schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
- schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/jsonld_context.py:252:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- schema_salad/schema.py:541:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/schema.py:541:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- schema_salad/schema.py:543:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/schema.py:543:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- schema_salad/schema.py:563:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/schema.py:563:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- schema_salad/schema.py:572:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ schema_salad/schema.py:572:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ mitmproxy/io/compat.py:324:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`

mypy (https://github.com/python/mypy)
- mypy/build.py:975:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, FgDepMeta].__setitem__(key: str, value: FgDepMeta, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | (str & ~AlwaysFalsy) | int]` on object of type `dict[str, FgDepMeta]`
+ mypy/build.py:975:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown | (str & ~AlwaysFalsy) | int]` on object of type `dict[str, FgDepMeta]`
- mypy/report.py:685:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `bytes` and a value of type `Unknown` on object of type `dict[str, Any]`
+ mypy/report.py:685:9: error[invalid-assignment] Invalid subscript assignment with key of type `bytes` and value of type `Unknown` on object of type `dict[str, Any]`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/clients.py:2720:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `str | Path` and a value of type `@Todo` on object of type `dict[str, ModuleType]`
- tanjun/clients.py:2720:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Path, ModuleType].__setitem__(key: Path, value: ModuleType, /) -> None` cannot be called with a key of type `str | Path` and a value of type `@Todo` on object of type `dict[Path, ModuleType]`
+ tanjun/clients.py:2720:13: error[invalid-assignment] Invalid subscript assignment with key of type `str | Path` and value of type `@Todo` on object of type `dict[str, ModuleType]`
+ tanjun/clients.py:2720:13: error[invalid-assignment] Invalid subscript assignment with key of type `str | Path` and value of type `@Todo` on object of type `dict[Path, ModuleType]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/exchange/exchange.py:2950:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[str, str, CandleType], DataFrame].__setitem__(key: tuple[str, str, CandleType], value: DataFrame, /) -> None` cannot be called with a key of type `tuple[str, str, CandleType]` and a value of type `Unknown | Series[Any]` on object of type `dict[tuple[str, str, CandleType], DataFrame]`
+ freqtrade/exchange/exchange.py:2950:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[str, str, CandleType]` and value of type `Unknown | Series[Any]` on object of type `dict[tuple[str, str, CandleType], DataFrame]`
- freqtrade/freqai/data_kitchen.py:827:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None` cannot be called with a key of type `Any` and a value of type `DataFrame` on object of type `dict[str, dict[str, DataFrame]]`
- freqtrade/freqai/data_kitchen.py:830:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `Any` and a value of type `dict[Unknown, Unknown]` on object of type `dict[str, DataFrame]`
- freqtrade/freqai/data_kitchen.py:836:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `dict[str, dict[str, DataFrame]]`
+ freqtrade/freqai/data_kitchen.py:827:17: error[invalid-assignment] Invalid subscript assignment with key of type `Any` and value of type `DataFrame` on object of type `dict[str, dict[str, DataFrame]]`
+ freqtrade/freqai/data_kitchen.py:830:21: error[invalid-assignment] Invalid subscript assignment with key of type `Any` and value of type `dict[Unknown, Unknown]` on object of type `dict[str, DataFrame]`
+ freqtrade/freqai/data_kitchen.py:836:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `DataFrame` on object of type `dict[str, dict[str, DataFrame]]`
- freqtrade/freqai/freqai_interface.py:890:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `Unknown | str` and a value of type `Series[Any]` on object of type `dict[str, DataFrame]`
+ freqtrade/freqai/freqai_interface.py:890:21: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | str` and value of type `Series[Any]` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/backtesting.py:1791:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `str` and a value of type `dict[str, DataFrame]` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/backtesting.py:1792:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `str` and a value of type `dict[str, DataFrame]` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/backtesting.py:1793:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `str` and a value of type `dict[str, DataFrame]` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/backtesting.py:1791:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[str, DataFrame]` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/backtesting.py:1792:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[str, DataFrame]` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/backtesting.py:1793:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[str, DataFrame]` on object of type `dict[str, DataFrame]`
- freqtrade/rpc/api_server/api_download_data.py:32:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ProgressTask].__setitem__(key: str, value: ProgressTask, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown]` on object of type `dict[str, ProgressTask]`
- freqtrade/rpc/api_server/api_download_data.py:71:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, JobsContainer].__setitem__(key: str, value: JobsContainer, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | None | dict[Unknown, Unknown] | bool]` on object of type `dict[str, JobsContainer]`
+ freqtrade/rpc/api_server/api_download_data.py:32:17: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown]` on object of type `dict[str, ProgressTask]`
+ freqtrade/rpc/api_server/api_download_data.py:71:5: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown | str | None | dict[Unknown, Unknown] | bool]` on object of type `dict[str, JobsContainer]`
- freqtrade/rpc/api_server/api_pairlists.py:92:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, JobsContainer].__setitem__(key: str, value: JobsContainer, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | None | bool | dict[Unknown, Unknown]]` on object of type `dict[str, JobsContainer]`
+ freqtrade/rpc/api_server/api_pairlists.py:92:5: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown | str | None | bool | dict[Unknown, Unknown]]` on object of type `dict[str, JobsContainer]`
- freqtrade/strategy/interface.py:1219:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, datetime].__setitem__(key: str, value: datetime, /) -> None` cannot be called with a key of type `str` and a value of type `Series[Any]` on object of type `dict[str, datetime]`
+ freqtrade/strategy/interface.py:1219:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `Series[Any]` on object of type `dict[str, datetime]`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `(Any, JSONOptions, /) -> Any` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
+ bson/json_util.py:1015:9: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `(Any, JSONOptions, /) -> Any` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
- pymongo/asynchronous/server.py:275:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ pymongo/asynchronous/server.py:275:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- pymongo/asynchronous/server.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ pymongo/asynchronous/server.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- pymongo/synchronous/server.py:275:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ pymongo/synchronous/server.py:275:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- pymongo/synchronous/server.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ pymongo/synchronous/server.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/_ridge.py:2484:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[float, float, float]` with no `__setitem__` method
+ sklearn/linear_model/_ridge.py:2484:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[float, float, float]`
- sklearn/linear_model/_stochastic_gradient.py:824:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ sklearn/linear_model/_stochastic_gradient.py:824:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- sklearn/manifold/_t_sne.py:1066:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ sklearn/manifold/_t_sne.py:1066:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- sklearn/manifold/_t_sne.py:1066:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ sklearn/manifold/_t_sne.py:1066:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
- sklearn/manifold/_t_sne.py:1068:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ sklearn/manifold/_t_sne.py:1068:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- sklearn/manifold/_t_sne.py:1068:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ sklearn/manifold/_t_sne.py:1068:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
- sklearn/manifold/_t_sne.py:1071:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ sklearn/manifold/_t_sne.py:1071:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- sklearn/manifold/_t_sne.py:1071:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `float` with no `__setitem__` method
+ sklearn/manifold/_t_sne.py:1071:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
- sklearn/metrics/_base.py:184:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ sklearn/metrics/_base.py:184:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- sklearn/neighbors/_base.py:1296:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[Unknown, Unknown]` with no `__setitem__` method
+ sklearn/neighbors/_base.py:1296:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[Unknown, Unknown]`
- sklearn/preprocessing/tests/test_function_transformer.py:92:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None` with no `__setitem__` method
+ sklearn/preprocessing/tests/test_function_transformer.py:92:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- sklearn/tree/_export.py:724:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with no `__setitem__` method
+ sklearn/tree/_export.py:724:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- sklearn/tree/_export.py:724:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int` with no `__setitem__` method
+ sklearn/tree/_export.py:724:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- sklearn/tree/_export.py:731:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str` with n

... (truncated 1528 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-12 20:45_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 20:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 0 | 630 |
| `invalid-key` | 0 | 0 | 229 |
| **Total** | **0** | **0** | **859** |

**[Full report with detailed diff](https://david-more-subscript-assignm.ecosystem-663.pages.dev/diff)** ([timing results](https://david-more-subscript-assignm.ecosystem-663.pages.dev/timing))




---

_Comment by @sharkdp on 2025-11-12 20:55_

~~Hm, there seems to be a bug in ecosystem-analyzer's diagnostic parsing. This should only show changed diagnostics.  Instead, some changes are shown as a "changed" + a "new" diagnostic. I guess it happens if there were two diagnostics of type A previously, and we now have two new diagnostics of type B.~~  Fixed

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_key_type_(d3d47de65fb3bad).snap`:22 on 2025-11-12 22:06_

I notice that in the "not subscriptable" case we take care to say "Cannot assign to a subscript on object of type ...", but here we shorthand it to "Cannot assign to ...".

In general we've taken care in our diagnostics to say "object of type ..." rather than just conflating types and inhabitants, though tbh I'm not clear if that's worth the extra words.

I do think it's a bit odd to say "Cannot assign to `dict[str, int]`" when what we mean is "Cannot assign to subscript of `dict[str, int]`". But I also don't feel strongly if you prefer the concision. 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:3877 on 2025-11-12 22:08_

I don't think saying "`None` may be missing a `__setitem__` method." would actually be the worst thing -- I wondered why it wasn't there when reading the tests. It is in fact the case that the reason for the diagnostic is that `None` has no `__setitem__` method! But I'm also not opposed to what you do here.

---

_@carljm approved on 2025-11-12 22:09_

Thank you, this is fantastic!

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-13 08:06_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-13 08:06_

---

_@sharkdp reviewed on 2025-11-13 08:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_key_type_(d3d47de65fb3bad).snap`:22 on 2025-11-13 08:12_

I wanted it to be more concise, yes. But I agree that the slightly longer form "Cannot assign to a subscript on object of type …" is better. Changed.

---

_@sharkdp reviewed on 2025-11-13 08:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:3877 on 2025-11-13 08:13_

I chose the wording *"`Xyz` may be missing a `__setitem__` method"* because it suggests to me that the user may want to consider adding such a method to `Xyz`, and that is certainly not a helpful hint for stdlib types. I changed it now to say *"`None` does not have a `__setitem__` method"* for known classes.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_key_type_(d3d47de65fb3bad).snap`:22 on 2025-11-13 10:38_

This is the same number of characters but fewer words, so it feels slightly more terse -- WDYT?

```suggestion
error[invalid-assignment]: Invalid subscript assignment on object of type `dict[str, int]` with key of type `Literal[0]` and value of type `Literal[3]`
```

or alternatively, we could put the key/value first, which feel maybe more important than the type of the object being subscripted:

```suggestion
error[invalid-assignment]: Cannot assign to a subscript with a key of type `Literal[0]` and a value of type `Literal[3]` on an object of type `dict[str, int]`
```

or

```suggestion
error[invalid-assignment]: Invalid subscript assignment with key of type `Literal[0]` and value of type `Literal[3]` on object of type `dict[str, int]`
```

the last suggestion is maybe my favourite 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_key_type_for…_(815dae276e2fd2b7).snap`:27 on 2025-11-13 10:39_

"key type `Literal[0]`" isn't quite grammatical, I don't think

```suggestion
error[invalid-key]: TypedDict `Config` can only be subscripted with a string literal key, got key of type `Literal[0]`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_No_`__setitem__`_met…_(468f62a3bdd1d60c).snap`:33 on 2025-11-13 10:41_

"may be" sounds like we're unsure here -- it's the language we use elsewhere if we're emitting a diagnostic on a union, for example, where some elements of the union are fine but other elements of the union are invalid. In this situation, I think we're very confident that it doesn't have a `__getitem__` method, so I think this should be

```suggestion
info: `ReadOnlyDict` has no `__setitem__` method.
```

---

_@AlexWaygood approved on 2025-11-13 10:43_

Fantastic, thank you!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3887 on 2025-11-13 11:23_

ah, if this is a "here's how to fix this:" subdiagnostic, it might be better as

```suggestion
                                    diagnostic.help(format_args!(
```

---

_@AlexWaygood reviewed on 2025-11-13 11:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3110 on 2025-11-13 12:25_

for consistency with the other message, which now uses the phrase "a string literal key" rather than "string literal keys"

```suggestion
                    "TypedDict `{}` can only be subscripted with a string literal key, \
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3807 on 2025-11-13 12:28_

I see that we can only give the nice subdiagnostic if it's a dict, but the top-level diagnostic summary here seems like it would be an improvement for non-dictionaries as well as dictionaries? The terms "key", "value", and "subscript assignment" aren't specific to dictionaries; they apply to any object with a `__setitem__` method.

But I suppose that can be done as a separate change

---

_Merged by @sharkdp on 2025-11-13 12:31_

---

_Closed by @sharkdp on 2025-11-13 12:31_

---

_Branch deleted on 2025-11-13 12:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3880 on 2025-11-13 12:33_

A more exact way of checking whether a class is user-defined might be:

```suggestion
                                // If it's a user-defined class, suggest adding a `__setitem__` method.
                                if object_ty
                                    .as_nominal_instance()
                                    .and_then(|instance| {
                                        file_to_module(
                                            db,
                                            instance.class(db).class_literal(db).0.file(db),
                                        )
                                    })
                                    .and_then(|module| module.search_path(db))
                                    .is_some_and(crate::SearchPath::is_first_party)
```

---

_@AlexWaygood reviewed on 2025-11-13 12:33_

---
