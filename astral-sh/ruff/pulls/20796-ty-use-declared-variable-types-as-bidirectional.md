```yaml
number: 20796
title: "[ty] Use declared variable types as bidirectional type context"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/declared-type-context
created_at: 2025-10-10T04:29:01Z
updated_at: 2025-10-16T19:40:40Z
url: https://github.com/astral-sh/ruff/pull/20796
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Use declared variable types as bidirectional type context

---

_Pull request opened by @ibraheemdev on 2025-10-10 04:29_

## Summary

Use the declared type of variables as type context for the RHS of assignment expressions, e.g.,
```py
x: list[int | str]
x = [1]
reveal_type(x)  # revealed: list[int | str]
```

---

_Review requested from @carljm by @ibraheemdev on 2025-10-10 04:29_

---

_Label `ty` added by @ibraheemdev on 2025-10-10 04:29_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-10 04:29_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-10 04:29_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-10 04:29_

---

_Comment by @github-actions[bot] on 2025-10-10 04:30_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-16 19:20:45.139893750 +0000
+++ new-output.txt	2025-10-16 19:20:48.626930815 +0000
@@ -883,6 +883,13 @@
 tuples_type_form.py:15:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""]]` is not assignable to `tuple[int, int]`
 tuples_type_form.py:25:1: error[invalid-assignment] Object of type `tuple[Literal[1]]` is not assignable to `tuple[()]`
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
+typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
+typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`
+typeddicts_operations.py:24:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
+typeddicts_operations.py:26:13: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
+typeddicts_operations.py:28:9: error[missing-typed-dict-key] Missing required key 'year' in TypedDict `Movie` constructor
+typeddicts_operations.py:29:42: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `Movie`: value of type `float`
+typeddicts_operations.py:32:36: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
@@ -892,6 +899,9 @@
 typeddicts_readonly.py:61:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie2`: key is marked read-only
 typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Cannot assign to key "name" on TypedDict `Album2`: key is marked read-only
 typeddicts_readonly_inheritance.py:65:19: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
+typeddicts_readonly_inheritance.py:82:14: error[invalid-assignment] Invalid assignment to key "ident" with declared type `str` on TypedDict `User`: value of type `Literal[3]`
+typeddicts_readonly_inheritance.py:83:15: error[invalid-argument-type] Invalid argument to key "ident" with declared type `str` on TypedDict `User`: value of type `Literal[3]`
+typeddicts_readonly_inheritance.py:84:5: error[missing-typed-dict-key] Missing required key 'ident' in TypedDict `User` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_type_consistency.py:126:56: error[invalid-argument-type] Invalid argument to key "inner_key" with declared type `str` on TypedDict `Inner1`: value of type `Literal[1]`
@@ -900,5 +910,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 902 diagnostics
+Found 912 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-10 04:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `str | None`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `str | None`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `int | None`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `int | None`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyright` is incorrect: Expected `str | None`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyright` is incorrect: Expected `str | None`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str | None`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str | None`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str | None`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str | None`, found `Any | str | None | int | Path`
- mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `Path | None`, found `Unknown | str | None | int | Path`
+ mypy_primer/main.py:70:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `Path | None`, found `Any | str | None | int | Path`

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:916:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Connection]`, found `Unknown | str | int | None | float`
+ aioredis/client.py:916:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Connection]`, found `Any | str | int | None | float`
- aioredis/client.py:916:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Unknown | str | int | None | float`
+ aioredis/client.py:916:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Any | str | int | None | float`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/python.py:1442:13: error[invalid-assignment] Object of type `dict[str, Literal["direct"]]` is not assignable to `dict[str, Literal["indirect", "direct"]]`
- Found 488 diagnostics
+ Found 487 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
+ jwt/api_jws.py:215:30: error[missing-typed-dict-key] Missing required key 'verify_signature' in TypedDict `SigOptions` constructor
- Found 33 diagnostics
+ Found 34 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/testclient.py:365:49: warning[possibly-missing-attribute] Attribute `read` on type `Unknown | int | list[Unknown] | BytesIO` may be missing
+ starlette/testclient.py:365:49: warning[possibly-missing-attribute] Attribute `read` on type `Any | int | list[Unknown] | BytesIO` may be missing

poetry (https://github.com/python-poetry/poetry)
- tests/puzzle/test_solver.py:614:9: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[Literal["a", "b", "download-package", "install-package"]]`
- Found 991 diagnostics
+ Found 990 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/data_kitchen.py:827:17: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `(dict[str, dict[str, DataFrame]] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ freqtrade/freqai/data_kitchen.py:827:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `dict[str, dict[str, DataFrame]]`
- freqtrade/freqai/data_kitchen.py:830:21: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown, Unknown]` on object of type `(dict[str, DataFrame] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ freqtrade/freqai/data_kitchen.py:830:21: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown, Unknown]` on object of type `dict[str, DataFrame]`
- freqtrade/freqai/data_kitchen.py:836:13: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `(dict[str, dict[str, DataFrame]] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ freqtrade/freqai/data_kitchen.py:836:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `dict[str, dict[str, DataFrame]]`
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'id' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'pair' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'lock_time' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'lock_timestamp' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'lock_end_time' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'lock_end_timestamp' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'reason' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'side' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2417:19: error[missing-typed-dict-key] Missing required key 'active' in TypedDict `RPCProtectionMsg` constructor
- Found 401 diagnostics
+ Found 410 diagnostics

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:420:5: error[invalid-assignment] Object of type `tuple[type, ...] | tuple[Unknown, ...]` is not assignable to `list[type] | None`
+ references/classification/utils.py:420:5: error[invalid-assignment] Object of type `tuple[type | Unknown, ...]` is not assignable to `list[type] | None`
- torchvision/prototype/transforms/_misc.py:56:13: error[invalid-assignment] Object of type `dict[Any, tuple[int, int]]` is not assignable to `tuple[int, int] | dict[type, tuple[int, int] | None]`
- torchvision/transforms/functional.py:1205:9: error[invalid-assignment] Object of type `list[Unknown | (list[int | float] & Number) | float]` is not assignable to `list[int | float]`
+ torchvision/transforms/functional.py:1205:9: error[invalid-assignment] Object of type `list[int | float | (list[int | float] & Number)]` is not assignable to `list[int | float]`
- torchvision/transforms/functional.py:1225:13: error[invalid-assignment] Object of type `list[Unknown | int | float]` is not assignable to `list[int] | None`
+ torchvision/transforms/functional.py:1225:13: error[invalid-assignment] Object of type `list[int | float]` is not assignable to `list[int] | None`
- torchvision/transforms/v2/functional/_geometry.py:649:9: error[invalid-assignment] Object of type `list[Unknown | (list[int | float] & Number) | float]` is not assignable to `list[int | float]`
+ torchvision/transforms/v2/functional/_geometry.py:649:9: error[invalid-assignment] Object of type `list[int | float | (list[int | float] & Number)]` is not assignable to `list[int | float]`
- Found 1481 diagnostics
+ Found 1480 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-docker/prefect_docker/images.py:64:5: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | None | bool | dict[str, Any]]` is not assignable to `dict[str, dict[str, Any]]`
+ src/integrations/prefect-docker/prefect_docker/images.py:64:5: error[invalid-assignment] Object of type `dict[str, dict[str, Any] | str | None | bool]` is not assignable to `dict[str, dict[str, Any]]`
+ src/prefect/flows.py:3093:28: warning[redundant-cast] Value is already of type `str`
+ src/prefect/flows.py:3106:28: warning[redundant-cast] Value is already of type `str`
- Found 3147 diagnostics
+ Found 3149 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/llmobs/_integrations/bedrock.py:304:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | list[Unknown] | str` may be missing
+ ddtrace/llmobs/_integrations/bedrock.py:304:21: warning[possibly-missing-attribute] Attribute `append` on type `Any | list[Unknown] | str` may be missing
- ddtrace/llmobs/_integrations/bedrock.py:314:25: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | list[Unknown] | str` may be missing
+ ddtrace/llmobs/_integrations/bedrock.py:314:25: warning[possibly-missing-attribute] Attribute `append` on type `Any | list[Unknown] | str` may be missing

zulip (https://github.com/zulip/zulip)
- zerver/openapi/python_examples.py:1338:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, str]`, found `tuple[Unknown, Unknown | str | list[Unknown | int]]`
+ zerver/openapi/python_examples.py:1338:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, str]`, found `tuple[Unknown, Any | str | list[Unknown | int]]`

sympy (https://github.com/sympy/sympy)
- sympy/core/evalf.py:1410:5: error[invalid-assignment] Object of type `dict[Unknown | <class 'Symbol'> | <class 'Dummy'> | ... omitted 31 union elements, Unknown | ((x: Expr, prec: int, options: dict[str, Any]) -> Any) | ((expr: Float, prec: int, options: dict[str, Any]) -> Any) | ... omitted 17 union elements]` is not assignable to `dict[type[Expr], (Expr, int, dict[str, Any], /) -> Any]`
+ sympy/core/evalf.py:1410:5: error[invalid-assignment] Object of type `dict[type[Expr], ((Expr, int, dict[str, Any], /) -> Any) | (def evalf_float(expr: Float, prec: int, options: dict[str, Any]) -> Any) | (def evalf_rational(expr: Rational, prec: int, options: dict[str, Any]) -> Any) | ... omitted 17 union elements]` is not assignable to `dict[type[Expr], (Expr, int, dict[str, Any], /) -> Any]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/globaldb/handler.py:587:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1713 diagnostics
+ Found 1712 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/recorder/statistics.py:1706:9: error[invalid-assignment] Object of type `set[Unknown | str]` is not assignable to `set[Literal["max", "mean", "min", "change"]] | None`
- homeassistant/components/recorder/statistics.py:1790:16: warning[possibly-missing-attribute] Attribute `isdisjoint` on type `set[Literal["max", "mean", "min", "change"]] | None` may be missing
- homeassistant/components/recorder/statistics.py:1801:17: error[invalid-argument-type] Argument to function `_get_max_mean_min_statistic` is incorrect: Expected `set[Literal["max", "mean", "min", "change"]]`, found `set[Literal["max", "mean", "min", "change"]] | None`
- homeassistant/components/recorder/statistics.py:1804:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["change"]` with `set[Literal["max", "mean", "min", "change"]] | None`
- Found 13760 diagnostics
+ Found 13756 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-10 06:50_

---

_Comment by @github-actions[bot] on 2025-10-10 06:56_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 9 | 1 | 12 |
| `invalid-assignment` | 0 | 4 | 9 |
| `missing-typed-dict-key` | 10 | 0 | 0 |
| `possibly-missing-attribute` | 0 | 1 | 3 |
| `redundant-cast` | 2 | 0 | 0 |
| `invalid-return-type` | 0 | 0 | 1 |
| `unsupported-operator` | 0 | 1 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **21** | **8** | **25** |

**[Full report with detailed diff](https://ibraheem-declared-type-conte.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-declared-type-conte.ecosystem-663.pages.dev/timing))


---

_Comment by @ibraheemdev on 2025-10-10 21:38_

This is more complicated for class attributes as may need to infer the RHS multiple times during attribute resolution. This is similar to what we do for function overloads, but the difference is that the RHS of an assignment statement is a standalone expression, so I'm not exactly sure how to approach this. We may be able to collect the possible type contexts beforehand and infer the intersection as the single type for the expression, but this needs more work.

I'm going to cut down this PR to just handle simple assignment statements, and implement class attributes in a followup PR.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-10 21:39_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-10 21:39_

---

_Comment by @carljm on 2025-10-10 23:03_

Took a look at some of the ecosystem hits here, just documenting what I found:

* aioredis is https://github.com/astral-sh/ty/issues/1248
* bandersnatch seems like it might be an issue with this PR? Even with `Unknown | None` as the type context, I don't see why we shouldn't narrow away the `None` from the conditional assignment
* similarly with cki-lib, it looks like this may be causing us to fail to narrow types in a problematic way

IIRC our approach to type context is generally just to union it in in the specialization builder? It seems like this approach may need some refinement in cases where the type context is a union, so we avoid unioning in any elements of the context union that are definitely not part of the type of the expression we are inferring?

---

_Comment by @carljm on 2025-10-10 23:11_

I guess the issue isn't really new to this PR, it already exists: https://play.ty.dev/6d7b5e71-848a-4091-90ea-b56f548a5eff

I think in that case it's going to be important that we can at least narrow away the `None`.

---

_Comment by @ibraheemdev on 2025-10-11 00:13_

https://github.com/astral-sh/ruff/pull/20528 adds a `filter_disjoint_elements` operation that we might be able to use here (though I'm not sure if will work as is for `T | None`).

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-11 00:27_

---

_Comment by @dcreager on 2025-10-14 19:35_

> I think in that case it's going to be important that we can at least narrow away the `None`.

Discussed this a bit with @ibraheemdev just now, and I think a rewording of this that I strongly agree with is that we should reveal the same types for `x` and `y` in: [[playground](https://play.ty.dev/c7c679db-2465-4cc9-8e04-0f7e25833214)]

```py
def id[T](x: T) -> T:
    return x

def _(i: int):
    x: int | None = id(i)
    y: int | None = i
    reveal_type(x)
    reveal_type(y)  # revealed: int
```

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-15 01:04_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-15 23:48_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-15 23:48_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-16 00:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:1357 on 2025-10-16 18:43_

nit: Most of the callers below don't look to be using the type context, and provide a closure that just immediately returns the previous `ty` parameter. You can reduce the churn on the diff by keeping `add_binding` with its existing signature, and calling this new variant e.g. `add_binding_with_type_context`.

```rust
fn add_binding(&mut self, node: AnyNodeRef, binding: Definition<'db>, ty: Type<'db>) {
    self.add_binding_with_context(node, binding, |_, _| ty);
}
```

---

_@dcreager reviewed on 2025-10-16 18:46_

Code looks good. Have you looked through the remaining ecosystem report results? (Also, does the ecosystem report show the diff between this PR and #20875, or `main`? If the latter we should want until that lands and get a fresh report to look at.)

---

_Comment by @ibraheemdev on 2025-10-16 18:50_

The diff is between this PR and https://github.com/astral-sh/ruff/pull/20875. The ecosystem report looks good, mostly positive changes and a couple regressions due to other known limitations (i.e. https://github.com/astral-sh/ty/issues/1248).

---

_@ibraheemdev reviewed on 2025-10-16 18:53_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:1357 on 2025-10-16 18:53_

This has come up a few times, and I think I would prefer to keep the signature as is for now, because I think it's useful to know when there is a potential type context available that is being ignored, and in fact there are a couple of cases where we probably could be using the type context, but I've held off for now.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-16 18:58_

---

_@dcreager approved on 2025-10-16 19:30_

---

_Merged by @ibraheemdev on 2025-10-16 19:40_

---

_Closed by @ibraheemdev on 2025-10-16 19:40_

---

_Branch deleted on 2025-10-16 19:40_

---
