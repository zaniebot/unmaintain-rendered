```yaml
number: 22487
title: "[ty] Bind `typing.Self` in class attributes and assignment"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: charlie/self
created_at: 2026-01-09T23:24:18Z
updated_at: 2026-01-19T16:23:25Z
url: https://github.com/astral-sh/ruff/pull/22487
synced_at: 2026-01-19T16:27:07Z
```

# [ty] Bind `typing.Self` in class attributes and assignment

---

_@charliermarsh_

## Summary

We now bind `typing.Self` to its class context so class-body attribute annotations resolve correctly, including dataclass fields and regular instance assignments.

Closes https://github.com/astral-sh/ty/issues/1124.


---

_Label `ty` added by @charliermarsh on 2026-01-09 23:24_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 23:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 71.78% to 72.09%. The percentage of expected errors that received a diagnostic held steady at 60.22%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 669 | 669 | +0 |  |
| False Positives | 263 | 259 | -4 | ⏬ (✅) |
| False Negatives | 442 | 442 | +0 |  |
| Total Diagnostics | 932 | 928 | -4 | ⏬ |
| Precision | 71.78% | 72.09% | +0.31% | ⏫ (✅) |
| Recall | 60.22% | 60.22% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_self_advanced.py:36:9](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_self_advanced.py#L36) | type-assertion-failure | Type `list[Self@method2]` does not match asserted type `list[<special-form 'typing.Self'>]` |
| [generics_self_advanced.py:37:9](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_self_advanced.py#L37) | type-assertion-failure | Type `Self@method2` does not match asserted type `<special-form 'typing.Self'>` |
| [generics_self_attributes.py:29:5](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_self_attributes.py#L29) | invalid-assignment | Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `<special-form 'typing.Self'> \| None` |
| [generics_self_usage.py:50:34](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_self_usage.py#L50) | invalid-assignment | Object of type `def foo(self) -> int` is not assignable to `(<special-form 'typing.Self'>, /) -> int` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-09 23:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_base.py:501:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 16 diagnostics
+ Found 15 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/python_generator.py:270:51: error[unresolved-attribute] Object of type `GrammarVisitor` has no attribute `keywords`
- src/pegen/python_generator.py:271:56: error[unresolved-attribute] Object of type `GrammarVisitor` has no attribute `soft_keywords`
- Found 48 diagnostics
+ Found 46 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:838:28: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_effectively_cancelled`
- src/anyio/_backends/_asyncio.py:856:40: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_host_task`
- src/anyio/_backends/_asyncio.py:859:28: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_host_task`
- src/anyio/_backends/_asyncio.py:883:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `anyio._backends._asyncio.CancelScope | None`, found `anyio._core._tasks.CancelScope`
- src/anyio/_backends/_asyncio.py:885:9: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_tasks`
- Found 92 diagnostics
+ Found 87 diagnostics

parso (https://github.com/davidhalter/parso)
- parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/distlib/util.py:1484:36: warning[possibly-missing-attribute] Attribute `getpeercert` may be missing on object of type `socket | Any`
+ src/pip/_vendor/distlib/util.py:1484:36: warning[possibly-missing-attribute] Attribute `getpeercert` may be missing on object of type `Unknown | socket`
- src/pip/_vendor/urllib3/poolmanager.py:508:13: warning[possibly-missing-attribute] Attribute `host` may be missing on object of type `Unknown | None`
- src/pip/_vendor/urllib3/poolmanager.py:508:30: warning[possibly-missing-attribute] Attribute `port` may be missing on object of type `Unknown | None`
- src/pip/_vendor/urllib3/poolmanager.py:508:47: warning[possibly-missing-attribute] Attribute `scheme` may be missing on object of type `Unknown | None`
- Found 594 diagnostics
+ Found 591 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
- lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
- lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `_BufferedReaderStream | Unknown | None`
+ lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `Unknown | None`
- Found 4337 diagnostics
+ Found 4335 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- src/bandersnatch/mirror.py:353:52: error[invalid-argument-type] Argument to bound method `sync_index_page` is incorrect: Expected `int`, found `int | None`
- Found 78 diagnostics
+ Found 76 diagnostics

stone (https://github.com/dropbox/stone)
- stone/backends/js_client.py:167:27: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:170:29: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/python_types.py:210:24: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- Found 252 diagnostics
+ Found 249 diagnostics

jinja (https://github.com/pallets/jinja)
- tests/test_loader.py:240:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:240:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:244:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:244:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:261:9: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:261:9: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:263:16: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:263:16: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:265:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:265:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:282:9: error[invalid-assignment] Object of type `ChoiceLoader` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:282:9: error[invalid-assignment] Object of type `ChoiceLoader` is not assignable to attribute `loader` on type `Unknown | Environment | None`
- tests/test_loader.py:283:14: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:283:14: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:285:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:285:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:287:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:287:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:292:9: error[invalid-assignment] Object of type `PrefixLoader` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:292:9: error[invalid-assignment] Object of type `PrefixLoader` is not assignable to attribute `loader` on type `Unknown | Environment | None`
- tests/test_loader.py:294:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:294:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:298:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:298:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:300:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:300:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:306:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:306:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | Environment | None`
- tests/test_loader.py:315:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:315:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | Environment | None`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `UpdateDictMixin[Any, Any]`
- src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `Self@setdefault`
- src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `Self@pop`
- src/werkzeug/datastructures/structures.py:1053:9: error[invalid-assignment] Object of type `((Self@__init__, /) -> None) | None` is not assignable to attribute `on_update` of type `((<special-form 'typing.Self'>, /) -> None) | None`
+ src/werkzeug/datastructures/structures.py:1053:9: error[invalid-assignment] Object of type `((Self@__init__, /) -> None) | None` is not assignable to attribute `on_update` of type `((CallbackDict[Unknown, Unknown], /) -> None) | None`
- src/werkzeug/datastructures/structures.py:1104:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@remove`
- src/werkzeug/datastructures/structures.py:1119:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@update`
- src/werkzeug/datastructures/structures.py:1159:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@clear`
- src/werkzeug/datastructures/structures.py:1185:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@__delitem__`
- src/werkzeug/datastructures/structures.py:1193:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@__setitem__`
- Found 407 diagnostics
+ Found 399 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rtcpeerconnection.py:571:25: warning[possibly-missing-attribute] Attribute `media` may be missing on object of type `SessionDescription | None | @Todo`
- src/aiortc/rtcpeerconnection.py:575:21: error[invalid-argument-type] Argument to function `create_media_description_for_transceiver` is incorrect: Expected `RTCRtpTransceiver`, found `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:578:25: warning[possibly-missing-attribute] Attribute `direction` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:578:48: error[invalid-argument-type] Argument to function `and_direction` is incorrect: Expected `str`, found `str | None | @Todo`
- src/aiortc/rtcpeerconnection.py:578:48: warning[possibly-missing-attribute] Attribute `_offerDirection` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:580:21: error[invalid-argument-type] Argument to function `create_media_description_for_transceiver` is incorrect: Expected `str`, found `str | None | @Todo`
- src/aiortc/rtcpeerconnection.py:580:25: warning[possibly-missing-attribute] Attribute `mid` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:582:33: warning[possibly-missing-attribute] Attribute `receiver` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:593:17: error[invalid-assignment] Object of type `Unknown & ~Literal["auto"]` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
+ src/aiortc/rtcpeerconnection.py:593:17: error[invalid-assignment] Object of type `@Todo & ~Literal["auto"]` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
- src/aiortc/rtcpeerconnection.py:675:33: error[invalid-argument-type] Argument to function `get_media` is incorrect: Expected `SessionDescription`, found `SessionDescription | None | @Todo`
- src/aiortc/rtcpeerconnection.py:676:34: error[invalid-argument-type] Argument to function `get_media` is incorrect: Expected `SessionDescription`, found `SessionDescription | None | @Todo`
- src/aiortc/rtcpeerconnection.py:684:17: warning[possibly-missing-attribute] Attribute `_set_mline_index` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:687:25: error[invalid-argument-type] Argument to function `create_media_description_for_transceiver` is incorrect: Expected `RTCRtpTransceiver`, found `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:689:35: warning[possibly-missing-attribute] Attribute `direction` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:833:17: warning[possibly-missing-attribute] Attribute `_set_mid` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:849:21: warning[possibly-missing-attribute] Attribute `receiver` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:863:50: warning[possibly-missing-attribute] Attribute `receiver` may be missing on object of type `RTCRtpTransceiver | None | @Todo`
- src/aiortc/rtcpeerconnection.py:1237:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> MediaDescription, (s: slice[Any, Any, Any], /) -> list[MediaDescription]]` cannot be called with key of type `None` on object of type `list[MediaDescription]`
- src/aiortc/rtcpeerconnection.py:1237:17: warning[possibly-missing-attribute] Attribute `media` may be missing on object of type `SessionDescription | None | @Todo`
- src/aiortc/rtcpeerconnection.py:1407:71: warning[possibly-missing-attribute] Attribute `media` may be missing on object of type `SessionDescription | None | @Todo`
- src/aiortc/rtcsctptransport.py:1095:33: error[invalid-argument-type] Argument to bound method `_receive` is incorrect: Expected `int`, found `int | bytes | @Todo`
- src/aiortc/rtcsctptransport.py:1095:33: error[invalid-argument-type] Argument to bound method `_receive` is incorrect: Expected `int`, found `int | bytes | @Todo`
- src/aiortc/rtcsctptransport.py:1095:33: error[invalid-argument-type] Argument to bound method `_receive` is incorrect: Expected `bytes`, found `int | bytes | @Todo`
- src/aiortc/rtcsctptransport.py:1131:37: error[invalid-argument-type] Argument to bound method `_receive` is incorrect: Expected `int`, found `int | bytes | @Todo`
- src/aiortc/rtcsctptransport.py:1131:37: error[invalid-argument-type] Argument to bound method `_receive` is incorrect: Expected `int`, found `int | bytes | @Todo`
- src/aiortc/rtcsctptransport.py:1131:37: error[invalid-argument-type] Argument to bound method `_receive` is incorrect: Expected `bytes`, found `int | bytes | @Todo`
- Found 194 diagnostics
+ Found 169 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_scheduler.py:180:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ tests/test_scheduler.py:180:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | str | None`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/incremental_graph.py:349:27: error[invalid-argument-type] Argument to bound method `_enqueue` is incorrect: Expected `ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | StreamItemsResult`, found `object`
- src/graphql/execution/incremental_graph.py:383:16: error[unresolved-attribute] Object of type `object` has no attribute `item`
- src/graphql/execution/incremental_graph.py:393:66: error[unresolved-attribute] Object of type `object` has no attribute `errors`
- src/graphql/execution/incremental_graph.py:396:25: error[unresolved-attribute] Object of type `object` has no attribute `item`
- src/graphql/execution/incremental_graph.py:397:16: error[unresolved-attribute] Object of type `object` has no attribute `errors`
- src/graphql/execution/incremental_graph.py:398:31: error[unresolved-attribute] Object of type `object` has no attribute `errors`
- src/graphql/execution/incremental_graph.py:399:16: error[unresolved-attribute] Object of type `object` has no attribute `incremental_data_records`
- src/graphql/execution/incremental_graph.py:400:49: error[unresolved-attribute] Object of type `object` has no attribute `incremental_data_records`
- src/graphql/type/definition.py:430:27: error[invalid-argument-type] Invalid argument to key "parse_literal" with declared type `((ValueNode, dict[str, Any] | None, /) -> Any) | None` on TypedDict `GraphQLScalarTypeKwargs`: value of type `None | (bound method Self@to_kwargs.parse_literal(node: ValueNode, variables: dict[str, Any] | None = None) -> Any) | (def parse_literal(self, node: ValueNode, variables: dict[str, Any] | None = None) -> Any)`
+ src/graphql/type/definition.py:430:27: error[invalid-argument-type] Invalid argument to key "parse_literal" with declared type `((ValueNode, dict[str, Any] | None, /) -> Any) | None` on TypedDict `GraphQLScalarTypeKwargs`: value of type `None | (def parse_literal(self, node: ValueNode, variables: dict[str, Any] | None = None) -> Any)`
- Found 641 diagnostics
+ Found 633 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/python.py:1043:34: error[invalid-argument-type] Argument to function `ascii_escaped` is incorrect: Expected `bytes | str`, found `object`
- src/_pytest/subtests.py:85:24: error[not-iterable] Object of type `None` is not iterable
- Found 413 diagnostics
+ Found 411 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/boost.py:362:9: error[unresolved-attribute] Object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` has no attribute `append`
- boostedblob/boost.py:368:16: error[unresolved-attribute] Object of type `Collection[Awaitable[T@OrderedMappingBoostable]] & ~AlwaysFalsy` has no attribute `popleft`
- boostedblob/boost.py:382:19: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` with no `__getitem__` method
- boostedblob/boost.py:402:9: error[unresolved-attribute] Object of type `Collection[Awaitable[T@UnorderedMappingBoostable]]` has no attribute `add`
- boostedblob/boost.py:413:55: error[unresolved-attribute] Object of type `Awaitable[T@UnorderedMappingBoostable]` has no attribute `done`
- boostedblob/boost.py:416:9: error[unresolved-attribute] Object of type `Collection[Awaitable[T@UnorderedMappingBoostable]]` has no attribute `remove`
- Found 21 diagnostics
+ Found 15 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/server.py:604:13: error[invalid-argument-type] Argument to bound method `find_missing_objects` is incorrect: Expected `((bytes, /) -> None) | None`, found `(bound method Self@handle.progress(message: bytes) -> None) | (def progress(self, message: bytes) -> None)`
+ dulwich/server.py:604:13: error[invalid-argument-type] Argument to bound method `find_missing_objects` is incorrect: Expected `((bytes, /) -> None) | None`, found `def progress(self, message: bytes) -> None`
- dulwich/server.py:649:21: error[invalid-argument-type] Argument to function `filter_pack_objects_with_paths` is incorrect: Expected `((bytes, /) -> None) | None`, found `(bound method Self@handle.progress(message: bytes) -> None) | (def progress(self, message: bytes) -> None)`
+ dulwich/server.py:649:21: error[invalid-argument-type] Argument to function `filter_pack_objects_with_paths` is incorrect: Expected `((bytes, /) -> None) | None`, found `def progress(self, message: bytes) -> None`

PyGithub (https://github.com/PyGithub/PyGithub)
- tests/Requester.py:216:9: warning[possibly-missing-attribute] Attribute `info` may be missing on object of type `Unknown | None | MagicMock`
- tests/Requester.py:263:9: warning[possibly-missing-attribute] Attribute `info` may be missing on object of type `Unknown | None | MagicMock`
- Found 299 diagnostics
+ Found 297 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/console/commands/build.py:242:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/console/commands/test_show.py:42:12: warning[redundant-cast] Value is already of type `F@output_format_parametrize`
- Found 977 diagnostics
+ Found 979 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/iostream.py:1343:13: warning[possibly-missing-attribute] Attribute `getpeername` may be missing on object of type `Unknown | None | socket`
+ tornado/iostream.py:1343:13: warning[possibly-missing-attribute] Attribute `getpeername` may be missing on object of type `Unknown | socket | None`
- tornado/iostream.py:1363:13: warning[possibly-missing-attribute] Attribute `do_handshake` may be missing on object of type `Unknown | None | socket`
+ tornado/iostream.py:1363:13: warning[possibly-missing-attribute] Attribute `do_handshake` may be missing on object of type `Unknown | socket | None`
- tornado/iostream.py:1375:28: warning[possibly-missing-attribute] Attribute `getpeername` may be missing on object of type `Unknown | None | socket`
+ tornado/iostream.py:1375:28: warning[possibly-missing-attribute] Attribute `getpeername` may be missing on object of type `Unknown | socket | None`
- tornado/iostream.py:1379:47: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `Unknown | None | socket`
+ tornado/iostream.py:1379:47: warning[possibly-missing-attribute] Attribute `fileno` may be missing on object of type `Unknown | socket | None`
- tornado/iostream.py:1461:37: error[invalid-argument-type] Argument to bound method `remove_handler` is incorrect: Expected `int | _Selectable`, found `Unknown | None | socket`
+ tornado/iostream.py:1461:37: error[invalid-argument-type] Argument to bound method `remove_handler` is incorrect: Expected `int | _Selectable`, found `Unknown | socket | None`
- tornado/iostream.py:1466:13: error[invalid-argument-type] Argument to function `ssl_wrap_socket` is incorrect: Expected `socket`, found `Unknown | None | socket`
+ tornado/iostream.py:1466:13: error[invalid-argument-type] Argument to function `ssl_wrap_socket` is incorrect: Expected `socket`, found `Unknown | socket | None`
- tornado/iostream.py:1542:24: warning[possibly-missing-attribute] Attribute `recv_into` may be missing on object of type `Unknown | None | socket`
+ tornado/iostream.py:1542:24: warning[possibly-missing-attribute] Attribute `recv_into` may be missing on object of type `Unknown | socket | None`
- tornado/queues.py:175:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | deque[Unknown]`
- tornado/queues.py:310:16: warning[possibly-missing-attribute] Attribute `popleft` may be missing on object of type `Unknown | None | deque[Unknown]`
- tornado/queues.py:313:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | deque[Unknown]`
- tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[Unknown]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[Unknown]`, found `Unknown | list[Unknown] | deque[Unknown]`
- tornado/queues.py:385:30: error[invalid-argument-type] Argument to function `heappop` is incorrect: Expected `list[Unknown]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:385:30: error[invalid-argument-type] Argument to function `heappop` is incorrect: Expected `list[Unknown]`, found `Unknown | list[Unknown] | deque[Unknown]`
- tornado/queues.py:419:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | list[Unknown] | deque[Unknown]`
- tornado/queues.py:422:16: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `Unknown | None | list[Unknown] | deque[Unknown]`
- tornado/test/netutil_test.py:100:24: warning[possibly-missing-attribute] Attribute `resolve` may be missing on object of type `Unknown | None | OverrideResolver`
- tornado/test/netutil_test.py:103:24: warning[possibly-missing-attribute] Attribute `resolve` may be missing on object of type `Unknown | None | OverrideResolver`
- tornado/test/netutil_test.py:116:9: warning[possibly-missing-attribute] Attribute `close` may be missing on object of type `Unknown | None | ThreadedResolver`
- tornado/web.py:1214:30: error[not-iterable] Object of type `Unknown | None | list[OutputTransform]` may not be iterable
- tornado/web.py:1239:30: error[not-iterable] Object of type `Unknown | None | list[OutputTransform]` may not be iterable
- tornado/websocket.py:1086:16: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1086:16: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1137:22: warning[possibly-missing-attribute] Attribute `read_bytes` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1137:22: warning[possibly-missing-attribute] Attribute `read_bytes` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1278:20: warning[possibly-missing-attribute] Attribute `closed` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1278:20: warning[possibly-missing-attribute] Attribute `closed` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1294:17: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1294:17: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1296:13: warning[possibly-missing-attribute] Attribute `close` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1296:13: warning[possibly-missing-attribute] Attribute `close` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1300:29: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1300:29: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1301:17: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1301:17: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1314:16: warning[possibly-missing-attribute] Attribute `closed` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1314:16: warning[possibly-missing-attribute] Attribute `closed` may be missing on object of type `Unknown | IOStream | None`
- tornado/websocket.py:1317:9: warning[possibly-missing-attribute] Attribute `set_nodelay` may be missing on object of type `Unknown | None | IOStream`
+ tornado/websocket.py:1317:9: warning[possibly-missing-attribute] Attribute `set_nodelay` may be missing on object of type `Unknown | IOStream | None`
- Found 327 diagnostics
+ Found 317 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/search.py:68:23: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:68:23: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:73:41: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:73:41: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:75:45: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:75:45: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:78:49: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:78:49: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:82:50: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:82:50: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- mitmproxy/tools/console/commands.py:109:38: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `(text) -> None`, found `(bound method Self@__init__.sig_mod(txt) -> Unknown) | @Todo`
- mitmproxy/tools/console/flowview.py:350:33: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `Message`
- Found 2143 diagnostics
+ Found 2141 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/connection.py:560:9: warning[possibly-missing-attribute] Attribute `settimeout` may be missing on object of type `socket | Any | None`
+ src/urllib3/connection.py:560:9: warning[possibly-missing-attribute] Attribute `settimeout` may be missing on object of type `Unknown | socket | None`
- src/urllib3/http2/connection.py:110:17: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:110:17: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:167:17: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:167:17: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:180:17: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:180:17: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:191:25: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:191:25: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:202:25: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:202:25: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:207:29: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:207:29: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:235:37: warning[possibly-missing-attribute] Attribute `recv` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:235:37: warning[possibly-missing-attribute] Attribute `recv` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:258:21: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:258:21: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
- src/urllib3/http2/connection.py:312:21: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `socket | Any | SSLTransport | None`
+ src/urllib3/http2/connection.py:312:21: warning[possibly-missing-attribute] Attribute `sendall` may be missing on object of type `Unknown | socket | SSLTransport | None`
+ src/urllib3/poolmanager.py:615:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 303 diagnostics
+ Found 304 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_raw/wrapper.py:102:13: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `((...) -> object) | ((...) -> object)`
- src/antidote/core/_raw/wrapper.py:136:13: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `((...) -> Awaitable[object]) | ((...) -> Awaitable[object])`
+ src/antidote/core/_raw/wrapper.py:102:13: error[unresolved-attribute] Object of type `(...) -> object` has no attribute `__get__`
+ src/antidote/core/_raw/wrapper.py:136:13: error[unresolved-attribute] Object of type `(...) -> Awaitable[object]` has no attribute `__get__`

psycopg (https://github.com/psycopg/psycopg)
- docs/lib/libpq_docs.py:99:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 652 diagnostics
+ Found 651 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/core/smtlib/constraints.py:81:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `_parent` on type `Unknown | None | Self@__enter__`
+ manticore/core/smtlib/constraints.py:81:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `_parent` on type `Unknown | None | Self@__exit__`
+ manticore/native/cpu/abstractcpu.py:350:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/native/test_armv7unicorn.py:1518:9: warning[possibly-missing-attribute] Attribute `STACK` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1520:15: warning[possibly-missing-attribute] Attribute `symbolicate_buffer` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1521:9: warning[possibly-missing-attribute] Attribute `write_bytes` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1527:9: warning[possibly-missing-attribute] Attribute `STACK` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1529:15: warning[possibly-missing-attribute] Attribute `symbolicate_buffer` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1531:9: warning[possibly-missing-attribute] Attribute `write_bytes` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1542:15: warning[possibly-missing-attribute] Attribute `new_symbolic_value` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1576:15: warning[possibly-missing-attribute] Attribute `new_symbolic_value` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1580:13: warning[possibly-missing-attribute] Attribute `emulate` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1580:30: warning[possibly-missing-attribute] Attribute `decode_instruction` may be missing on object of type `Unknown | None`
- tests/native/test_armv7unicorn.py:1580:58: warning[possibly-missing-attribute] Attribute `PC` may be missing on object of type `Unknown | None`
- Found 11070 diagnostics
+ Found 11060 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/models/densenet.py:62:37: error[invalid-argument-type] Argument to bound method `bn_function` is incorrect: Expected `list[Unknown]`, found `tuple[Unknown, ...]`
+ torchvision/prototype/datasets/_builtin/caltech.py:84:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ torchvision/prototype/datasets/_builtin/caltech.py:95:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ torchvision/prototype/datasets/_builtin/imagenet.py:141:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `int | float`, found `int | float | tuple[int | float, int | float] | @Todo`
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `list[int]`, found `int | float | tuple[int | float, int | float] | @Todo`
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `int | float`, found `int | float | tuple[int | float, int | float] | @Todo`
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `list[int | float]`, found `int | float | tuple[int | float, int | float] | @Todo`
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `InterpolationMode`, found `int | float | tuple[int | float, int | float] | @Todo`
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `list[int | float] | None`, found `int | float | tuple[int | float, int | float] | @Todo`
- torchvision/transforms/transforms.py:1537:30: error[invalid-argument-type] Argument to function `affine` is incorrect: Expected `list[int] | None`, found `int | float | tuple[int | float, int | float] | @Todo`
- Found 1403 diagnostics
+ Found 1398 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:1184:79: error[invalid-argument-type] Argument to bound method `initialize_from_top_level_project_call` is incorrect: Expected `dict[OptionKey, str | int | list[str]]`, found `list[str]`
- mesonbuild/interpreter/interpreter.py:1190:72: error[invalid-argument-type] Argument to bound method `initialize_from_subproject_call` is incorrect: Expected `dict[OptionKey, str | int | list[str]]`, found `list[str]`
- Found 2156 diagnostics
+ Found 2154 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceEc2.py:196:9: error[invalid-assignment] Object of type `Unknown | None` is not assignable to attribute `metadata` of type `dict[Unknown, Unknown]`
- cloudinit/sources/DataSourceEc2.py:196:25: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `(dict[Unknown, Unknown] & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- cloudinit/sources/DataSourceEc2.py:197:29: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `(dict[Unknown, Unknown] & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- cloudinit/sources/DataSourceEc2.py:199:13: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `(dict[Unknown, Unknown] & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- cloudinit/sources/DataSourceLXD.py:119:9: warning[possibly-missing-attribute] Attribute `connect` may be missing on object of type `socket | Any | None`
+ cloudinit/sources/DataSourceLXD.py:119:9: warning[possibly-missing-attribute] Attribute `connect` may be missing on object of type `Unknown | None | socket`
- cloudinit/sources/DataSourceSmartOS.py:243:16: warning[possibly-missing-attribute] Attribute `exists` may be missing on object of type `Unknown | Literal["_unset"] | None`
+ cloudinit/sources/DataSourceSmartOS.py:243:16: warning[possibly-missing-attribute] Attribute `exists` may be missing on object of type `Unknown | None`
- cloudinit/sources/DataSourceSmartOS.py:251:9: warning[possibly-missing-attribute] Attribute `open_transport` may be missing on object of type `Unknown | Literal["_unset"] | None`
+ cloudinit/sources/DataSourceSmartOS.py:251:9: warning[possibly-missing-attribute] Attribute `open_transport` may be missing on object of type `Unknown | None`
- cloudinit/sources/DataSourceSmartOS.py:255:27: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | Literal["_unset"] | None`
+ cloudinit/sources/DataSourceSmartOS.py:255:27: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | None`
- cloudinit/sources/DataSourceSmartOS.py:258:27: warning[possibly-missing-attribute] Attribute `get_json` may be missing on object of type `Unknown | Literal["_unset"] | None`
+ cloudinit/sources/DataSourceSmartOS.py:258:27: warning[possibly-missing-attribute] Attribute `get_json` may be missing on object of type `Unknown | None`
- cloudinit/sources/DataSourceSmartOS.py:260:9: warning[possibly-missing-attribute] Attribute `close_transport` may be missing on object of type `Unknown | Literal["_unset"] | None`
+ cloudinit/sources/DataSourceSmartOS.py:260:9: warning[possibly-missing-attribute] Attribute `close_transport` may be missing on object of type `Unknown | None`
- tests/unittests/sources/test_init.py:87:32: warning[possibly-missing-attribute] Attribute `sys_cfg` may be missing on

... (truncated 1970 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-09 23:28_

(A lot of conformance and ecosystem changes to review before this will be ready.)

---

_Comment by @charliermarsh on 2026-01-10 00:43_

The `python-htmlgen` diagnostics appear to be correct in that there's an inheritance chain that ultimately calls `Element.__getattr__` and delegates attribute lookups to `self.children`.

Edit: Oh, but they have this in the stub:

```python
class Element(NonVoidElement, Sized):
    children: HTMLChildGenerator
    def __init__(self, element_name: str) -> None: ...
    def __bool__(self) -> bool: ...
    def __getattr__(self, item: str) -> Any: ...
    def __len__(self) -> int: ...
    def __nonzero__(self) -> bool: ...
```

So maybe that should work? Reviewing...

Edit: I was wrong; we were missing the `__getattr__`. It's now fixed.


---

_Comment by @charliermarsh on 2026-01-10 00:44_

I think the `paasta` errors are also "correct" in that they're using a dynamic setter: https://github.com/Yelp/paasta/blob/4a16cce26b8bb53b5186b1ca04d5f93fe1d13791/paasta_tools/paastaapi/model_utils.py#L107C9-L107C22

Edit: I was wrong; we were missing the `__getattr__`. It's now fixed.

---

_Comment by @charliermarsh on 2026-01-10 00:48_

I believe the conformance changes are also improvements. 

The removal of `generics_self_attributes.py:29:5` is corrrect.

The removal of `generics_self_usage.py:50:34` is correct.


---

_Comment by @charliermarsh on 2026-01-10 04:03_

Current version looks better but needs more review of the ecosystem changes.

---

_Marked ready for review by @charliermarsh on 2026-01-10 17:01_

---

_Review requested from @carljm by @charliermarsh on 2026-01-10 17:01_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-10 17:01_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-10 17:01_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-10 17:01_

---

_Renamed from "[ty] Bind `typing.Self` in class attributes and assignment" to "[ty] Fix incorrect `Self` binding for bound methods stored as instance attributes" by @charliermarsh on 2026-01-10 17:20_

---

_Renamed from "[ty] Fix incorrect `Self` binding for bound methods stored as instance attributes" to "[ty] Bind `typing.Self` in class attributes and assignment" by @charliermarsh on 2026-01-10 17:21_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-14 14:20_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:2543 on 2026-01-16 15:54_

Is this change necessary, when [the implementation of `instance_member` for `Type::NominalInstance` already delegates to `.class`](https://github.com/astral-sh/ruff/blob/27d60685d018eb93805af91cebf528448344594f/crates/ty_python_semantic/src/types.rs#L2505)?

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:2562 on 2026-01-16 16:09_

I'm a little confused about the purpose of this type mapping, it seems to be mapping `typing.Self` to itself?

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2026-01-16 16:11_

---

_@ibraheemdev reviewed on 2026-01-16 16:12_

---

_@charliermarsh reviewed on 2026-01-17 15:47_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:2562 on 2026-01-17 15:47_

So I believe these are two different selfs in this context... If you consider:

```python
class LinkedList:
    next_node: Self  # Self@LinkedList

    def next(self: Self) -> Self:  # Self@next
        return self.next_node
```

Then when type-checking `return self.next_node`...

1. `self` is `Self@next` (the method's `Self` parameter).
2. We look up `next_node` on `Self@next` upper bound (`LinkedList`).
3. `LinkedList.next_node` has type `Self@LinkedList` (from the class annotation).
4. Without the mapping, we'd return `Self@LinkedList`, but the method expects `Self@next`.
5. With the mapping: `Self@LinkedList` is converted to `Self@next`.


---

_@charliermarsh reviewed on 2026-01-17 15:51_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:2543 on 2026-01-17 15:51_

No! Thank you.

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 15:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 950 | 86 | 8 |
| `possibly-missing-attribute` | 59 | 163 | 86 |
| `invalid-argument-type` | 10 | 217 | 30 |
| `invalid-assignment` | 2 | 72 | 14 |
| `invalid-return-type` | 2 | 41 | 43 |
| `unsupported-operator` | 0 | 45 | 9 |
| `call-non-callable` | 0 | 44 | 0 |
| `not-subscriptable` | 0 | 19 | 0 |
| `invalid-type-arguments` | 0 | 18 | 0 |
| `unused-ignore-comment` | 12 | 1 | 0 |
| `type-assertion-failure` | 0 | 6 | 4 |
| `no-matching-overload` | 2 | 6 | 0 |
| `not-iterable` | 0 | 8 | 0 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-raise` | 0 | 4 | 0 |
| `parameter-already-assigned` | 0 | 1 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **1,038** | **731** | **201** |


**[Full report with detailed diff](https://364d5762.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://364d5762.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @charliermarsh on 2026-01-17 15:56_

Need to review the ecosystem changes. Look likes all of the new diagnostics are in Zulip.

---

_Converted to draft by @charliermarsh on 2026-01-17 17:03_

---

_@ibraheemdev reviewed on 2026-01-17 17:04_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:2562 on 2026-01-17 17:04_

Right, but the type mapping here seems to be mapping `typing.Self` variables with the identity of `bound_typevar.typevar(db).identity(db)` to `bound_typevar`, while `Self@LinkedList` and `Self@next` have diferent identities there?

---

_Comment by @codspeed-hq[bot] on 2026-01-18 02:58_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **degrade performance by 12.47%**




`❌ 5` regressed benchmarks  
`✅ 18` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?utm_source=github&utm_medium=comment-v2&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 87.7 s | 100.2 s | -12.47% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 21 s | 22.4 s | -6.29% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 62.6 s | 65.9 s | -4.93% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 7.7 s | 8.1 s | -5.2% |
| ❌ | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 236.6 ms | 257.2 ms | -8% |
---

<sub>Comparing <code>charlie/self</code> (0c64074) with <code>main</code> (2bc4756)[^unexpected-base]</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).
[^unexpected-base]: No successful run was found on <code>main</code> (c5902a3) during the generation of this report, so 2bc4756 was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.


---

_@charliermarsh reviewed on 2026-01-18 03:22_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:2562 on 2026-01-18 03:22_

(You're right.)

---
