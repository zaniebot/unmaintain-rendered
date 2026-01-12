```yaml
number: 20517
title: "[ty] Use `typing.Self` for the first parameter of instance methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-self-signature
created_at: 2025-09-22T18:13:05Z
updated_at: 2025-09-29T19:08:09Z
url: https://github.com/astral-sh/ruff/pull/20517
synced_at: 2026-01-12T15:57:03Z
```

# [ty] Use `typing.Self` for the first parameter of instance methods

---

_@sharkdp_

## Summary

Modify the (external) signature of instance methods such that the first parameter uses `Self` unless it is explicitly annotated. This allows us to correctly type-check more code, and allows us to infer correct return types for many functions that return `Self`. For example:

```py
from pathlib import Path
from datetime import datetime, timedelta

reveal_type(Path(".config") / ".ty")  # now Path, previously Unknown

def _(dt: datetime, delta: timedelta):
    reveal_type(dt - delta)  # now datetime, previously Unknown
```

part of https://github.com/astral-sh/ty/issues/159

Co-authored by: @Glyphack 

## Performance

I ran benchmarks locally on `attrs`, `freqtrade` and `colour`, the projects with the largest regressions on CodSpeed. I see much smaller effects locally, but can definitely reproduce the regression on `attrs`. From looking at the profiling results (on Codspeed), it seems that we simply do more type inference work, which seems plausible, given that we now understand much more return types (of many stdlib functions). In particular, whenever a function uses an implicit `self` and returns `Self` (without mentioning `Self` anywhere else in its signature), we will now infer the correct type, whereas we would previously return `Unknown`. This also means that we need to invoke the generics solver in more cases. Comparing half a million lines of log output on attrs, I can see that we do 5% more "work" (number of lines in the log), and have a lot more `apply_specialization` events (7108 vs 4304). On freqtrade, I see similar numbers for `apply_specialization` (11360 vs 5138 calls). Given these results, I'm not sure if it's generally worth doing more performance work, especially since none of the code modifications themselves seem to be likely candidates for regressions.

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./ty_main check /home/shark/ecosystem/attrs` | 92.6 ± 3.6 | 85.9 | 102.6 | 1.00 |
| `./ty_self check /home/shark/ecosystem/attrs` | 101.7 ± 3.5 | 96.9 | 113.8 | 1.10 ± 0.06 |

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./ty_main check /home/shark/ecosystem/freqtrade` | 599.0 ± 20.2 | 568.2 | 627.5 | 1.00 |
| `./ty_self check /home/shark/ecosystem/freqtrade` | 607.9 ± 11.5 | 594.9 | 626.4 | 1.01 ± 0.04 |

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./ty_main check /home/shark/ecosystem/colour` | 423.9 ± 17.9 | 394.6 | 447.4 | 1.00 |
| `./ty_self check /home/shark/ecosystem/colour` | 426.9 ± 24.9 | 373.8 | 456.6 | 1.01 ± 0.07 |

## Test Plan

New Markdown tests

## Ecosystem report

* apprise: ~300 new diagnostics related to problematic stubs in apprise :weary: 
* attrs: a new true positive, since [this function](https://github.com/python-attrs/attrs/blob/4e2c89c82398914dbbe24fe00b03f00a8b8efe05/tests/test_make.py#L2135) is missing a `@staticmethod`? 
* Some legitimate true positives
* sympy: lots of new `invalid-operator` false positives in [matrix multiplication](https://github.com/sympy/sympy/blob/cf9f4b680520821b31e2baceb4245252939306be/sympy/matrices/matrixbase.py#L3267-L3269) due to our limited understanding of [generic `Callable[[Callable[[T1, T2], T3]], Callable[[T1, T2], T3]]` "identity" types](https://github.com/sympy/sympy/blob/cf9f4b680520821b31e2baceb4245252939306be/sympy/core/decorators.py#L83-L84) of decorators. This is not related to type-of-self.

## Typing conformance results

The changes are all correct, except for
```diff
+generics_self_usage.py:50:5: error[invalid-assignment] Object of type `def foo(self) -> int` is not assignable to `(typing.Self, /) -> int`
```
which is related to an assignability problem involving type variables on both sides:
```py
class CallableAttribute:
    def foo(self) -> int:
        return 0

    bar: Callable[[Self], int] = foo  # <- we currently error on this assignment
```

---

_Label `ty` added by @sharkdp on 2025-09-22 18:13_

---

_Comment by @github-actions[bot] on 2025-09-22 18:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-29 18:52:45.668425107 +0000
+++ new-output.txt	2025-09-29 18:52:49.013452786 +0000
@@ -420,17 +420,12 @@
 generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
-generics_self_basic.py:51:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
-generics_self_basic.py:52:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
 generics_self_basic.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
 generics_self_basic.py:55:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
 generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@set_value`
 generics_self_basic.py:67:26: error[invalid-type-form] Special form `typing.Self` expected no type parameter
-generics_self_basic.py:74:5: error[type-assertion-failure] Argument does not have asserted type `Container[int]`
-generics_self_basic.py:75:5: error[type-assertion-failure] Argument does not have asserted type `Container[str]`
-generics_self_basic.py:83:5: error[type-assertion-failure] Argument does not have asserted type `Container[T@object_with_generic_type]`
-generics_self_protocols.py:48:5: error[type-assertion-failure] Argument does not have asserted type `ShapeProtocol`
 generics_self_protocols.py:61:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `BadReturnType`
+generics_self_usage.py:50:5: error[invalid-assignment] Object of type `def foo(self) -> int` is not assignable to `(typing.Self, /) -> int`
 generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:76:6: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
@@ -860,5 +855,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 861 diagnostics
+Found 856 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-09-22 18:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-self-signature?runnerMode=WallTime)

### Merging #20517 will **degrade performances by 14.5%**

<sub>Comparing <code>david/implicit-self-signature</code> (3ce98ce) with <code>main</code> (1d3e4a9)</sub>



### Summary

`❌ 8 (👁 8)` regressions  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` large[sympy] `` | 40.2 s | 42.4 s | -5.2% |
| 👁 | `` medium[colour-science] `` | 9.6 s | 10.5 s | -8.43% |
| 👁 | `` medium[pandas] `` | 30.8 s | 33.8 s | -8.85% |
| 👁 | `` medium[static-frame] `` | 8.3 s | 8.9 s | -6.53% |
| 👁 | `` small[altair] `` | 2.5 s | 2.6 s | -4.61% |
| 👁 | `` small[freqtrade] `` | 7.8 s | 9.2 s | -14.5% |
| 👁 | `` small[pydantic] `` | 2.3 s | 2.5 s | -7.06% |
| 👁 | `` small[tanjun] `` | 1.6 s | 1.7 s | -4.9% |


---

_Comment by @codspeed-hq[bot] on 2025-09-22 18:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-self-signature?runnerMode=Instrumentation)

### Merging #20517 will **degrade performances by 9.78%**

<sub>Comparing <code>david/implicit-self-signature</code> (3ce98ce) with <code>main</code> (1d3e4a9)</sub>



### Summary

`❌ 4 (👁 4)` regressions  
`✅ 39` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_check_file[incremental] `` | 4.9 ms | 5.1 ms | -4.96% |
| 👁 | `` attrs `` | 389.6 ms | 431.8 ms | -9.78% |
| 👁 | `` DateType `` | 215.4 ms | 235.7 ms | -8.62% |
| 👁 | `` hydra-zen `` | 780.1 ms | 841.7 ms | -7.31% |


---

_Comment by @github-actions[bot] on 2025-09-22 18:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @github-actions[bot] on 2025-09-22 18:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/_make.py:1581:5: error[invalid-assignment] Object of type `tuple[Unknown, ...]` is not assignable to `list[Attribute | Unknown]`
+ src/attr/_make.py:1581:5: error[invalid-assignment] Object of type `tuple[@Todo, ...]` is not assignable to `list[Attribute | Unknown]`
+ tests/test_make.py:2174:57: error[invalid-argument-type] Argument to function `_get_copy_kwargs` is incorrect: Expected `TestClassBuilder`, found `Literal[False]`
- Found 550 diagnostics
+ Found 551 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/cmd/ci.py:716:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- lib/spack/spack/config.py:485:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/install_test.py:517:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/install_test.py:519:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/oci/oci.py:48:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ lib/spack/spack/oci/opener.py:288:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[b""]`
- lib/spack/spack/solver/asp.py:3148:26: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `tuple[Unknown, ...]`
+ lib/spack/spack/solver/asp.py:3148:26: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `tuple[@Todo, ...]`
- lib/spack/spack/solver/asp.py:3149:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `tuple[Unknown, ...]`
+ lib/spack/spack/solver/asp.py:3149:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `tuple[@Todo, ...]`
+ lib/spack/spack/test/cmd/logs.py:38:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `TextIOWrapper[_WrappedBuffer]` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ lib/spack/spack/test/cmd/logs.py:38:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `TextIOWrapper[_WrappedBuffer]`
- lib/spack/spack/vendor/archspec/cpu/schema.py:78:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[Path, Path]`, found `tuple[Unknown, None | Unknown]`
+ lib/spack/spack/vendor/archspec/cpu/schema.py:78:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[Path, Path]`, found `tuple[Unknown | Path, None | Path]`
+ lib/spack/spack/vendor/markupsafe/__init__.py:248:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method _ListOrDict@_escape_argspec.__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Any` and a value of type `Markup` on object of type `_ListOrDict@_escape_argspec`
- Found 7516 diagnostics
+ Found 7519 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/settings.py:620:32: error[invalid-argument-type] Argument to function `fnmatch` is incorrect: Argument type `bytes | str` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ isort/settings.py:620:32: error[invalid-argument-type] Argument to function `fnmatch` is incorrect: Expected `str`, found `bytes | str`
+ isort/settings.py:620:69: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["/"]` and `bytes | str`
- Found 35 diagnostics
+ Found 38 diagnostics

asynq (https://github.com/quora/asynq)
+ asynq/decorators.py:282:39: error[invalid-argument-type] Argument to function `__get__` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `DecoratorBase[_T@DecoratorBase]` of type variable `Self`
- Found 139 diagnostics
+ Found 140 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ src/aiortc/rtcpeerconnection.py:825:62: warning[possibly-missing-attribute] Attribute `role` on type `RTCDtlsParameters | None` may be missing
+ src/aiortc/rtcpeerconnection.py:827:53: warning[possibly-missing-attribute] Attribute `role` on type `RTCDtlsParameters | None` may be missing
+ src/aiortc/rtcpeerconnection.py:913:47: error[invalid-argument-type] Argument to function `reverse_direction` is incorrect: Expected `str`, found `str | None`
+ src/aiortc/rtcpeerconnection.py:968:64: warning[possibly-missing-attribute] Attribute `iceLite` on type `RTCIceParameters | None` may be missing
+ src/aiortc/rtcpeerconnection.py:972:52: warning[possibly-missing-attribute] Attribute `role` on type `RTCDtlsParameters | None` may be missing
+ src/aiortc/rtcpeerconnection.py:976:42: warning[possibly-missing-attribute] Attribute `role` on type `RTCDtlsParameters | None` may be missing
- Found 94 diagnostics
+ Found 100 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/unicode.py:175:39: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
+ pylint/checkers/unicode.py:175:39: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `bytes`, found `_StrLike@_map_positions_to_result`
+ pylint/checkers/unicode.py:181:15: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
+ pylint/checkers/unicode.py:181:15: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `bytes`, found `_StrLike@_map_positions_to_result`
+ pylint/checkers/unicode.py:188:19: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `str`, found `_StrLike@_map_positions_to_result`
+ pylint/checkers/unicode.py:188:19: error[invalid-argument-type] Argument to bound method `find` is incorrect: Expected `bytes`, found `_StrLike@_map_positions_to_result`
- Found 187 diagnostics
+ Found 193 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/status.py:2335:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 912 diagnostics
+ Found 911 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/assertion/rewrite.py:1023:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 488 diagnostics
+ Found 487 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/http/headers.py:45:30: error[invalid-argument-type] Argument to bound method `title` is incorrect: Expected `bytes`, found `AnyStr@normkey`
+ scrapy/http/headers.py:45:30: error[no-matching-overload] No overload of bound method `title` matches arguments
+ scrapy/utils/datatypes.py:76:16: error[invalid-argument-type] Argument to bound method `lower` is incorrect: Expected `bytes`, found `AnyStr@normkey`
- scrapy/utils/datatypes.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@normkey`, found `str | bytes`
+ scrapy/utils/datatypes.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@normkey`, found `Unknown | bytes`
+ scrapy/utils/datatypes.py:76:16: error[no-matching-overload] No overload of bound method `lower` matches arguments
- Found 1059 diagnostics
+ Found 1063 diagnostics

ignite (https://github.com/pytorch/ignite)
+ examples/cifar10/main.py:100:45: error[invalid-argument-type] Argument to function `setup_tb_logging` is incorrect: Expected `str`, found `Unknown | Path`
+ examples/cifar10_qat/main.py:96:45: error[invalid-argument-type] Argument to function `setup_tb_logging` is incorrect: Expected `str`, found `Unknown | Path`
+ examples/transformers/main.py:103:13: error[invalid-argument-type] Argument to function `setup_tb_logging` is incorrect: Expected `str`, found `Unknown | Path`
+ tests/ignite/handlers/test_clearml_logger.py:43:35: error[invalid-argument-type] Argument to function `__call__` is incorrect: Expected `ClearMLSaver`, found `None`
- Found 2163 diagnostics
+ Found 2167 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/utilities/value_from_ast.py:137:46: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode & ~AlwaysFalsy & ~VariableNode & ~NullValueNode`
+ src/graphql/utilities/value_from_ast.py:139:46: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode & ~AlwaysFalsy & ~VariableNode & ~NullValueNode`
+ src/graphql/validation/rules/values_of_correct_type.py:162:48: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_definition.py:157:37: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_definition.py:159:34: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_definition.py:163:34: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_scalars.py:60:49: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_scalars.py:222:51: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_scalars.py:348:52: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_scalars.py:471:53: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
+ tests/type/test_scalars.py:616:48: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode`
- Found 412 diagnostics
+ Found 423 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/data_io.py:176:74: error[unsupported-operator] Operator `/` is unsupported between objects of type `int` and `int | float | None`
+ sockeye/data_io.py:193:27: error[unsupported-operator] Operator `*` is unsupported between objects of type `int | Unknown` and `int | float | None`
- sockeye/scoring.py:190:48: warning[division-by-zero] Cannot divide object of type `Literal[1]` by zero
- Found 315 diagnostics
+ Found 316 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/contrib/diffstat.py:169:27: warning[division-by-zero] Cannot divide object of type `float` by zero
- dulwich/contrib/diffstat.py:170:27: warning[division-by-zero] Cannot divide object of type `float` by zero
- Found 180 diagnostics
+ Found 178 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/models/link.py:182:11: error[unsupported-operator] Operator `+` is unsupported between objects of type `str` and `Literal[b""]`
- Found 470 diagnostics
+ Found 471 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/vcs/git/backend.py:578:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ tests/integration/test_utils_vcs_git.py:432:43: error[unresolved-attribute] Type `Literal[b""]` has no attribute `encode`
- Found 985 diagnostics
+ Found 987 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/misc.py:283:18: error[unresolved-attribute] Type `str & ~AlwaysFalsy` has no attribute `decode`
+ cki_lib/misc.py:284:18: error[unresolved-attribute] Type `str & ~AlwaysFalsy` has no attribute `decode`
- Found 181 diagnostics
+ Found 183 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/test_connectionpool.py:464:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:653:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:654:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:684:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:685:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:720:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:721:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_ssltransport.py:447:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_ssltransport.py:451:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:76:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:315:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:339:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:358:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:376:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_connectionpool.py:1083:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:345:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:460:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:461:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:814:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:816:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:909:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_https.py:911:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/with_dummyserver/test_proxy_poolmanager.py:175:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 388 diagnostics
+ Found 365 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/test_tstring.py:238:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_tstring.py:239:25: warning[possibly-missing-attribute] Attribute `query` on type `PostgresQuery | None` may be missing
+ tests/test_tstring.py:240:29: warning[possibly-missing-attribute] Attribute `query` on type `PostgresQuery | None` may be missing
- Found 698 diagnostics
+ Found 701 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/mitmproxywrapper.py:164:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo, Unknown, /) -> BaseRequestHandler`, found `None`
+ examples/contrib/mitmproxywrapper.py:164:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo, TCPServer, /) -> BaseRequestHandler`, found `None`
+ mitmproxy/proxy/layers/modes.py:29:45: error[invalid-argument-type] Argument to function `handle_event` is incorrect: Expected `NextLayer`, found `Event`
+ mitmproxy/proxy/layers/modes.py:37:45: error[invalid-argument-type] Argument to function `handle_event` is incorrect: Expected `NextLayer`, found `Event`
+ test/mitmproxy/proxy/test_layer.py:170:24: error[invalid-argument-type] Argument to function `handle_event` is incorrect: Expected `NextLayer`, found `DataReceived`
- Found 1838 diagnostics
+ Found 1841 diagnostics

mypy (https://github.com/python/mypy)
+ mypyc/ir/pprint.py:422:17: error[unresolved-attribute] Type `Op` has no attribute `label`
- Found 1854 diagnostics
+ Found 1855 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/generic.py:236:21: error[not-iterable] Object of type `ListOrTupleOrSetAny@UniqueItems` may not be iterable
+ koda_validate/generic.py:290:16: error[invalid-argument-type] Argument to bound method `startswith` is incorrect: Expected `str`, found `StrOrBytes@StartsWith`
+ koda_validate/generic.py:290:16: error[invalid-argument-type] Argument to bound method `startswith` is incorrect: Expected `bytes`, found `StrOrBytes@StartsWith`
+ koda_validate/generic.py:298:16: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `StrOrBytes@EndsWith`
+ koda_validate/generic.py:298:16: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `bytes`, found `StrOrBytes@EndsWith`
+ koda_validate/generic.py:312:20: error[invalid-argument-type] Argument to bound method `strip` is incorrect: Expected `bytes`, found `StrOrBytes@NotBlank`
+ koda_validate/generic.py:312:20: error[no-matching-overload] No overload of bound method `strip` matches arguments
+ koda_validate/generic.py:321:16: error[invalid-argument-type] Argument to bound method `strip` is incorrect: Expected `bytes`, found `StrOrBytes@Strip`
- koda_validate/generic.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@Strip`, found `str | bytes`
+ koda_validate/generic.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@Strip`, found `Unknown | bytes`
+ koda_validate/generic.py:321:16: error[no-matching-overload] No overload of bound method `strip` matches arguments
+ koda_validate/generic.py:330:16: error[invalid-argument-type] Argument to bound method `upper` is incorrect: Expected `bytes`, found `StrOrBytes@UpperCase`
- koda_validate/generic.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@UpperCase`, found `str | bytes`
+ koda_validate/generic.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@UpperCase`, found `Unknown | bytes`
+ koda_validate/generic.py:330:16: error[no-matching-overload] No overload of bound method `upper` matches arguments
+ koda_validate/generic.py:336:16: error[invalid-argument-type] Argument to bound method `lower` is incorrect: Expected `bytes`, found `StrOrBytes@LowerCase`
- koda_validate/generic.py:336:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@LowerCase`, found `str | bytes`
+ koda_validate/generic.py:336:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@LowerCase`, found `Unknown | bytes`
+ koda_validate/generic.py:336:16: error[no-matching-overload] No overload of bound method `lower` matches arguments
- Found 56 diagnostics
+ Found 69 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/tasks/__init__.py:100:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/webhook/sync.py:181:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 513 diagnostics
+ Found 511 diagnostics

vision (https://github.com/pytorch/vision)
+ test/test_datasets.py:3118:9: error[invalid-assignment] Object of type `Path` is not assignable to `str`
+ test/test_datasets.py:3128:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `Literal["_camera_settings.json"]`
+ test/test_datasets.py:3190:9: error[invalid-assignment] Object of type `Path` is not assignable to `str`
+ test/test_datasets.py:3194:65: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `str`
+ test/test_datasets.py:3272:9: error[invalid-assignment] Object of type `Path` is not assignable to `str`
+ test/test_image.py:844:16: error[invalid-argument-type] Argument to function `write_file` is incorrect: Expected `str`, found `Path`
+ test/test_image.py:845:21: error[invalid-argument-type] Argument to function `write_jpeg` is incorrect: Expected `str`, found `Path`
+ test/test_image.py:846:20: error[invalid-argument-type] Argument to function `write_png` is incorrect: Expected `str`, found `Path`
- Found 1474 diagnostics
+ Found 1482 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/builders/linkcheck.py:758:20: error[invalid-return-type] Return type does not match returned value: expected `str | None`, found `Literal[b""]`
+ sphinx/ext/graphviz.py:268:32: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `str`, found `Literal[b""]`
- sphinx/writers/text.py:448:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, list[str]]`, found `tuple[Unknown, list[str] | list[LiteralString] | Unknown]`
+ sphinx/writers/text.py:448:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, list[str]]`, found `tuple[Unknown, list[str] | list[LiteralString]]`
- Found 519 diagnostics
+ Found 521 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/_gp/search_space.py:193:56: error[invalid-argument-type] Argument to bound method `to_external_repr` is incorrect: Expected `int | float`, found `ndarray[Unknown, Unknown]`
- Found 564 diagnostics
+ Found 565 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- src/tests/test_static_typing.py:56:20: error[invalid-return-type] Return type does not match returned value: expected `T@f`, found `int | float | str`
+ src/tests/test_static_typing.py:56:20: error[unsupported-operator] Operator `*` is unsupported between objects of type `T@f` and `Literal[2]`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceAzure.py:2120:17: warning[possibly-missing-attribute] Attribute `append` on type `Any | bool | dict[Unknown | str, Unknown] | list[Unknown]` may be missing
+ cloudinit/sources/DataSourceAzure.py:2120:17: warning[possibly-missing-attribute] Attribute `append` on type `Any | bool | dict[Unknown | str, Unknown | int] | list[Unknown]` may be missing
- tests/unittests/test_ds_identify.py:986:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str` may be missing
+ tests/unittests/test_ds_identify.py:986:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str | int]` may be missing
- tests/unittests/test_ds_identify.py:1482:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str` may be missing
+ tests/unittests/test_ds_identify.py:1482:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | str | int]` may be missing
+ tests/unittests/test_net.py:1474:22: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `str`, found `Unknown | bool | str | None`
- Found 834 diagnostics
+ Found 835 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/build.py:2766:16: error[invalid-return-type] Return type does not match returned value: expected `list[str | File | BuildTarget | CustomTarget | ExternalProgram]`, found `list[str | File | BuildTarget | CustomTarget] | Unknown`
+ mesonbuild/build.py:2766:16: error[invalid-return-type] Return type does not match returned value: expected `list[str | File | BuildTarget | CustomTarget | ExternalProgram]`, found `list[str | File | BuildTarget | CustomTarget]`
+ mesonbuild/cmake/fileapi.py:195:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown, Unknown]]`
+ mesonbuild/cmake/fileapi.py:200:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown, Unknown]]`
+ mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
+ mesonbuild/cmake/fileapi.py:223:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/cmake/interpreter.py:1154:52: error[invalid-argument-type] Argument to function `resolve_ctgt_ref` is incorrect: Expected `CustomTargetReference`, found `CustomTargetReference | None | Unknown`
+ mesonbuild/cmake/interpreter.py:1154:52: error[invalid-argument-type] Argument to function `resolve_ctgt_ref` is incorrect: Expected `CustomTargetReference`, found `CustomTargetReference | None`
+ mesonbuild/compilers/mixins/gnu.py:348:12: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[str]`
+ mesonbuild/interpreter/interpreter.py:1843:24: error[invalid-key] TypedDict `Executable` cannot be indexed with a key of type `Unknown | str`
+ mesonbuild/modules/simd.py:105:24: error[invalid-key] Cannot access `StaticLibrary` with a key of type `str`. Only string literals are allowed as keys on TypedDicts.
+ mesonbuild/wrap/wrap.py:788:21: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- Found 884 diagnostics
+ Found 892 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:524:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 207 diagnostics
+ Found 208 diagnostics

manticore (https://github.com/trailofbits/manticore)
+ examples/script/aarch64/hello42.py:44:9: error[invalid-argument-type] Argument to function `subscribe` is incorrect: Expected `ManticoreBase`, found `Literal["will_execute_instruction"]`
+ examples/script/aarch64/hello42.py:48:9: error[invalid-argument-type] Argument to function `subscribe` is incorrect: Expected `ManticoreBase`, found `Literal["did_execute_instruction"]`
+ examples/script/basic_statemerging.py:27:17: error[invalid-argument-type] Argument to function `subscribe` is incorrect: Expected `ManticoreBase`, found `Literal["will_load_state"]`
+ examples/script/basic_statemerging.py:28:17: error[invalid-argument-type] Argument to function `subscribe` is incorrect: Expected `ManticoreBase`, found `Literal["did_load_state"]`
- Found 1082 diagnostics
+ Found 1086 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/core/output/sanitization.py:54:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 293 diagnostics
+ Found 294 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/data/btanalysis/historic_precision.py:27:12: error[invalid-return-type] Return type does not match returned value: expected `Series[Any]`, found `int | float`
+ freqtrade/data/btanalysis/trade_parallelism.py:44:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[int]`
+ freqtrade/data/converter/trade_converter.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | Unknown`
+ freqtrade/data/converter/trade_converter_kraken.py:82:39: error[invalid-argument-type] Argument to function `trades_convert_types` is incorrect: Expected `DataFrame`, found `Series[Any]`
+ freqtrade/data/dataprovider.py:225:9: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `Timestamp`
- freqtrade/data/entryexitanalysis.py:319:35: error[invalid-argument-type] Argument to function `print_df_rich_table` is incorrect: Expected `Sequence[str]`, found `Unknown | Index[Any]`
+ freqtrade/data/entryexitanalysis.py:319:35: error[invalid-argument-type] Argument to function `print_df_rich_table` is incorrect: Expected `Sequence[str]`, found `Index[Any]`
+ freqtrade/data/history/history_utils.py:287:19: error[invalid-argument-type] Argument to function `dt_ts` is incorrect: Expected `datetime | None`, found `Series[Any]`
+ freqtrade/data/history/history_utils.py:529:51: error[invalid-argument-type] Argument to function `format_ms_time_det` is incorrect: Expected `int | float`, found `Series[Any]`
+ freqtrade/data/history/history_utils.py:560:9: error[invalid-argument-type] Argument to bound method `get_historic_trades` is incorrect: Expected `str | None`, found `Series[Any] | None`
+ freqtrade/data/metrics.py:243:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:244:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:249:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:250:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any]`
+ freqtrade/data/metrics.py:251:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any]`
+ freqtrade/data/metrics.py:252:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any]`
+ freqtrade/freqai/data_drawer.py:324:51: error[unknown-argument] Argument `columns` does not match any known parameter of bound method `reindex`
- freqtrade/freqai/data_kitchen.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame, DataFrame]`, found `tuple[Unknown, Unknown | list[Unknown]]`
+ freqtrade/freqai/data_kitchen.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame, DataFrame]`, found `tuple[DataFrame | Series[Any], Series[Any] | list[Unknown]]`
+ freqtrade/freqai/data_kitchen.py:836:13: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `(dict[str, dict[str, DataFrame]] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ freqtrade/optimize/optimize_reports/optimize_reports.py:424:31: error[unresolved-attribute] Type `Hashable` has no attribute `date`
+ freqtrade/templates/strategy_analysis_example.ipynb:cell 23:13:5: error[invalid-argument-type] Argument to function `generate_candlestick_graph` is incorrect: Expected `DataFrame`, found `Series[Any]`
- Found 388 diagnostics
+ Found 406 diagnostics

xarray (https://github.com/pydata/xarray)
- properties/test_pandas_roundtrip.py:104:41: error[invalid-argument-type] Argument to function `assert_series_equal` is incorrect: Expected `Series[Any]`, found `Unknown | Series[Any] | DataFrame`
+ properties/test_pandas_roundtrip.py:104:41: error[invalid-argument-type] Argument to function `assert_series_equal` is incorrect: Expected `Series[Any]`, found `DataArray | Series[Any] | DataFrame`
- properties/test_pandas_roundtrip.py:122:39: error[invalid-argument-type] Argument to function `assert_frame_equal` is incorrect: Expected `DataFrame`, found `Unknown | Series[Any] | DataFrame`
+ properties/test_pandas_roundtrip.py:122:39: error[invalid-argument-type] Argument to function `assert_frame_equal` is incorrect: Expected `DataFrame`, found `DataArray | Series[Any] | DataFrame`
+ xarray/computation/rolling.py:1218:20: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@Coarsen`, found `Dataset`
+ xarray/core/coordinates.py:1196:69: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Hashable` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/coordinates.py:1199:51: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Hashable` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/dataarray.py:517:13: error[invalid-assignment] Object of type `Unknown | Default` is not assignable to attribute `attrs` on type `Variable | Unknown`
+ xarray/core/groupby.py:686:25: error[invalid-argument-type] Argument to bound method `transpose` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
+ xarray/core/groupby.py:686:25: error[invalid-argument-type] Argument to bound method `transpose` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/parallel.py:670:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/plot/facetgrid.py:172:43: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~None` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:173:43: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~None` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:184:24: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:185:24: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:200:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:231:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:232:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:236:44: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/tests/test_dask.py:1789:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_dask.py:1790:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_dask.py:1791:86: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_dask.py:1792:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_dataarray.py:2317:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_dataset.py:3977:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_distributed.py:274:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1622 diagnostics
+ Found 1628 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_run.py:2884:43: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `BaseException`
- src/trio/_core/_tests/test_run.py:524:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_highlevel_socket.py:159:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_util.py:171:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_util.py:172:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/path.py:16:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:17:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:18:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:19:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:53:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:55:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:58:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:59:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:60:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- Found 668 diagnostics
+ Found 656 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/main.py:286:21: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["type"]` and a value of type `Unknown` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | (Unknown & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:297:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["type"]` and a value of type `Unknown` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | (Unknown & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:305:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["type"]` and a value of type `Unknown` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | (Unknown & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:311:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["items"]` and a value of type `Unknown` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | (Unknown & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
+ cwltool/main.py:319:17: error[invalid-assignment] Method `__setitem__` of type `(bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None) | (bound method MutableMapping[str, @Todo | None].__setitem__(key: str, value: @Todo | None, /) -> None)` cannot be called with a key of type `Literal["fields"]` and a value of type `Unknown` on object of type `(str & Top[MutableMapping[Unknown, Unknown]]) | (Unknown & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, @Todo | None]`
- tests/test_provenance.py:759:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 150 diagnostics
+ Found 154 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pandas/container.py:105:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/backends/pandas/container.py:751:12: error[unresolved-attribute] Type `Index[str]` has no attribute `any`
- pandera/backends/pandas/error_formatters.py:115:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:80:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `Series[Any]`
- tests/pandas/test_decorators.py:858:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_model.py:1394:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_model.py:1409:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pandas/test_schemas.py:1907:16: warning[possibly-missing-attribute] Attribute `name` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2291:12: warning[possibly-missing-attribute] Attribute `unique` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2292:12: warning[possibly-missing-attribute] Attribute `title` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2293:12: warning[possibly-missing-attribute] Attribute `description` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2294:12: warning[possibly-missing-attribute] Attribute `metadata` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2414:12: warning[possibly-missing-attribute] Attribute `named_indexes` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2415:12: warning[possibly-missing-attribute] Attribute `named_indexes` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2416:12: warning[possibly-missing-attribute] Attribute `named_indexes` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2418:12: warning[possibly-missing-attribute] Attribute `named_indexes` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2419:12: warning[possibly-missing-attribute] Attribute `named_indexes` on type `Unknown | None` may be missing
+ tests/pandas/test_schemas.py:2420:12: warning[possibly-missing-attribute] Attribute `named_indexes` on type `Unknown | None` may be missing
- Found 1585 diagnostics
+ Found 1594 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_utils.py:367:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: Literal["cls_name"], value: str, /) -> None, (key: Literal["namespace"], value: dict[str, Any] | None, /) -> None, (key: Literal["bases"], value: tuple[type[@Todo], ...], /) -> None, (key: Literal["init"], value: bool, /) -> None, (key: Literal["repr"], value: bool, /) -> None, (key: Literal["eq"], value: bool, /) -> None, (key: Literal["order"], value: bool, /) -> None, (key: Literal["unsafe_hash"], value: bool, /) -> None, (key: Literal["frozen"], value: bool, /) -> None, (key: Literal["match_args"], value: bool, /) -> None, (key: Literal["kw_only"], value: bool, /) -> None, (key: Literal["slots"], value: bool, /) -> None, (key: Literal["weakref_slot"], value: bool, /) -> None, (key: Literal["module"], value: str | None, /) -> None, (key: Literal["target"], value: str, /) -> None, (key: Literal["target_repr"], value: bool, /) -> None])` cannot be called with a key of type `str` and a value of type `Any` on object of type `Unknown | DataclassOptions`
- Found 568 diagnostics
+ Found 569 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/core/lending.py:797:22: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown | str, Unknown | str | ~AlwaysTruthy | dict[Unknown | str, Unknown] | int]`
+ openlibrary/coverstore/utils.py:92:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, dict[str, str]]`, found `tuple[Literal[b""], dict[@Todo, @Todo]]`
- openlibrary/plugins/worksearch/code.py:315:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, @Todo | str] | tuple[str, Unknown]] | Unknown`
+ openlibrary/plugins/worksearch/code.py:315:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, @Todo | str] | tuple[str, Unknown]]`
- openlibrary/utils/bulkimport.py:78:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- openlibrary/utils/bulkimport.py:189:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/commands/context.py:1017:21: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `str | None`
+ pwndbg/commands/context.py:1041:33: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `BitFlags`
- Found 2533 diagnostics
+ Found 2535 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/feature_selection/_univariate_selection.py:109:35: warning[possibly-missing-attribute] Attribute `size` on type `int | float | Unknown` may be missing
+ sklearn/feature_selection/_univariate_selection.py:109:35: warning[possibly-missing-attribute] Attribute `size` on type `int | float | @Todo` may be missing
+ sklearn/preprocessing/_data.py:3422:31: error[no-matching-overload] No overload matches arguments
+ sklearn/preprocessing/_data.py:3455:27: error[no-matching-overload] No overload matches arguments
+ sklearn/preprocessing/_data.py:3506:31: error[no-matching-overload] No overload matches arguments
- Found 1986 diagnostics
+ Found 1989 diagnostics

apprise (https://github.com/caronc/apprise)
+ tests/test_api.py:871:31: error[invalid-argument-type] Argument to function `schemas` is incorrect: Expected `URLBase`, found `<class 'TextNotification'>`
+ tests/test_api.py:881:31: error[invalid-argument-type] Argument to function `schemas` is incorrect: Expected `URLBase`, found `<class 'TextNotification'>`
+ tests/test_api.py:888:31: error[invalid-argument-type] Argument to function `schemas` is incorrect: Expected `URLBase`, found `<class 'HtmlNotification'>`
+ tests/test_api.py:898:35: error[invalid-argument-type] Argument to function `schemas` is incorrect: Expected `URLBase`, found `object`
+ tests/test_rest_plugins.py:655:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:656:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:666:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:672:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:675:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:688:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:689:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:692:30: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:693:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:697:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:700:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:738:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:739:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:747:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:753:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:756:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:792:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:793:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:797:51: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:799:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:806:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:807:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:813:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:818:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:830:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:831:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:841:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:842:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:848:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:853:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:888:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:889:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:897:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:905:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:906:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:912:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:917:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1029:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1030:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1040:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1041:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1045:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1050:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1088:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1089:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1098:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1106:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1107:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1111:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1116:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1260:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1261:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1271:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1277:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1280:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1293:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1294:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1297:30: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:1298:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:1302:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1305:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1343:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1344:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1352:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1358:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1361:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1400:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1401:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1405:51: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:1407:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_rest_plugins.py:1414:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1415:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1421:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1426:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1438:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1439:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1449:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1450:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1456:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1461:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1502:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1503:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1511:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1519:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1520:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1526:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1531:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1541:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1542:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1550:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1558:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1559:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1565:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1570:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1584:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1585:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1588:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1596:45: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1597:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1601:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1606:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1724:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1725:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1735:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1736:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1740:31: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1745:23: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1790:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1791:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1800:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_plugins.py:1808:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `str | None`
+ tests/test_rest_p...*[Comment body truncated]*

---

_@Glyphack reviewed on 2025-09-22 20:36_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:316 on 2025-09-22 20:36_

This should be supertype instead of subtype?

This is what pyright says:
https://pyright-play.net/?pyrightVersion=1.1.405&code=MYGwhgzhAECCBcBYAUNN0AmBTAZtARmBgBQRYg7zQBEAwtQJTQC0AfNAHID2AdlkqnRCAdKJQpQkGLWKwGVUcKA


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-23 09:55_

---

_Comment by @github-actions[bot] on 2025-09-23 10:00_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 15 | 567 | 0 |
| `invalid-argument-type` | 546 | 0 | 14 |
| `unsupported-operator` | 170 | 0 | 8 |
| `unresolved-attribute` | 96 | 0 | 0 |
| `unused-ignore-comment` | 0 | 71 | 0 |
| `possibly-missing-attribute` | 58 | 0 | 12 |
| `invalid-assignment` | 33 | 0 | 6 |
| `non-subscriptable` | 32 | 0 | 0 |
| `invalid-return-type` | 19 | 4 | 7 |
| `no-matching-overload` | 30 | 0 | 0 |
| `not-iterable` | 9 | 0 | 0 |
| `call-non-callable` | 5 | 2 | 0 |
| `division-by-zero` | 0 | 3 | 0 |
| `invalid-key` | 2 | 0 | 0 |
| `possibly-missing-implicit-call` | 0 | 0 | 2 |
| `too-many-positional-arguments` | 2 | 0 | 0 |
| `invalid-context-manager` | 1 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| `parameter-already-assigned` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `unknown-argument` | 1 | 0 | 0 |
| **Total** | **1,022** | **647** | **49** |

**[Full report with detailed diff](https://david-implicit-self-signatur.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-self-signatur.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-23 12:50_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-23 12:50_

---

_@sharkdp reviewed on 2025-09-23 12:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:328 on 2025-09-23 12:57_

This regression is tracked in https://github.com/astral-sh/ty/issues/1157

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:280 on 2025-09-23 13:04_

Hm. I looks like the https://github.com/astral-sh/ruff/pull/20328 fix doesn't work here?

---

_@sharkdp reviewed on 2025-09-23 13:04_

---

_@sharkdp reviewed on 2025-09-23 13:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:316 on 2025-09-23 13:15_

Hmm, in a generic class `C[T]`, `self` may be also be annotated with e.g. `C[int]` (see last example in https://typing.python.org/en/latest/spec/generics.html#use-in-generic-classes). I'm not sure I understand pyright's error message here?

---

_@sharkdp reviewed on 2025-09-23 14:40_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:316 on 2025-09-23 14:40_

I talked with @AlexWaygood and it seems to us that we can hardly put any restrictions on the annotated type of `self`, so it may be fine to remove this TODO for now. Exceptions might be `self: B` annotation on a class `A`, where `B` is disjoint from `A`. So we could also update the TODO comment to mention this example.
```suggestion
    # TODO: Potentially emit a warning if the annotated type of `self` is disjoint from the method's class
```

---

_@sharkdp reviewed on 2025-09-23 14:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:280 on 2025-09-23 14:42_

This also seems to cause new diagnostics in the ecosystem, so this should be fixed.

---

_@sharkdp reviewed on 2025-09-23 14:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:280 on 2025-09-23 14:45_

Oh, we still use the `Unknown`-specialization in `determine_upper_bound`

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-24 11:32_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 11:32_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-24 12:02_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 12:02_

---

_@sharkdp reviewed on 2025-09-24 12:02_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:280 on 2025-09-24 12:02_

No, the problem was that we eagerly "baked" `NamedTupleFallback` into the signature with the new "no generics here" trick. Fixed now.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-24 12:42_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 12:42_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-24 12:50_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 12:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:145 on 2025-09-24 15:06_

This should be fixed. Implicit `self` only works for positional-or-keyword parameters, at the moment.

---

_@sharkdp reviewed on 2025-09-24 15:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:156 on 2025-09-24 15:07_

This should also be fixed. I have not looked into it yet. Maybe it can be deferred to a follow-up.

---

_@sharkdp reviewed on 2025-09-24 15:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:1284 on 2025-09-24 15:28_

it looks like this was supposed to be
```suggestion
                                context
                                    .variables(db)
                                    .iter()
                                    .any(|v| v.typevar(db).kind(db) == TypeVarKind::TypingSelf)
```

---

_@sharkdp reviewed on 2025-09-24 15:28_

---

_@sharkdp reviewed on 2025-09-24 16:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:145 on 2025-09-24 16:07_

Fixed.

---

_@sharkdp reviewed on 2025-09-24 16:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:1284 on 2025-09-24 16:07_

Fixed.

---

_@sharkdp reviewed on 2025-09-24 16:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:1239 on 2025-09-24 16:09_

Will talk to @AlexWaygood about this. I assume that we break assignability/subtyping for protocol since we create `Self` with a bound that refers to the protocol class itself (instead of doing something that is more appropriate for structural types?).

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-24 16:09_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 16:09_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-25 11:44_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-25 11:44_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-25 12:14_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-25 12:14_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-29 11:28_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-29 11:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:4276 on 2025-09-29 11:43_

I am also fine with moving this down and making it a catch-all case (so we don't have to update it every time we add a `KnownClass`). Seems unlikely that new fallback classes are (ever) added. On the other hand, *if* we ever add another fallback class, it would be important to get this right. And not getting it right can lead to subtle and confusing errors.

---

_@sharkdp reviewed on 2025-09-29 11:43_

---

_Marked ready for review by @sharkdp on 2025-09-29 12:39_

---

_Review requested from @carljm by @sharkdp on 2025-09-29 12:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-29 12:39_

---

_Review requested from @dcreager by @sharkdp on 2025-09-29 12:39_

---

_Review requested from @MichaReiser by @sharkdp on 2025-09-29 12:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-29 13:19_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-29 13:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:93 on 2025-09-29 13:56_

But we also verify the type of the first argument if `self` is passed implicitly -- for example (and I'm very happy to see that we emit a diagnostic here!):

```py
from typing import Never

class Foo:
    def f(self: Never) -> None: ...

# error[invalid-argument-type]: Argument to bound method `f` is incorrect: Expected `Never`, found `Foo`
Foo().f()
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:102 on 2025-09-29 13:59_

```suggestion
If the method is a class or static method then first argument is not inferred as `Self`:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:112 on 2025-09-29 13:59_

nit


```suggestion
A.a_classmethod()
A.a_classmethod(a)  # error: [too-many-positional-arguments]
A.a_staticmethod(1)
a.a_staticmethod(1)
A.a_staticmethod(a)  # error: [invalid-argument-type]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:153 on 2025-09-29 14:00_

```suggestion
reveal_type(B().name_does_not_matter())  # revealed: B
reveal_type(B().positional_only(1))  # revealed: B
reveal_type(B().keyword_only(x=1))  # revealed: B
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:358 on 2025-09-29 14:01_

`int` is not disjoint from `Explicit` -- you could maybe have another method with `self` annotated as `bool` (which _is_ disjoint), and put the TODO next to that method instead?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:364 on 2025-09-29 14:06_

I think it comes to the same thing from a soundness perspective, but a more user-friendly diagnostic might be to say

```suggestion
# error: [invalid-argument-type] "Argument to bound method `bad` is incorrect: Expected `int & Explicit`, found `Explicit`"
```

to make it clearer to the user that the only valid way to call the method from an instance would be on an instance some class that is _both_ a subclass of `Explicit` and a subclass of `int`

I don't think you need to change anything here now, though you could also add a TODO -- I'll leave it up to you :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4276 on 2025-09-29 14:14_

I think this is fine; it's not too burdensome to have to update a few `match`es when we add `KnownClass`es (which anyway isn't very common). And we've had errors elsewhere in the past due to not having exhaustive matches, and then forgetting to update things when we add new variants

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:1234 on 2025-09-29 14:16_

hmm, what do we end up displaying if the parameter is unnamed and `self.param.should_annotation_be_displayed()` returns `false`? That seems like it could produce some interesting results 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:112 on 2025-09-29 14:27_

(if we merge https://github.com/astral-sh/ruff/pull/20630 then this call won't be necessary, since you're only passing it to `bind_typevar()` down below)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:113 on 2025-09-29 14:30_

if we're looking for speedups: one micro-optimisation (that probably won't make a difference, but might be worth trying?) might be to initialise this as a `const` variable at compile time and then clone it here rather than calling `Name::new_static("Self")` every time

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1258 on 2025-09-29 14:35_

nit: could we add some SAFETY comments here about why the `.unwrap()`s are okay? (Or switch to using `.expect()`? Obviously even better would be if we could avoid the `.unwrap()`s entirely ;) )

---

_@AlexWaygood approved on 2025-09-29 14:38_

Thank you, this looks great. I haven't analysed the ecosystem report at all, but I trust your analysis if you say it looks good!

---

_Comment by @AlexWaygood on 2025-09-29 14:39_

And thank you so much @Glyphack as well!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:93 on 2025-09-29 18:23_

Added this test:
```py
from typing import Never

class Strange:
    def can_not_be_called(self: Never) -> None: ...

# error: [invalid-argument-type] "Argument to bound method `can_not_be_called` is incorrect: Expected `Never`, found `Strange`"
Strange().can_not_be_called()
```

---

_@sharkdp reviewed on 2025-09-29 18:23_

---

_@sharkdp reviewed on 2025-09-29 18:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:364 on 2025-09-29 18:27_

Decided to leave everything as is for now. Not sure if the intersection type in the error message really helps.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/display.rs`:1234 on 2025-09-29 18:43_

None of our mdtests currently hit the (implicit) `else` branch here, at the moment. I removed the check for now. I thought about adding `debug_assert!(self.param.should_annotation_be_displayed())`, but it seems too annoying even for a debug build to crash because of that.

---

_@sharkdp reviewed on 2025-09-29 18:43_

---

_@sharkdp reviewed on 2025-09-29 18:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:113 on 2025-09-29 18:50_

We use `Name::new_static` in lots of places. I somehow can't imagine that this will help with performance, but we can try that separately, I guess.

---

_Comment by @sharkdp on 2025-09-29 19:07_

The performance regressions look severe, but I am going to merge this. We can always work on performance later, and it's probably also important to keep in mind that across the *whole* ecosystem, this is more of a [3% slowdown](https://david-implicit-self-signatur.ecosystem-663.pages.dev/timing).

---

_Merged by @sharkdp on 2025-09-29 19:08_

---

_Closed by @sharkdp on 2025-09-29 19:08_

---

_Branch deleted on 2025-09-29 19:08_

---
