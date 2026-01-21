```yaml
number: 22756
title: "[ty] Fix false-positive `unsupported-operator` for \"symmetric\" TypeVars"
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
base: main
head: charlie/con
created_at: 2026-01-20T03:55:05Z
updated_at: 2026-01-21T03:32:35Z
url: https://github.com/astral-sh/ruff/pull/22756
synced_at: 2026-01-21T03:58:15Z
```

# [ty] Fix false-positive `unsupported-operator` for "symmetric" TypeVars

---

_@charliermarsh_

## Summary

When a TypeVar has value constraints (e.g., `TypeVar("Scalar", int, float)`), we were reporting `unsupported-operator` because the constrained TypeVars were being treated like unions -- we checked every possible combination, rather than understanding that both occurrences of the same TypeVar must have the same type.


---

_Label `bug` added by @charliermarsh on 2026-01-20 03:55_

---

_Label `ty` added by @charliermarsh on 2026-01-20 03:55_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.42% to 77.51%. The percentage of expected errors that received a diagnostic held steady at 60.49%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 672 | 672 | +0 |  |
| False Positives | 196 | 195 | -1 | ⏬ (✅) |
| False Negatives | 439 | 439 | +0 |  |
| Total Diagnostics | 868 | 867 | -1 | ⏬ |
| Precision | 77.42% | 77.51% | +0.09% | ⏫ (✅) |
| Recall | 60.49% | 60.49% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_basic.py:34:12](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_basic.py#L34) | unsupported-operator | Operator `+` is not supported between two objects of type `AnyStr@concat` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/capture.py:948:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:949:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:964:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:965:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:968:30: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture | Unknown` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/_pytest/capture.py:968:30: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- src/_pytest/capture.py:968:44: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture | Unknown` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/_pytest/capture.py:968:44: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 407 diagnostics
+ Found 403 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between two objects of type `Num@MultipleOf`
- Found 405 diagnostics
+ Found 404 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5407 diagnostics
+ Found 5412 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/polys/domains/gaussiandomains.py:97:25: error[unsupported-operator] Operator `+` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:97:37: error[unsupported-operator] Operator `+` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:106:25: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:106:37: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:113:25: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:113:37: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:25: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:36: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:46: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:57: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- Found 15653 diagnostics
+ Found 15643 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4455 diagnostics
+ Found 4453 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2058 diagnostics
+ Found 2059 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:41:43: error[unsupported-operator] Operator `-` is not supported between two objects of type `_R@ignore_variance`
- Found 14465 diagnostics
+ Found 14464 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @carljm by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-20 03:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:12293 on 2026-01-20 15:00_

ummmm is this correct? `+` and `-` operations don't necessarily return the same type; what about this?

```py
class Foo:
    def __add__(self, other: Foo) -> int:
        return 42

def bar[T: (Foo, str)](x: T):
    reveal_type(x + x)
```

your branch reveals `T@Bar` there but I think that's only correct if every constraint has an `__add__` method that returns `Self`

---

_@AlexWaygood reviewed on 2026-01-20 15:02_

---

_@charliermarsh reviewed on 2026-01-20 15:10_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:12293 on 2026-01-20 15:10_

Mmm yes you're right!

---

_Converted to draft by @charliermarsh on 2026-01-20 15:24_

---

_Marked ready for review by @charliermarsh on 2026-01-20 15:37_

---

_Converted to draft by @charliermarsh on 2026-01-20 16:59_

---

_Marked ready for review by @charliermarsh on 2026-01-20 19:16_

---

_Converted to draft by @charliermarsh on 2026-01-20 19:16_

---

_Comment by @charliermarsh on 2026-01-20 19:39_

(I think this isn't quite right, revisiting.)

---

_Marked ready for review by @charliermarsh on 2026-01-21 02:16_

---

_Renamed from "[ty] Fix false-positive unsupported-operator for constrained TypeVars" to "[ty] Fix false-positive `unsupported-operator` for symmetric TypeVars" by @charliermarsh on 2026-01-21 02:19_

---

_Renamed from "[ty] Fix false-positive `unsupported-operator` for symmetric TypeVars" to "[ty] Fix false-positive `unsupported-operator` for "symmetric" TypeVars" by @charliermarsh on 2026-01-21 02:34_

---

_Comment by @charliermarsh on 2026-01-21 03:32_

I think this fix makes sense, but I'm not certain if there's a more "general" fix or whether this should be happening at a deeper level.

---
