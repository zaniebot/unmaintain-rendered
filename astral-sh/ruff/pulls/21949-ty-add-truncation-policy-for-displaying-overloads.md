```yaml
number: 21949
title: "[ty] Add truncation policy for displaying overloads on single line"
type: pull_request
state: open
author: dhruvmanila
labels:
  - ty
assignees: []
draft: true
base: dhruv/paramspec-overload-1
head: dhruv/overload-truncation-policy
created_at: 2025-12-12T14:49:15Z
updated_at: 2025-12-12T16:36:05Z
url: https://github.com/astral-sh/ruff/pull/21949
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Add truncation policy for displaying overloads on single line

---

_@dhruvmanila_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-12-12 14:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-12 14:56:51.725135747 +0000
+++ new-output.txt	2025-12-12 14:56:55.361146571 +0000
@@ -260,7 +260,7 @@
 constructors_callable.py:126:1: error[type-assertion-failure] Type `Class6Proxy` does not match asserted type `Unknown`
 constructors_callable.py:127:4: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 constructors_callable.py:142:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:162:5: info[revealed-type] Revealed type: `Overload[(x: int) -> Unknown, (x: str) -> Unknown]`
+constructors_callable.py:162:5: info[revealed-type] Revealed type: `Overload[(x: int) -> Unknown, (x: str) -> Unknown, ... omitted 0 overloads]`
 constructors_callable.py:164:1: error[type-assertion-failure] Type `Class7[int]` does not match asserted type `Unknown`
 constructors_callable.py:165:1: error[type-assertion-failure] Type `Class7[str]` does not match asserted type `Unknown`
 constructors_callable.py:182:13: info[revealed-type] Revealed type: `(x: list[Unknown], y: list[Unknown]) -> Unknown`
@@ -770,7 +770,7 @@
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:152:26: error[invalid-argument-type] Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
-overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
+overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes, ... omitted 0 overloads]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
 overloads_definitions.py:33:5: error[invalid-overload] Overloads for function `func2` must be followed by a non-`@overload`-decorated implementation function
 overloads_definitions.py:63:9: error[invalid-overload] Overloads for function `not_abstract` must be followed by a non-`@overload`-decorated implementation function

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-12 14:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None, ... omitted 0 overloads]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`

paasta (https://github.com/yelp/paasta)
- paasta_tools/contrib/bounce_log_latency_parser.py:48:54: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:48:54: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
- paasta_tools/contrib/bounce_log_latency_parser.py:49:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:49:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
- paasta_tools/contrib/bounce_log_latency_parser.py:50:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:50:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
- paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
+ paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["labels"]` on object of type `str`

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None, ... omitted 0 overloads]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rtcpeerconnection.py:1237:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> MediaDescription, (s: slice[Any, Any, Any], /) -> list[MediaDescription]]` cannot be called with key of type `int | None` on object of type `list[MediaDescription]`
+ src/aiortc/rtcpeerconnection.py:1237:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> MediaDescription, (s: slice[Any, Any, Any], /) -> list[MediaDescription], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[MediaDescription]`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_feedexport.py:1777:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal["format"]` on object of type `bytes`
+ tests/test_feedexport.py:1777:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> bytes, ... omitted 0 overloads]` cannot be called with key of type `Literal["format"]` on object of type `bytes`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/data_io.py:506:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown]]` cannot be called with key of type `int | None` on object of type `list[int | Unknown]`
+ sockeye/data_io.py:506:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[int | Unknown]`
- sockeye/data_io.py:508:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:508:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
- sockeye/data_io.py:513:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:513:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
- sockeye/data_io.py:517:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:517:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
- sockeye/data_io.py:519:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:519:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
- sockeye/data_io.py:522:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown]]` cannot be called with key of type `int | None` on object of type `list[int | Unknown]`
+ sockeye/data_io.py:522:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown], ... omitted 0 overloads]` cannot be called with key of type `int | None` on object of type `list[int | Unknown]`
- test/common.py:272:35: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["translation"]` on object of type `str`
+ test/common.py:272:35: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["translation"]` on object of type `str`
- test/integration/test_seq_copy_int.py:242:23: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["translation"]` on object of type `str`
+ test/integration/test_seq_copy_int.py:242:23: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str, ... omitted 0 overloads]` cannot be called with key of type `Literal["translation"]` on object of type `str`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
+ tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
+ tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
+ tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
+ tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
+ tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
+ tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
+ tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
+ tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
+ tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
+ tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str], ... omitted 0 overloads]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any],

... (truncated 811 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-12 14:56_

---

_Comment by @dhruvmanila on 2025-12-12 16:36_

It's still pretty long, maybe we should avoid displaying the signature when an overload is involved?

---
