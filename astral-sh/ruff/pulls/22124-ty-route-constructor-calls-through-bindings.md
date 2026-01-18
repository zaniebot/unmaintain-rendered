```yaml
number: 22124
title: "[ty] route constructor calls through bindings"
type: pull_request
state: open
author: Hugo-Polloli
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: constructor-bindings-refactor
created_at: 2025-12-21T03:30:02Z
updated_at: 2026-01-17T23:47:25Z
url: https://github.com/astral-sh/ruff/pull/22124
synced_at: 2026-01-18T00:18:13Z
```

# [ty] route constructor calls through bindings

---

_@Hugo-Polloli_

## Summary

fixes https://github.com/astral-sh/ty/issues/2124

* route constructor calls through the existing `Type::bindings` machinery via `constructor_bindings()`, ensuring constructor return types are instance-typed (not `__init__`'s `None`)
* delete the legacy constructor call path
* constructor overload resolution is deterministic (first matching overload), and implicit `__new__`/`__init__` lints remain method-specific and fire once per call site (including the missing-`__init__` fallback)

## Test Plan

* mdtest (constructor): added coverage for `@staticmethod __new__`, conflicting `__new__`/`__init__` parameter types, `__new__`-only fallback (object `__init__` accepts args), and generic constructor inference.
* mdtest (decorators): `functools.cached_property` specifically added to address https://github.com/astral-sh/ty/issues/1446
* mdtest (classes): fixed the TODOs for generic class params `C[Unknown]` are now correctly revealed as `C[int]` (and constructor specialization uses first matching overload)
* mdtest (functions): improved constructor path now correctly surfaces both the ill-typed generic call and the resulting invalid assignment


---

_Review requested from @carljm by @Hugo-Polloli on 2025-12-21 03:30_

---

_Review requested from @AlexWaygood by @Hugo-Polloli on 2025-12-21 03:30_

---

_Review requested from @sharkdp by @Hugo-Polloli on 2025-12-21 03:30_

---

_Review requested from @dcreager by @Hugo-Polloli on 2025-12-21 03:30_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 03:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 70.65% to 70.68%. The percentage of expected errors that received a diagnostic increased from 61.24% to 61.33%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 662 | 663 | +1 | ⏫ (✅) |
| False Positives | 275 | 275 | +0 |  |
| False Negatives | 419 | 418 | -1 | ⏬ (✅) |
| Total Diagnostics | 937 | 938 | +1 | ⏫ |
| Precision | 70.65% | 70.68% | +0.03% | ⏫ (✅) |
| Recall | 61.24% | 61.33% | +0.09% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_scoping.py:29:10](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_scoping.py#L29) | invalid-argument-type | Argument to bound method `meth_2` is incorrect: Expected `int`, found `Literal["a"]` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2025-12-21 03:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_make.py:2880:20: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2880:26: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2885:20: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2885:26: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2890:20: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ tests/test_make.py:2890:26: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- Found 628 diagnostics
+ Found 634 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/cmd/create.py:1084:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | str | list[str]`
+ lib/spack/spack/database.py:652:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `lib.spack.spack.llnl.util.lock.Lock`, found `ForbiddenLock | lib.spack.spack.util.lock.Lock`
+ lib/spack/spack/database.py:656:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `lib.spack.spack.llnl.util.lock.Lock`, found `ForbiddenLock | lib.spack.spack.util.lock.Lock`
- Found 4344 diagnostics
+ Found 4371 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/structures.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `dict[K@MultiDict, V@MultiDict] | dict[K@MultiDict, list[V@MultiDict]]`, found `dict[K@MultiDict, V@MultiDict | list[V@MultiDict]]`
+ tests/test_datastructures.py:582:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(str | None, /) -> Literal[-1]`, found `<class 'int'>`
+ tests/test_datastructures.py:583:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(str | None, /) -> Literal[-1]`, found `<class 'int'>`
- Found 407 diagnostics
+ Found 410 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch/tests/test_configuration.py:108:17: error[no-matching-overload] No overload of class `str` matches arguments
+ src/bandersnatch/tests/test_configuration.py:108:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `str | None`
- Found 78 diagnostics
+ Found 80 diagnostics

black (https://github.com/psf/black)
- src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- Found 53 diagnostics
+ Found 51 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/cli/cmds/logs.py:1228:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
- Found 1103 diagnostics
+ Found 1104 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_http_request.py:33:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
+ tests/test_http_request.py:37:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[123]`
+ tests/test_http_request.py:207:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def somecallback() -> Unknown`
+ tests/test_http_request.py:300:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def a_function() -> Unknown`
+ tests/test_http_request.py:307:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def a_function() -> Unknown`
+ tests/test_http_request.py:324:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal["a_function"]`
+ tests/test_http_request.py:329:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal["a_function"]`
+ tests/test_http_request.py:430:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `tuple[tuple[Literal["a"], Literal["one"]], tuple[Literal["a"], Literal["two"]], tuple[Literal["b"], Literal["2"]]]`
+ tests/test_http_request.py:432:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `tuple[tuple[Literal["a"], Literal["one"]], tuple[Literal["a"], Literal["two"]], tuple[Literal["b"], Literal["2"]]]`
+ tests/test_http_request.py:447:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | bytes, Unknown | bytes]`
+ tests/test_http_request.py:465:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | str | bytes, Unknown | bytes | str]`
+ tests/test_http_request.py:474:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | bytes, Unknown | bytes]`
+ tests/test_http_response.py:30:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
+ tests/test_http_response.py:35:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[b"http://example.com"]`
+ tests/test_http_response.py:37:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `dict[Unknown, Unknown]`
+ tests/test_http_response.py:72:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal["301"]`
+ tests/test_http_response.py:75:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal["lala200"]`
- Found 1789 diagnostics
+ Found 1806 diagnostics

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(int, /) -> list[int | float] | int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:351:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(int, /) -> list[int | float] | int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
+ tests/ignite/handlers/test_state_param_scheduler.py:358:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | list[Unknown | tuple[int, int]] | ... omitted 4 union elements`
- Found 2033 diagnostics
+ Found 2073 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ tests/test_pytypes.py:457:29: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | SupportsBytes | Buffer`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[SupportsIndex] | SupportsIndex | Buffer`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Buffer`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
- Found 216 diagnostics
+ Found 225 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/PaginatedList.py:372:101: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Requester.py:916:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Requester.py:924:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 299 diagnostics
+ Found 296 diagnostics

flake8 (https://github.com/pycqa/flake8)
+ src/flake8/__init__.py:64:15: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 37 diagnostics
+ Found 38 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_decorators.py:930:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:932:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:934:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:943:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:945:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:947:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:958:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:960:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_decorators.py:962:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1581 diagnostics
+ Found 1572 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

vision (https://github.com/pytorch/vision)
+ test/test_transforms_v2.py:2572:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[(...) -> Unknown]`, found `(x) -> Unknown`
+ test/test_transforms_v2.py:2572:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[(...) -> Unknown]`, found `(x) -> Unknown`
+ test/test_transforms_v2.py:2572:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[(...) -> Unknown]`, found `(x) -> Unknown`
- Found 1403 diagnostics
+ Found 1406 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/types/bigquery.py:231:13: error[invalid-argument-type] Argument is incorrect: Expected `Struct`, found `Type`
+ src/arti/types/pyarrow.py:241:13: error[invalid-argument-type] Argument is incorrect: Expected `Struct`, found `Type`
+ src/arti/types/python.py:158:83: error[invalid-argument-type] Argument is incorrect: Expected `frozenset[Any]`, found `tuple[Any, ...]`
- Found 145 diagnostics
+ Found 148 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/storages/_rdb/storage.py:351:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:351:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:365:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:365:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:367:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:367:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:374:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:374:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:376:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:376:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:384:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:384:48: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`
- optuna/storages/_rdb/storage.py:394:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[Unknown]`
+ optuna/storages/_rdb/storage.py:394:50: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | Column[str]`
- optuna/storages/_rdb/storage.py:396:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[Unknown], Any]`
+ optuna/storages/_rdb/storage.py:396:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown | Column[str], Any]`

comtypes (https://github.com/enthought/comtypes)
+ comtypes/test/test_util.py:28:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | float | bytes`
+ comtypes/test/test_util.py:28:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | int | float | bytes`
+ comtypes/test/test_util.py:28:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | bytes | bytearray`, found `Unknown | int | float | bytes`
- Found 412 diagnostics
+ Found 415 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/interactions.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionResponse[ClientT@Interaction]`, found `InteractionResponse[Client]`
- Found 542 diagnostics
+ Found 541 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/ninjabackend.py:654:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str] | None`, found `Unknown | _odict_values[str, CustomTarget | BuildTarget]`
+ mesonbuild/compilers/detect.py:1151:83: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | list[str]`, found `None | str | list[str]`
+ mesonbuild/interpreter/interpreter.py:3557:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3557:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3557:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3557:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[File | CustomTarget | CustomTargetIndex | ... omitted 4 union elements]`, found `list[File | str | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3558:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ExecutableKeywordArguments`, found `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3558:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `StaticLibraryKeywordArguments`, found `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- Found 2157 diagnostics
+ Found 2165 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:2871:22: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ValueError | ExceptionGroup[ValueError]`
- Found 487 diagnostics
+ Found 486 diagnostics

apprise (https://github.com/caronc/apprise)
+ apprise/plugins/email/base.py:562:22: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ apprise/plugins/email/base.py:563:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | int | tuple[Unknown | str] | None`
+ apprise/plugins/email/base.py:564:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | str | tuple[Unknown | str] | None`
+ apprise/plugins/fcm/__init__.py:266:13: error[unknown-argument] Argument `user_agent` does not match any known parameter of bound method `__init__`
+ apprise/plugins/fcm/__init__.py:267:13: error[unknown-argument] Argument `timeout` does not match any known parameter of bound method `__init__`
+ apprise/plugins/fcm/__init__.py:268:13: error[unknown-argument] Argument `verify_certificate` does not match any known parameter of bound method `__init__`
- Found 2648 diagnostics
+ Found 2654 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/cmd/devel/net_convert.py:194:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Unknown, Unknown] | None`, found `MutableMapping[str, Any] | None`
- Found 1169 diagnostics
+ Found 1170 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/worksearch/code.py:453:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[tuple[str, str, int]]]`, found `dict[Unknown, Unknown] | None`
+ openlibrary/plugins/worksearch/code.py:453:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[tuple[str, str, int]]]`, found `dict[str, list[tuple[str, str, int]]] | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/utilities/collections.py:484:36: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Any]`, found `list[Any | None]`
- Found 5411 diagnostics
+ Found 5407 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/backends/zarr.py:1091:25: error[invalid-assignment] Object of type `dict[str, bool | dict[Unknown | str, Unknown | bool]]` is not assignable to `dict[str, bool] | dict[str, dict[str, bool]]`
+ xarray/coding/times.py:451:53: error[no-matching-overload] No overload of function `__new__` matches arguments
+ xarray/core/resample_cftime.py:409:23: error[no-matching-overload] No overload of function `__new__` matches arguments
- xarray/tests/test_variable.py:2740:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1762 diagnostics
+ Found 1764 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/web_protocol.py:65:10: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aiohttp/web_protocol.py:66:10: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 181 diagnostics
+ Found 179 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/ensemble/_forest.py:348:21: error[missing-argument] No argument provided for required parameter `self` of function `__init__`
+ sklearn/ensemble/_forest.py:348:42: error[unknown-argument] Argument `criterion` does not match any known parameter of function `__init__`
+ sklearn/ensemble/_forest.py:700:21: error[missing-argument] No argument provided for required parameter `self` of function `__init__`
+ sklearn/ensemble/_forest.py:700:42: error[unknown-argument] Argument `criterion` does not match any known parameter of function `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_gb.py:2146:68: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:1869:46: error[unknown-argument] Argument `quantile` does not match any known parameter of bound method `__init__`
- Found 2423 diagnostics
+ Found 2443 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/layouts.py:298:20: error[invalid-assignment] Object of type `list[Sequence[Unknown]]` is not assignable to `list[UIElement | None] | list[list[UIElement | None]]`
+ src/bokeh/layouts.py:298:20: error[invalid-assignment] Object of type `list[UIElement | None | Sequence[Unknown]]` is not assignable to `list[UIElement | None] | list[list[UIElement | None]]`

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:365:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/backends/sql/datatypes.py:380:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | None`
+ ibis/expr/types/generic.py:2177:16: error[missing-argument] No argument provided for required parameter `quantile`
+ ibis/expr/types/generic.py:2177:16: error[missing-argument] No argument provided for required parameter `quantile`
+ ibis/expr/types/generic.py:2177:25: error[invalid-argument-type] Argument is incorrect: Expected `Value[Unknown, Columnar]`, found `int | float | NumericValue | Sequence[NumericValue | int | float]`
+ ibis/expr/types/generic.py:2177:25: error[invalid-argument-type] Argument is incorrect: Expected `Value[Unknown, Columnar]`, found `int | float | NumericValue | Sequence[NumericValue | int | float]`
+ ibis/expr/types/generic.py:2177:35: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Value[Boolean, Any] | None`, found `ibis.expr.types.generic.Value | None | @Todo`
+ ibis/expr/types/generic.py:2177:35: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Value[Boolean, Any] | None`, found `ibis.expr.types.generic.Value | None | @Todo`
+ ibis/expr/types/generic.py:2177:35: error[parameter-already-assigned] Multiple values provided for parameter `where`
+ ibis/expr/types/generic.py:2177:35: error[parameter-already-assigned] Multiple values provided for parameter `where`
+ ibis/expr/types/numeric.py:1465:16: error[missing-argument] No argument provided for required parameter `quantile`
+ ibis/expr/types/numeric.py:1465:16: error[missing-argument] No argument provided for required parameter `quantile`
+ ibis/expr/types/numeric.py:1465:25: error[invalid-argument-type] Argument is incorrect: Expected `Value[Numeric, Columnar]`, found `int | float | NumericValue | Sequence[NumericValue | int | float]`
+ ibis/expr/types/numeric.py:1465:25: error[invalid-argument-type] Argument is incorrect: Expected `Value[Numeric, Columnar]`, found `int | float | NumericValue | Sequence[NumericValue | int | float]`
+ ibis/expr/types/numeric.py:1465:35: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Value[Boolean, Any] | None`, found `ibis.expr.types.generic.Value | None | @Todo`
+ ibis/expr/types/numeric.py:1465:35: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Value[Boolean, Any] | None`

... (truncated 338 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-12-21 03:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Aconstructor-bindings-refactor?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 4.28%**

<sub>Comparing <code>Hugo-Polloli:constructor-bindings-refactor</code> (1d0e882) with <code>main</code> (704d30f)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Aconstructor-bindings-refactor?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 22 s | 21.1 s | +4.28% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Aconstructor-bindings-refactor?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Converted to draft by @Hugo-Polloli on 2025-12-21 03:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-21 08:54_

---

_Renamed from "ty: route constructor calls through bindings" to "[ty] route constructor calls through bindings" by @AlexWaygood on 2025-12-21 18:02_

---

_Label `ty` added by @AlexWaygood on 2025-12-21 18:02_

---

_Marked ready for review by @Hugo-Polloli on 2025-12-22 14:59_

---

_Comment by @Hugo-Polloli on 2025-12-22 15:53_

Hi all ! This should be ready for review, though I have a question concerning the test assertions I modified inside functions.md, the previous TODO mentioned no error but to my knowledge the comment I added is correct and the two errors surfaced should indeed appear, so I'd like your input on which it should be ! Thanks :)

---

_Comment by @carljm on 2025-12-23 02:17_

Wow, awesome, thank you for working on this!

It looks like the diagnostics that this PR removes on lines 81 and 82 of the conformance suite, in `constructors_call_type.py`, are supposed to be there, so that looks like a regression in this PR (and maybe some coverage we are missing in mdtests.)

The diagnostic added on line 29 of `generics_scoping.py` is correct, looks like a fix from this PR!

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-23 02:17_

---

_Review comment by @Hugo-Polloli on `crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md`:961 on 2025-12-23 12:04_

the previous TODO mentioned no error but to my knowledge the comment I added is correct and the two errors surfaced should indeed appear, so I'd like your input on which it should be, thanks !

---

_@Hugo-Polloli reviewed on 2025-12-23 12:04_

---

_Comment by @Hugo-Polloli on 2025-12-23 12:52_

> Wow, awesome, thank you for working on this!
> 
> It looks like the diagnostics that this PR removes on lines 81 and 82 of the conformance suite, in `constructors_call_type.py`, are supposed to be there, so that looks like a regression in this PR (and maybe some coverage we are missing in mdtests.)
> 
> The diagnostic added on line 29 of `generics_scoping.py` is correct, looks like a fix from this PR!

Indeed looks like a regression ! That was because I missed updating the TypeVar matching arms, so thanks for catching that, I fixed it to validate params against the bound class and added an mdtest (straight copied from typing conformance) to prevent future regression on that side

CI is currently red for reasons that look unrelated to this change: mypy_primer is failing while cloning pypa/twine (git clone … exit 128), and codspeed is panicking. I’ll rerun CI and dig in after Christmas if issues persist :)


---

_Comment by @sinon on 2025-12-23 13:14_

> CI is currently red for reasons that look unrelated to this change: mypy_primer is failing while cloning pypa/twine (git clone … exit 128), and codspeed is panicking. I’ll rerun CI and dig in after Christmas if issues persist :)

The CodSpeed panic is related to your change but not necessarily a blocker to the change. There is an assertion in each benchmarked library that the number of diagnostics are >0 but less than a per-library upperbound but your change means it is now above this bound. If the new diagnostics are not false-positives then you just increase the number in the relevant file (plus some headroom). The relevant setting is here: https://github.com/astral-sh/ruff/blob/5ea30c4c53894dc35fa5af2ca84757ec24f3f7d0/crates/ruff_benchmark/benches/ty_walltime.rs#L197

---

_Comment by @Hugo-Polloli on 2025-12-23 13:52_

> > CI is currently red for reasons that look unrelated to this change: mypy_primer is failing while cloning pypa/twine (git clone … exit 128), and codspeed is panicking. I’ll rerun CI and dig in after Christmas if issues persist :)
> 
> The CodSpeed panic is related to your change but not necessarily a blocker to the change. There is an assertion in each benchmarked library that the number of diagnostics are >0 but less than a per-library upperbound but your change means it is now above this bound. If the new diagnostics are not false-positives then you just increase the number in the relevant file (plus some headroom). The relevant setting is here:
> 
> https://github.com/astral-sh/ruff/blob/5ea30c4c53894dc35fa5af2ca84757ec24f3f7d0/crates/ruff_benchmark/benches/ty_walltime.rs#L197

Thanks for the explanation ! I found the way to run it locally, I'll check the diffs and whether they're regressions/bugs when I get back :)

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 19:56_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 396 | 1 | 10 |
| `unknown-argument` | 32 | 0 | 0 |
| `unused-ignore-comment` | 0 | 23 | 0 |
| `missing-argument` | 16 | 0 | 0 |
| `invalid-return-type` | 2 | 4 | 9 |
| `too-many-positional-arguments` | 15 | 0 | 0 |
| `invalid-assignment` | 1 | 1 | 6 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `no-matching-overload` | 5 | 0 | 0 |
| `parameter-already-assigned` | 4 | 0 | 0 |
| `type-assertion-failure` | 0 | 2 | 0 |
| **Total** | **471** | **31** | **32** |


**[Full report with detailed diff](https://5957b35e.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://5957b35e.ty-ecosystem-ext.pages.dev/timing))



---

_Converted to draft by @Hugo-Polloli on 2025-12-26 22:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 23:04_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @Hugo-Polloli on 2025-12-27 11:37_

So I checked the new sympy diagnostics, they come from strict unions of constructors and `type[Self]`. I added corresponding mdtests and TODOs, since some of these can currently be considered false positives, but properly fixing them would require additional modelling (runtime-config constructor aliases, subclass-widened constructors etc...)
I bumped the max diagnostics for sympy

---

_Marked ready for review by @Hugo-Polloli on 2025-12-27 12:37_

---

_Comment by @Hugo-Polloli on 2025-12-27 14:07_

The mypy_primer “duplicates” at `lib/spack/spack/cmd/create.py:1084:29` are from a call on a `Union[type[...]]` (24 variants). `report_diagnostics` currently emits one diagnostic per union element, and with `--output-format concise` the union subdiagnostics aren’t shown so they appear as identical repeated errors.

I found an existing TODO on `report_diagnostics` about aggregating union/overload failures into subdiagnostics, but implementing proper aggregation seems non-trivial and would probably be scope-creep for this PR.

I see two possibilities:
1. we accept the verbose/duplicated-looking output for union calls, follow up later with union-aware diagnostic aggregation (per the TODO).

2. some kind of minimal mitigation in `Bindings::report_diagnostics`, emit only one diagnostic for unions (first failing binding?) and add a subdiagnostic like “omitted N additional union variants”, quicker to do than proper aggregation


---

_Comment by @MichaReiser on 2025-12-29 09:17_

Thank you for working on this. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

---

_Comment by @Hugo-Polloli on 2025-12-29 21:33_

> Thank you for working on this. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

No problem at all, happy holidays! Also wishing the team very well deserved rest!

---

_Comment by @Hugo-Polloli on 2026-01-07 08:59_

Looks like the branch got stale and a conflict appeared, I’m going to rebase onto main to resolve the current conflicts and refresh CI ASAP (prob tonight if everything goes well), I’ll follow up with an update after #22377 lands to adapt this refactor to the new "class decorator calls go through constructor" path.

The open question about mypy_primer remains , if maintainers consider that "duplication" a regression (though from digging a bit in mdtests pertaining to unions it seems consistent with existing behavior?) I can prioritize fixing it before merge.

---

_Comment by @Hugo-Polloli on 2026-01-17 19:11_

I see #22377 has been merged, I will work on starting the rebase work soon :)

---

_Comment by @Hugo-Polloli on 2026-01-17 23:47_

> I see #22377 has been merged, I will work on starting the rebase work soon :)

Ended up being able to simplify @charliermarsh's `apply_decorator` to always use `try_call`, all mdtests still pass so behavior looks good ;)
This PR is officially ready for review!

---
