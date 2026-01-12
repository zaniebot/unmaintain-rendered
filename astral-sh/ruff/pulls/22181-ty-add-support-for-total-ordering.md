```yaml
number: 22181
title: "[ty] Add support for `@total_ordering`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/functools
created_at: 2025-12-24T19:24:22Z
updated_at: 2026-01-06T03:47:06Z
url: https://github.com/astral-sh/ruff/pull/22181
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Add support for `@total_ordering`

---

_@charliermarsh_

## Summary

We have some suppressions in the pyx codebase related to this, so wanted to resolve.

Closes https://github.com/astral-sh/ty/issues/1202.


---

_Label `ty` added by @charliermarsh on 2025-12-24 19:24_

---

_@charliermarsh reviewed on 2025-12-24 19:24_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:202 on 2025-12-24 19:24_

I wonder if we should have a dedicated diagnostic for this?

---

_@charliermarsh reviewed on 2025-12-24 19:26_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:202 on 2025-12-24 19:26_

Yeah, I think we should -- it seems like Pyright does.

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 19:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-24 19:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mkosi (https://github.com/systemd/mkosi)
- mkosi/qemu.py:1062:9: error[unsupported-operator] Operator `>=` is not supported between two objects of type `GenericVersion`
- mkosi/qemu.py:1171:12: error[unsupported-operator] Operator `>=` is not supported between two objects of type `GenericVersion`
- Found 98 diagnostics
+ Found 96 diagnostics

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

cloud-init (https://github.com/canonical/cloud-init)
- tests/integration_tests/dropins/test_custom_modules.py:26:8: error[unsupported-operator] Operator `>=` is not supported between two objects of type `Release`
- tests/integration_tests/modules/test_disk_setup.py:263:12: error[unsupported-operator] Operator `<=` is not supported between two objects of type `Release`
- tests/integration_tests/net/test_dhcp.py:19:9: error[unsupported-operator] Operator `>=` is not supported between two objects of type `Release`
- tests/integration_tests/test_defaults.py:58:12: error[unsupported-operator] Operator `>=` is not supported between two objects of type `Release`
- Found 1187 diagnostics
+ Found 1183 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5156 diagnostics
+ Found 5155 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2094 diagnostics
+ Found 2092 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-24 19:29_

Keeping in draft while I add a new diagnostic.

---

_Comment by @charliermarsh on 2025-12-24 19:30_

Although that could plausibly be its own PR.

---

_@charliermarsh reviewed on 2025-12-24 19:47_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:202 on 2025-12-24 19:47_

https://github.com/astral-sh/ruff/pull/22183

---

_Comment by @charliermarsh on 2025-12-24 19:48_

I implemented the diagnostic in a separate PR (since it's absence here doesn't cause any false positives or false negatives on the comparators, at least): https://github.com/astral-sh/ruff/pull/22183

---

_Marked ready for review by @charliermarsh on 2025-12-24 19:59_

---

_Review requested from @carljm by @charliermarsh on 2025-12-24 19:59_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-24 19:59_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-24 19:59_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-24 19:59_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-24 19:59_

---

_Comment by @AlexWaygood on 2025-12-24 20:01_

> in the ty codebase

guessing you mean pyx? :P

---

_Comment by @charliermarsh on 2025-12-24 20:04_

Err… yes :)

---

_Comment by @AlexWaygood on 2025-12-24 20:04_

I'm surprised there aren't more diagnostics going away in the ecosystem. I thought `functools.total_ordering` was pretty popular

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1414 on 2025-12-24 20:09_

this doesn't seem necessary, I think strum's default is fine here. No tests fail with it removed:

```suggestion
```

---

_@AlexWaygood reviewed on 2025-12-24 20:09_

---

_Comment by @charliermarsh on 2025-12-24 20:34_

I'm surprised it's not in the conformance tests!

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-24 20:45_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-05 21:00_

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 21:05_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 4 | 2 | 5 |
| `unsupported-operator` | 0 | 6 | 0 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 0 | 1 | 4 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `invalid-await` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **9** | **10** | **17** |


**[Full report with detailed diff](https://1314c0a1.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://1314c0a1.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:4 on 2026-01-06 02:57_

Defining `__eq__` is not required -- it can be inherited from `object`. So it's sufficient to just define a single comparison method. This should maybe be tested?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:1 on 2026-01-06 02:59_

Should we also test that our synthesized methods don't override an explicitly-defined method? E.g. if you define two methods explicitly, `total_ordering` won't override either of them.

Also, can we test the combined usage of `total_ordering` and `dataclass`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:2988 on 2026-01-06 03:02_

This seems like something that could be lazily computed (and Salsa cached), rather than eagerly computed and stored for every single class (even though only a tiny minority of them will use `total_ordering`).

But given that the PR doesn't show a CPU or memory regression, I guess it's not worth it?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:2973 on 2026-01-06 03:04_

TBH this could also be lazily computed instead of eagerly, though the benefit there is less, since the lazy computation would be triggered for any class whose instances are ever compared.

---

_@carljm approved on 2026-01-06 03:05_

Looks great! A few test suggestions, nothing blocking. Thank you!!

---

_Comment by @charliermarsh on 2026-01-06 03:24_

Thank you!

---

_@carljm reviewed on 2026-01-06 03:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:146 on 2026-01-06 03:28_

Hmm, this test doesn't really verify that we aren't overwriting these methods with our synthesized version, because they are indistinguishable. I think to actually be useful, the explicit method would need to be different (e.g. accept more types, or have a narrower return type (e.g. `Literal[True]`.)

---

_@charliermarsh reviewed on 2026-01-06 03:29_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/decorators/total_ordering.md`:146 on 2026-01-06 03:29_

Yeah that makes sense -- sorry.

---

_Merged by @charliermarsh on 2026-01-06 03:47_

---

_Closed by @charliermarsh on 2026-01-06 03:47_

---

_Branch deleted on 2026-01-06 03:47_

---
