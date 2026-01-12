```yaml
number: 22045
title: "Make `analyze_single_pattern_predicate` a `#[salsa::tracked]` function"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/mem
created_at: 2025-12-18T01:53:56Z
updated_at: 2025-12-18T03:01:52Z
url: https://github.com/astral-sh/ruff/pull/22045
synced_at: 2026-01-12T15:57:39Z
```

# Make `analyze_single_pattern_predicate` a `#[salsa::tracked]` function

---

_@charliermarsh_

## Summary

We had a report of a blowup with the following snippet:

```python
from enum import StrEnum

class BigEnum(StrEnum):
    VALUE_01 = "VALUE_01"
    VALUE_02 = "VALUE_02"
    VALUE_03 = "VALUE_03"
    VALUE_04 = "VALUE_04"
    VALUE_05 = "VALUE_05"
    VALUE_06 = "VALUE_06"
    VALUE_07 = "VALUE_07"
    VALUE_08 = "VALUE_08"
    VALUE_09 = "VALUE_09"
    VALUE_10 = "VALUE_10"
    VALUE_11 = "VALUE_11"
    VALUE_12 = "VALUE_12"
    VALUE_13 = "VALUE_13"
    VALUE_14 = "VALUE_14"
    VALUE_15 = "VALUE_15"
    VALUE_16 = "VALUE_16"
    VALUE_17 = "VALUE_17"
    VALUE_18 = "VALUE_18"
    VALUE_19 = "VALUE_19"
    VALUE_20 = "VALUE_20"
    VALUE_21 = "VALUE_21"
    VALUE_22 = "VALUE_22"
    VALUE_23 = "VALUE_23"
    VALUE_24 = "VALUE_24"
    VALUE_25 = "VALUE_25"
    VALUE_26 = "VALUE_26"
    VALUE_27 = "VALUE_27"
    VALUE_28 = "VALUE_28"
    VALUE_29 = "VALUE_29"
    VALUE_30 = "VALUE_30"
    VALUE_31 = "VALUE_31"
    VALUE_32 = "VALUE_32"
    VALUE_33 = "VALUE_33"
    VALUE_34 = "VALUE_34"
    VALUE_35 = "VALUE_35"
    VALUE_36 = "VALUE_36"
    VALUE_37 = "VALUE_37"
    VALUE_38 = "VALUE_38"
    VALUE_39 = "VALUE_39"
    VALUE_40 = "VALUE_40"
    VALUE_41 = "VALUE_41"
    VALUE_42 = "VALUE_42"
    VALUE_43 = "VALUE_43"
    VALUE_44 = "VALUE_44"
    VALUE_45 = "VALUE_45"
    VALUE_46 = "VALUE_46"
    VALUE_47 = "VALUE_47"
    VALUE_48 = "VALUE_48"
    VALUE_49 = "VALUE_49"
    VALUE_50 = "VALUE_50"
    VALUE_51 = "VALUE_51"
    VALUE_52 = "VALUE_52"
    VALUE_53 = "VALUE_53"
    VALUE_54 = "VALUE_54"
    VALUE_55 = "VALUE_55"
    VALUE_56 = "VALUE_56"
    VALUE_57 = "VALUE_57"
    VALUE_58 = "VALUE_58"
    VALUE_59 = "VALUE_59"
    VALUE_60 = "VALUE_60"
    VALUE_61 = "VALUE_61"
    VALUE_62 = "VALUE_62"
    VALUE_63 = "VALUE_63"
    VALUE_64 = "VALUE_64"
    VALUE_65 = "VALUE_65"
    VALUE_66 = "VALUE_66"
    VALUE_67 = "VALUE_67"
    VALUE_68 = "VALUE_68"
    VALUE_69 = "VALUE_69"
    VALUE_70 = "VALUE_70"
    VALUE_71 = "VALUE_71"
    VALUE_72 = "VALUE_72"
    VALUE_73 = "VALUE_73"
    VALUE_74 = "VALUE_74"
    VALUE_75 = "VALUE_75"
    VALUE_76 = "VALUE_76"
    VALUE_77 = "VALUE_77"
    VALUE_78 = "VALUE_78"
    VALUE_79 = "VALUE_79"
    VALUE_80 = "VALUE_80"
    VALUE_81 = "VALUE_81"
    VALUE_82 = "VALUE_82"
    VALUE_83 = "VALUE_83"
    VALUE_84 = "VALUE_84"
    VALUE_85 = "VALUE_85"
    VALUE_86 = "VALUE_86"
    VALUE_87 = "VALUE_87"
    VALUE_88 = "VALUE_88"
    VALUE_89 = "VALUE_89"
    VALUE_90 = "VALUE_90"
    VALUE_91 = "VALUE_91"
    VALUE_92 = "VALUE_92"
    VALUE_93 = "VALUE_93"
    VALUE_94 = "VALUE_94"
    VALUE_95 = "VALUE_95"
    VALUE_96 = "VALUE_96"
    VALUE_97 = "VALUE_97"
    VALUE_98 = "VALUE_98"
    VALUE_99 = "VALUE_99"

    def get_info(self) -> tuple[str, int]:
        match self:
            case BigEnum.VALUE_01:
                return self.value, 1
            case BigEnum.VALUE_02:
                return self.value, 2
            case BigEnum.VALUE_03:
                return self.value, 3
            case BigEnum.VALUE_04:
                return self.value, 4
            case BigEnum.VALUE_05:
                return self.value, 5
            case BigEnum.VALUE_06:
                return self.value, 6
            case BigEnum.VALUE_07:
                return self.value, 7
            case BigEnum.VALUE_08:
                return self.value, 8
            case BigEnum.VALUE_09:
                return self.value, 9
            case BigEnum.VALUE_10:
                return self.value, 10
            case BigEnum.VALUE_11:
                return self.value, 11
            case BigEnum.VALUE_12:
                return self.value, 12
            case BigEnum.VALUE_13:
                return self.value, 13
            case BigEnum.VALUE_14:
                return self.value, 14
            case BigEnum.VALUE_15:
                return self.value, 15
            case BigEnum.VALUE_16:
                return self.value, 16
            case BigEnum.VALUE_17:
                return self.value, 17
            case BigEnum.VALUE_18:
                return self.value, 18
            case BigEnum.VALUE_19:
                return self.value, 19
            case BigEnum.VALUE_20:
                return self.value, 20
            case BigEnum.VALUE_21:
                return self.value, 21
            case BigEnum.VALUE_22:
                return self.value, 22
            case BigEnum.VALUE_23:
                return self.value, 23
            case BigEnum.VALUE_24:
                return self.value, 24
            case BigEnum.VALUE_25:
                return self.value, 25
            case BigEnum.VALUE_26:
                return self.value, 26
            case BigEnum.VALUE_27:
                return self.value, 27
            case BigEnum.VALUE_28:
                return self.value, 28
            case BigEnum.VALUE_29:
                return self.value, 29
            case BigEnum.VALUE_30:
                return self.value, 30
            case BigEnum.VALUE_31:
                return self.value, 31
            case BigEnum.VALUE_32:
                return self.value, 32
            case BigEnum.VALUE_33:
                return self.value, 33
            case BigEnum.VALUE_34:
                return self.value, 34
            case BigEnum.VALUE_35:
                return self.value, 35
            case BigEnum.VALUE_36:
                return self.value, 36
            case BigEnum.VALUE_37:
                return self.value, 37
            case BigEnum.VALUE_38:
                return self.value, 38
            case BigEnum.VALUE_39:
                return self.value, 39
            case BigEnum.VALUE_40:
                return self.value, 40
            case BigEnum.VALUE_41:
                return self.value, 41
            case BigEnum.VALUE_42:
                return self.value, 42
            case BigEnum.VALUE_43:
                return self.value, 43
            case BigEnum.VALUE_44:
                return self.value, 44
            case BigEnum.VALUE_45:
                return self.value, 45
            case BigEnum.VALUE_46:
                return self.value, 46
            case BigEnum.VALUE_47:
                return self.value, 47
            case BigEnum.VALUE_48:
                return self.value, 48
            case BigEnum.VALUE_49:
                return self.value, 49
            case BigEnum.VALUE_50:
                return self.value, 50
            case BigEnum.VALUE_51:
                return self.value, 51
            case BigEnum.VALUE_52:
                return self.value, 52
            case BigEnum.VALUE_53:
                return self.value, 53
            case BigEnum.VALUE_54:
                return self.value, 54
            case BigEnum.VALUE_55:
                return self.value, 55
            case BigEnum.VALUE_56:
                return self.value, 56
            case BigEnum.VALUE_57:
                return self.value, 57
            case BigEnum.VALUE_58:
                return self.value, 58
            case BigEnum.VALUE_59:
                return self.value, 59
            case BigEnum.VALUE_60:
                return self.value, 60
            case BigEnum.VALUE_61:
                return self.value, 61
            case BigEnum.VALUE_62:
                return self.value, 62
            case BigEnum.VALUE_63:
                return self.value, 63
            case BigEnum.VALUE_64:
                return self.value, 64
            case BigEnum.VALUE_65:
                return self.value, 65
            case BigEnum.VALUE_66:
                return self.value, 66
            case BigEnum.VALUE_67:
                return self.value, 67
            case BigEnum.VALUE_68:
                return self.value, 68
            case BigEnum.VALUE_69:
                return self.value, 69
            case BigEnum.VALUE_70:
                return self.value, 70
            case BigEnum.VALUE_71:
                return self.value, 71
            case BigEnum.VALUE_72:
                return self.value, 72
            case BigEnum.VALUE_73:
                return self.value, 73
            case BigEnum.VALUE_74:
                return self.value, 74
            case BigEnum.VALUE_75:
                return self.value, 75
            case BigEnum.VALUE_76:
                return self.value, 76
            case BigEnum.VALUE_77:
                return self.value, 77
            case BigEnum.VALUE_78:
                return self.value, 78
            case BigEnum.VALUE_79:
                return self.value, 79
            case BigEnum.VALUE_80:
                return self.value, 80
            case BigEnum.VALUE_81:
                return self.value, 81
            case BigEnum.VALUE_82:
                return self.value, 82
            case BigEnum.VALUE_83:
                return self.value, 83
            case BigEnum.VALUE_84:
                return self.value, 84
            case BigEnum.VALUE_85:
                return self.value, 85
            case BigEnum.VALUE_86:
                return self.value, 86
            case BigEnum.VALUE_87:
                return self.value, 87
            case BigEnum.VALUE_88:
                return self.value, 88
            case BigEnum.VALUE_89:
                return self.value, 89
            case BigEnum.VALUE_90:
                return self.value, 90
            case BigEnum.VALUE_91:
                return self.value, 91
            case BigEnum.VALUE_92:
                return self.value, 92
            case BigEnum.VALUE_93:
                return self.value, 93
            case BigEnum.VALUE_94:
                return self.value, 94
            case BigEnum.VALUE_95:
                return self.value, 95
            case BigEnum.VALUE_96:
                return self.value, 96
            case BigEnum.VALUE_97:
                return self.value, 97
            case BigEnum.VALUE_98:
                return self.value, 98
            case BigEnum.VALUE_99:
                return self.value, 99
```

On my machine, memoizing the computation brings us from 70s to 0.6s.


---

_Comment by @astral-sh-bot[bot] on 2025-12-18 01:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 01:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2795 diagnostics
+ Found 2792 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1221:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- Found 5081 diagnostics
+ Found 5078 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2080 diagnostics
+ Found 2082 diagnostics

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


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-18 02:39_

---

_Review requested from @carljm by @charliermarsh on 2025-12-18 02:39_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-18 02:39_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-18 02:39_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-18 02:39_

---

_@dcreager approved on 2025-12-18 02:43_

That's a big enum!

---

_Merged by @charliermarsh on 2025-12-18 03:01_

---

_Closed by @charliermarsh on 2025-12-18 03:01_

---

_Branch deleted on 2025-12-18 03:01_

---
