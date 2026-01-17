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
updated_at: 2026-01-17T17:06:10Z
url: https://github.com/astral-sh/ruff/pull/22487
synced_at: 2026-01-17T17:17:37Z
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

The percentage of diagnostics emitted that were expected errors increased from 70.65% to 70.80%. The percentage of expected errors that received a diagnostic held steady at 61.24%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 662 | 662 | +0 |  |
| False Positives | 275 | 273 | -2 | ⏬ (✅) |
| False Negatives | 419 | 419 | +0 |  |
| Total Diagnostics | 937 | 935 | -2 | ⏬ |
| Precision | 70.65% | 70.80% | +0.15% | ⏫ (✅) |
| Recall | 61.24% | 61.24% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
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

spack (https://github.com/spack/spack)
+ lib/spack/spack/builder.py:145:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `__class__` on type `Self@__init__` with custom `__set__` method
- lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
- lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/llnl/util/lang.py:693:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `__class__` on type `Self@__init__` with custom `__set__` method
+ lib/spack/spack/llnl/util/lang.py:695:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `__class__` on type `Self@__init__` with custom `__set__` method
- lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `_BufferedReaderStream | Unknown | None`
+ lib/spack/spack/util/s3.py:143:16: warning[possibly-missing-attribute] Attribute `read` may be missing on object of type `Unknown | None`
- Found 4344 diagnostics
+ Found 4345 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- src/bandersnatch/mirror.py:353:52: error[invalid-argument-type] Argument to bound method `sync_index_page` is incorrect: Expected `int`, found `int | None`
- Found 78 diagnostics
+ Found 76 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `UpdateDictMixin[Any, Any]`
+ src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@UpdateDictMixin`, found `UpdateDictMixin[Any, Any]`
- src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `Self@setdefault`
+ src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@UpdateDictMixin`, found `Self@setdefault`
- src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `Self@pop`
+ src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@UpdateDictMixin`, found `Self@pop`
- src/werkzeug/datastructures/structures.py:1053:9: error[invalid-assignment] Object of type `((Self@__init__, /) -> None) | None` is not assignable to attribute `on_update` of type `((<special-form 'typing.Self'>, /) -> None) | None`
- src/werkzeug/datastructures/structures.py:1104:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@remove`
- src/werkzeug/datastructures/structures.py:1119:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@update`
- src/werkzeug/datastructures/structures.py:1159:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@clear`
- src/werkzeug/datastructures/structures.py:1185:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@__delitem__`
- src/werkzeug/datastructures/structures.py:1193:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@__init__`, found `Self@__setitem__`
- Found 407 diagnostics
+ Found 401 diagnostics

stone (https://github.com/dropbox/stone)
- stone/backends/js_client.py:167:27: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:170:29: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/python_types.py:210:24: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- Found 252 diagnostics
+ Found 249 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/distlib/util.py:1484:36: warning[possibly-missing-attribute] Attribute `getpeercert` may be missing on object of type `socket | Any`
+ src/pip/_vendor/distlib/util.py:1484:36: warning[possibly-missing-attribute] Attribute `getpeercert` may be missing on object of type `Unknown | socket`
- src/pip/_vendor/urllib3/poolmanager.py:508:13: warning[possibly-missing-attribute] Attribute `host` may be missing on object of type `Unknown | None`
- src/pip/_vendor/urllib3/poolmanager.py:508:30: warning[possibly-missing-attribute] Attribute `port` may be missing on object of type `Unknown | None`
- src/pip/_vendor/urllib3/poolmanager.py:508:47: warning[possibly-missing-attribute] Attribute `scheme` may be missing on object of type `Unknown | None`
- Found 594 diagnostics
+ Found 591 diagnostics

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

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/type/definition.py:430:27: error[invalid-argument-type] Invalid argument to key "parse_literal" with declared type `((ValueNode, dict[str, Any] | None, /) -> Any) | None` on TypedDict `GraphQLScalarTypeKwargs`: value of type `None | (bound method Self@to_kwargs.parse_literal(node: ValueNode, variables: dict[str, Any] | None = None) -> Any) | (def parse_literal(self, node: ValueNode, variables: dict[str, Any] | None = None) -> Any)`
+ src/graphql/type/definition.py:430:27: error[invalid-argument-type] Invalid argument to key "parse_literal" with declared type `((ValueNode, dict[str, Any] | None, /) -> Any) | None` on TypedDict `GraphQLScalarTypeKwargs`: value of type `None | (def parse_literal(self, node: ValueNode, variables: dict[str, Any] | None = None) -> Any)`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_scheduler.py:180:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ tests/test_scheduler.py:180:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | str | None`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/subtests.py:85:24: error[not-iterable] Object of type `None` is not iterable
- Found 413 diagnostics
+ Found 412 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/boost.py:362:9: error[unresolved-attribute] Object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` has no attribute `append`
- boostedblob/boost.py:368:16: error[unresolved-attribute] Object of type `Collection[Awaitable[T@OrderedMappingBoostable]] & ~AlwaysFalsy` has no attribute `popleft`
- boostedblob/boost.py:382:19: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` with no `__getitem__` method
- boostedblob/boost.py:402:9: error[unresolved-attribute] Object of type `Collection[Awaitable[T@UnorderedMappingBoostable]]` has no attribute `add`
- boostedblob/boost.py:413:55: error[unresolved-attribute] Object of type `Awaitable[T@UnorderedMappingBoostable]` has no attribute `done`
- boostedblob/boost.py:416:9: error[unresolved-attribute] Object of type `Collection[Awaitable[T@UnorderedMappingBoostable]]` has no attribute `remove`
- Found 21 diagnostics
+ Found 15 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/database/base.py:56:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `__class__` on type `Self@init_db` with custom `__set__` method
+ alerta/models/alarms/__init__.py:34:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `__class__` on type `Self@init_app` with custom `__set__` method
- Found 620 diagnostics
+ Found 622 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/server.py:604:13: error[invalid-argument-type] Argument to bound method `find_missing_objects` is incorrect: Expected `((bytes, /) -> None) | None`, found `(bound method Self@handle.progress(message: bytes) -> None) | (def progress(self, message: bytes) -> None)`
+ dulwich/server.py:604:13: error[invalid-argument-type] Argument to bound method `find_missing_objects` is incorrect: Expected `((bytes, /) -> None) | None`, found `def progress(self, message: bytes) -> None`
- dulwich/server.py:649:21: error[invalid-argument-type] Argument to function `filter_pack_objects_with_paths` is incorrect: Expected `((bytes, /) -> None) | None`, found `(bound method Self@handle.progress(message: bytes) -> None) | (def progress(self, message: bytes) -> None)`
+ dulwich/server.py:649:21: error[invalid-argument-type] Argument to function `filter_pack_objects_with_paths` is incorrect: Expected `((bytes, /) -> None) | None`, found `def progress(self, message: bytes) -> None`

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/console/commands/build.py:242:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 977 diagnostics
+ Found 978 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- tests/Requester.py:216:9: warning[possibly-missing-attribute] Attribute `info` may be missing on object of type `Unknown | None | MagicMock`
- tests/Requester.py:263:9: warning[possibly-missing-attribute] Attribute `info` may be missing on object of type `Unknown | None | MagicMock`
- Found 299 diagnostics
+ Found 297 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
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
- Found 328 diagnostics
+ Found 318 diagnostics

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

psycopg (https://github.com/psycopg/psycopg)
- docs/lib/libpq_docs.py:99:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 652 diagnostics
+ Found 651 diagnostics

antidote (https://github.com/Finistere/antidote)
+ src/antidote/core/_raw/onion.py:417:9: error[invalid-assignment] Object of type `ReferenceType[Self@add_child]` is not assignable to attribute `__parent_ref` of type `Function[(), CatalogOnionLayerImpl | None]`
- src/antidote/core/_raw/wrapper.py:102:13: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `((...) -> object) | ((...) -> object)`
- src/antidote/core/_raw/wrapper.py:136:13: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `((...) -> Awaitable[object]) | ((...) -> Awaitable[object])`
+ src/antidote/core/_raw/wrapper.py:102:13: error[unresolved-attribute] Object of type `(...) -> object` has no attribute `__get__`
+ src/antidote/core/_raw/wrapper.py:136:13: error[unresolved-attribute] Object of type `(...) -> Awaitable[object]` has no attribute `__get__`
- Found 247 diagnostics
+ Found 248 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/core/smtlib/constraints.py:81:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `_parent` on type `Unknown | None | Self@__enter__`
+ manticore/core/smtlib/constraints.py:81:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `_parent` on type `Unknown | None | Self@__exit__`
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
+ Found 11059 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/tree.py:1133:33: error[invalid-argument-type] Argument to function `on_error` is incorrect: Argument type `Interaction[ClientT@CommandTree]` does not satisfy upper bound `CommandTree[ClientT@CommandTree]` of type variable `Self`
+ discord/app_commands/tree.py:1133:33: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Self@_dispatch_error`, found `Interaction[ClientT@CommandTree]`
- discord/app_commands/tree.py:1243:33: error[invalid-argument-type] Argument to function `on_error` is incorrect: Argument type `Interaction[ClientT@CommandTree]` does not satisfy upper bound `CommandTree[ClientT@CommandTree]` of type variable `Self`
+ discord/app_commands/tree.py:1243:33: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Self@_call_context_menu`, found `Interaction[ClientT@CommandTree]`
- discord/app_commands/tree.py:1301:33: error[invalid-argument-type] Argument to function `on_error` is incorrect: Argument type `Interaction[ClientT@CommandTree]` does not satisfy upper bound `CommandTree[ClientT@CommandTree]` of type variable `Self`
+ discord/app_commands/tree.py:1301:33: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Self@_call`, found `Interaction[ClientT@CommandTree]`
- discord/asset.py:462:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ConnectionState[Client] | _WebhookState`, found `Any | None | ConnectionState[Client] | _WebhookState`
- discord/asset.py:490:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ConnectionState[Client] | _WebhookState`, found `Any | None | ConnectionState[Client] | _WebhookState`
- discord/asset.py:525:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ConnectionState[Client] | _WebhookState`, found `Any | None | ConnectionState[Client] | _WebhookState`
- discord/emoji.py:131:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ConnectionState[Client]`, found `Any | None | ConnectionState[Client]`
- discord/emoji.py:186:16: warning[possibly-missing-attribute] Attribute `_get_guild` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/emoji.py:225:30: warning[possibly-missing-attribute] Attribute `application_id` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/emoji.py:229:19: warning[possibly-missing-attribute] Attribute `http` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/emoji.py:232:15: warning[possibly-missing-attribute] Attribute `http` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/emoji.py:282:30: warning[possibly-missing-attribute] Attribute `application_id` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/emoji.py:287:26: warning[possibly-missing-attribute] Attribute `http` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/emoji.py:292:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ConnectionState[Client]`, found `Any | None | ConnectionState[Client]`
- discord/emoji.py:294:22: warning[possibly-missing-attribute] Attribute `http` may be missing on object of type `Any | None | ConnectionState[Client]`
- discord/ext/commands/cog.py:288:36: error[invalid-type-arguments] Type `<special-form 'typing.Self'>` is not assignable to upper bound `Cog | None` of type variable `CogT@Command`
- discord/ext/commands/cog.py:289:79: error[invalid-type-arguments] Type `<special-form 'typing.Self'>` is not assignable to upper bound `Group | Cog` of type variable `GroupT@Command`
+ discord/ext/commands/cog.py:715:13: error[invalid-assignment] Invalid assignm

... (truncated 6020 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB


```

</details>




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
| `unresolved-attribute` | 3,548 | 60 | 18 |
| `invalid-argument-type` | 1,168 | 153 | 36 |
| `possibly-missing-attribute` | 156 | 159 | 92 |
| `invalid-return-type` | 164 | 43 | 38 |
| `invalid-assignment` | 49 | 51 | 29 |
| `unsupported-operator` | 3 | 39 | 6 |
| `call-non-callable` | 0 | 44 | 0 |
| `not-subscriptable` | 0 | 19 | 0 |
| `invalid-type-arguments` | 0 | 18 | 0 |
| `unused-ignore-comment` | 9 | 5 | 0 |
| `not-iterable` | 2 | 8 | 0 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `type-assertion-failure` | 0 | 6 | 0 |
| `no-matching-overload` | 0 | 5 | 0 |
| `invalid-raise` | 0 | 4 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `parameter-already-assigned` | 0 | 1 | 0 |
| **Total** | **5,101** | **615** | **226** |


**[Full report with detailed diff](https://0955eba3.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://0955eba3.ty-ecosystem-ext.pages.dev/timing))



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
