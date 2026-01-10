```yaml
number: 22241
title: "[ty] don't expand type aliases via type mappings unless necessary"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dont-expand-alias-by-type-mapping
created_at: 2025-12-29T11:10:08Z
updated_at: 2025-12-30T03:03:31Z
url: https://github.com/astral-sh/ruff/pull/22241
synced_at: 2026-01-10T16:36:18Z
```

# [ty] don't expand type aliases via type mappings unless necessary

---

_Pull request opened by @mtshiba on 2025-12-29 11:10_

## Summary

`apply_type_mapping` always expands type aliases and operates on the resulting types, which can lead to cluttered results due to excessive type alias expansion in places where it is not actually needed.

Specifically, type aliases are expanded when displaying method signatures, because we use `TypeMapping::BindSelf` to get the method signature.

```python
type Scalar = int | float
type Array1d = list[Scalar] | tuple[Scalar]

def f(x: Scalar | Array1d) -> None: pass
reveal_type(f)  # revealed: def f(x: Scalar | Array1d) -> None

class Foo:
    def f(self, x: Scalar | Array1d) -> None: pass
# should be `bound method Foo.f(x: Scalar | Array1d) -> None`
reveal_type(Foo().f)  # revealed: bound method Foo.f(x: int | float | list[int | float] | tuple[int | float]) -> None
```

In this PR, when type mapping is performed on a type alias, the expansion result without type mapping is compared with the expansion result after type mapping, and if the two are equivalent, the expansion is deemed redundant and canceled.

## Test Plan

mdtest updated



---

_Label `ty` added by @mtshiba on 2025-12-29 11:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 11:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 11:13_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
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
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5548 diagnostics
+ Found 5543 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/cluster/test_vq.pyi:40:1: error[type-assertion-failure] Type `tuple[Unknown, floating[Any]]` does not match asserted type `Unknown`
+ tests/cluster/test_vq.pyi:40:1: error[type-assertion-failure] Type `tuple[Unknown, floating]` does not match asserted type `Unknown`
- tests/sparse/linalg/test__interface.pyi:17:1: error[type-assertion-failure] Type `_CustomLinearOperator[signedinteger[_16Bit], (Unknown, /) -> Unknown]` does not match asserted type `LinearOperator[Any]`
+ tests/sparse/linalg/test__interface.pyi:17:1: error[type-assertion-failure] Type `_CustomLinearOperator[signedinteger[_16Bit], (Unknown, /) -> ToComplex1D | ToComplex2D]` does not match asserted type `LinearOperator[Any]`
- tests/sparse/linalg/test__interface.pyi:18:1: error[type-assertion-failure] Type `_CustomLinearOperator[signedinteger[_64Bit], (Unknown, /) -> Unknown]` does not match asserted type `LinearOperator[Any]`
+ tests/sparse/linalg/test__interface.pyi:18:1: error[type-assertion-failure] Type `_CustomLinearOperator[signedinteger[_64Bit], (Unknown, /) -> ToComplex1D | ToComplex2D]` does not match asserted type `LinearOperator[Any]`
- tests/sparse/linalg/test__interface.pyi:19:1: error[type-assertion-failure] Type `_CustomLinearOperator[float64, (Unknown, /) -> Unknown]` does not match asserted type `LinearOperator[Any]`
+ tests/sparse/linalg/test__interface.pyi:19:1: error[type-assertion-failure] Type `_CustomLinearOperator[float64, (Unknown, /) -> ToComplex1D | ToComplex2D]` does not match asserted type `LinearOperator[Any]`
- tests/sparse/linalg/test__interface.pyi:20:1: error[type-assertion-failure] Type `_CustomLinearOperator[complex128, (Unknown, /) -> Unknown]` does not match asserted type `LinearOperator[Any]`
+ tests/sparse/linalg/test__interface.pyi:20:1: error[type-assertion-failure] Type `_CustomLinearOperator[complex128, (Unknown, /) -> ToComplex1D | ToComplex2D]` does not match asserted type `LinearOperator[Any]`
- tests/sparse/linalg/test__interface.pyi:21:1: error[type-assertion-failure] Type `_CustomLinearOperator[signedinteger[_8Bit] | Any, (Unknown, /) -> Unknown]` does not match asserted type `LinearOperator[Any]`
+ tests/sparse/linalg/test__interface.pyi:21:1: error[type-assertion-failure] Type `_CustomLinearOperator[signedinteger[_8Bit] | Any, (Unknown, /) -> ToComplex1D | ToComplex2D]` does not match asserted type `LinearOperator[Any]`
- tests/special/test_logsumexp.pyi:55:1: error[type-assertion-failure] Type `tuple[floating[Any], complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
+ tests/special/test_logsumexp.pyi:55:1: error[type-assertion-failure] Type `tuple[floating, complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/special/test_logsumexp.pyi:56:1: error[type-assertion-failure] Type `tuple[floating[Any], complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
+ tests/special/test_logsumexp.pyi:56:1: error[type-assertion-failure] Type `tuple[floating, complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/stats/test_pearsonr.pyi:46:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_pearsonr.pyi:46:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_pearsonr.pyi:72:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_pearsonr.pyi:72:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_sem.pyi:41:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_sem.pyi:41:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_sem.pyi:43:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_sem.pyi:43:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_sem.pyi:59:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_sem.pyi:59:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_sem.pyi:61:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_sem.pyi:61:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_sem.pyi:68:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_sem.pyi:68:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`
- tests/stats/test_sem.pyi:70:1: error[type-assertion-failure] Type `floating[Any]` does not match asserted type `float64`
+ tests/stats/test_sem.pyi:70:1: error[type-assertion-failure] Type `floating` does not match asserted type `float64`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/utils/tests/test_extmath.py:1084:20: error[unresolved-attribute] Object of type `spmatrix[complexfloating[Any, Any]]` has no attribute `toarray`
+ sklearn/utils/tests/test_extmath.py:1084:20: error[unresolved-attribute] Object of type `spmatrix[cfloating]` has no attribute `toarray`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5095 diagnostics
+ Found 5096 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2101 diagnostics
+ Found 2099 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/anthropic/ai_task.py:60:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/anthropic/ai_task.py:60:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/anthropic/entity.py:655:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/anthropic/entity.py:655:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/anthropic/entity.py:667:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/anthropic/entity.py:667:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/cloud/ai_task.py:132:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/cloud/ai_task.py:132:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/cloud/entity.py:461:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/cloud/entity.py:461:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/cloud/entity.py:462:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/cloud/entity.py:462:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/cloud/entity.py:463:31: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/cloud/entity.py:463:31: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/conversation/chat_log.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/conversation/chat_log.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "energy_sources" with declared type `list[SourceType]` on TypedDict `EnergyPreferences`: value of type `list[GridSourceType | SolarSourceType | BatterySourceType | GasSourceType | WaterSourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "energy_sources" with declared type `list[SourceType]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
- homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[GridSourceType | SolarSourceType | BatterySourceType | GasSourceType | WaterSourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
- homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption_water" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[GridSourceType | SolarSourceType | BatterySourceType | GasSourceType | WaterSourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption_water" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
- homeassistant/components/google_generative_ai_conversation/ai_task.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/google_generative_ai_conversation/ai_task.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/google_generative_ai_conversation/entity.py:518:37: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `ToolResultContent`, found `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/google_generative_ai_conversation/entity.py:518:37: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `ToolResultContent`, found `Content`
- homeassistant/components/hue/v1/light.py:236:34: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `() -> None`, found `None`
+ homeassistant/components/hue/v1/light.py:236:34: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `CALLBACK_TYPE`, found `None`
- homeassistant/components/ollama/ai_task.py:60:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/ollama/ai_task.py:60:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/open_router/ai_task.py:61:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/open_router/ai_task.py:61:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/open_router/entity.py:264:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/open_router/entity.py:264:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/open_router/entity.py:272:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/open_router/entity.py:272:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/openai_conversation/ai_task.py:82:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/openai_conversation/ai_task.py:82:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/openai_conversation/entity.py:602:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/openai_conversation/entity.py:602:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/openai_conversation/entity.py:605:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
+ homeassistant/components/openai_conversation/entity.py:605:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
- homeassistant/components/otbr/__init__.py:40:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `SyncHardwareFirmwareInfoModule | AsyncHardwareFirmwareInfoModule`, found `<module 'homeassistant.components.otbr.homeassistant_hardware'>`
+ homeassistant/components/otbr/__init__.py:40:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `HardwareFirmwareInfoModule`, found `<module 'homeassistant.components.otbr.homeassistant_hardware'>`
- homeassistant/components/zha/__init__.py:120:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `SyncHardwareFirmwareInfoModule | AsyncHardwareFirmwareInfoModule`, found `<module 'homeassistant.components.zha.homeassistant_hardware'>`
+ homeassistant/components/zha/__init__.py:120:57: error[invalid-argument-type] Argument to function `async_register_firmware_info_provider` is incorrect: Expected `HardwareFirmwareInfoModule`, found `<module 'homeassistant.components.zha.homeassistant_hardware'>`
- homeassistant/helpers/area_registry.py:526:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[_EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder | EventLabelRegistryUpdatedData] | str`, found `EventType[_EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder]`
+ homeassistant/helpers/area_registry.py:526:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[_EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder | EventLabelRegistryUpdatedData] | str`, found `EventType[EventFloorRegistryUpdatedData]`
- homeassistant/helpers/area_registry.py:528:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[_EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder | EventLabelRegistryUpdatedData], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_floor_registry_update(event: Event[_EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder]) -> None`
+ homeassistant/helpers/area_registry.py:528:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[_EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder | EventLabelRegistryUpdatedData], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_floor_registry_update(event: EventFloorRegistryUpdated) -> None`
- homeassistant/helpers/area_registry.py:541:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[EventLabelRegistryUpdatedData | _EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_label_registry_update(event: Event[EventLabelRegistryUpdatedData]) -> None`
+ homeassistant/helpers/area_registry.py:541:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[EventLabelRegistryUpdatedData | _EventFloorRegistryUpdatedData_Create_Remove_Update | _EventFloorRegistryUpdatedData_Reorder], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_label_registry_update(event: EventLabelRegistryUpdated) -> None`
- homeassistant/helpers/entity_registry.py:1064:13: error[invalid-argument-type] Argument is incorrect: Expected `ReadOnlyDict[str, ReadOnlyDict[str, Any]]`, found `ReadOnlyDict[str, ReadOnlyDict[str, Any]] | @Todo | None`
+ homeassistant/helpers/entity_registry.py:1064:13: error[invalid-argument-type] Argument is incorrect: Expected `ReadOnlyEntityOptionsType`, found `ReadOnlyDict[str, ReadOnlyDict[str, Any]] | @Todo | None`
- homeassistant/helpers/entity_registry.py:1865:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[EventLabelRegistryUpdatedData | EventCategoryRegistryUpdatedData], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_label_registry_update(event: Event[EventLabelRegistryUpdatedData]) -> None`
+ homeassistant/helpers/entity_registry.py:1865:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[EventLabelRegistryUpdatedData | EventCategoryRegistryUpdatedData], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_label_registry_update(event: EventLabelRegistryUpdated) -> None`
- homeassistant/helpers/entity_registry.py:1878:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[EventCategoryRegistryUpdatedData | EventLabelRegistryUpdatedData], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_category_registry_update(event: Event[EventCategoryRegistryUpdatedData]) -> None`
+ homeassistant/helpers/entity_registry.py:1878:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[EventCategoryRegistryUpdatedData | EventLabelRegistryUpdatedData], /) -> Coroutine[Any, Any, None] | None`, found `def _handle_category_registry_update(event: EventCategoryRegistryUpdated) -> None`
- homeassistant/helpers/entity_registry.py:1934:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update | Mapping[str, Any]] | str`, found `EventType[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update]`
+ homeassistant/helpers/entity_registry.py:1934:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update | Mapping[str, Any]] | str`, found `EventType[EventEntityRegistryUpdatedData]`
- homeassistant/helpers/entity_registry.py:1935:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update | Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `def cleanup_restored_states(event: Event[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update]) -> None`
+ homeassistant/helpers/entity_registry.py:1935:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update | Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `def cleanup_restored_states(event: Event[EventEntityRegistryUpdatedData]) -> None`

scipy (https://github.com/scipy/scipy)
- scipy/integrate/_quad_vec.py:383:48: error[invalid-argument-type] Argument is incorrect: Expected `Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements`, found `None | Unknown`
+ scipy/integrate/_quad_vec.py:383:48: error[invalid-argument-type] Argument is incorrect: Expected `ArrayLike`, found `None | Unknown`
- scipy/integrate/_quad_vec.py:419:52: error[invalid-argument-type] Argument is incorrect: Expected `Buffer | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | ... omitted 5 union elements`, found `None | Unknown`
+ scipy/integrate/_quad_vec.py:419:52: error[invalid-argument-type] Argument is incorrect: Expected `ArrayLike`, found `None | Unknown`
- scipy/spatial/transform/_rotation.py:55:9: error[invalid-assignment] Object of type `tuple[tuple[Any | Buffer | _SupportsArray[dtype[Any]] | ... omitted 6 union elements, ...] | ndarray[tuple[int], Unknown], ...]` is not assignable to `tuple[tuple[ArrayLike, ...], ...]`
+ scipy/spatial/transform/_rotation.py:55:9: error[invalid-assignment] Object of type `tuple[tuple[ArrayLike, ...] | ndarray[tuple[int], Unknown], ...]` is not assignable to `tuple[tuple[ArrayLike, ...], ...]`


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-29 11:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 11:20_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 3 | 17 |
| `possibly-missing-attribute` | 0 | 0 | 16 |
| `type-assertion-failure` | 0 | 0 | 16 |
| `invalid-return-type` | 2 | 0 | 8 |
| `invalid-assignment` | 0 | 0 | 9 |
| `unresolved-attribute` | 0 | 0 | 1 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **3** | **3** | **67** |


**[Full report with detailed diff](https://8b3c8492.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://8b3c8492.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @mtshiba on 2025-12-29 11:25_

---

_Review requested from @carljm by @mtshiba on 2025-12-29 11:25_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-29 11:25_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-29 11:25_

---

_Review requested from @dcreager by @mtshiba on 2025-12-29 11:25_

---

_@carljm approved on 2025-12-30 03:02_

---

_Merged by @carljm on 2025-12-30 03:02_

---

_Closed by @carljm on 2025-12-30 03:02_

---

_Branch deleted on 2025-12-30 03:03_

---
