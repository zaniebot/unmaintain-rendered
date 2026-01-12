```yaml
number: 22065
title: "[ty] Cache `ClassType::nearest_disjoint_base`"
type: pull_request
state: open
author: zanieb
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: zb/cache-nearest
created_at: 2025-12-18T22:32:26Z
updated_at: 2026-01-06T15:49:52Z
url: https://github.com/astral-sh/ruff/pull/22065
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Cache `ClassType::nearest_disjoint_base`

---

_@zanieb_

Pulled out of https://github.com/astral-sh/ruff/pull/22052

Locally, this improved performance on Pydantic by 5%, avoiding the regressions in the referenced pull request.

---

_Label `performance` added by @AlexWaygood on 2025-12-18 22:33_

---

_Label `ty` added by @AlexWaygood on 2025-12-18 22:33_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 22:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 22:35_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

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

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Yarn[Any], generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5160 diagnostics
+ Found 5159 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2095 diagnostics
+ Found 2093 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-18 22:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zb%2Fcache-nearest?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22065 will **improve performance by 4.23%**

<sub>Comparing <code>zb/cache-nearest</code> (d83e0dc) with <code>main</code> (fd86e69)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/zb%2Fcache-nearest?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1.2 s | +4.23% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/zb%2Fcache-nearest?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @zanieb on 2025-12-18 23:19_

Hm running this again locally (due to the lack of difference on Pydantic in the CodSpeed report), I can't reproduce the performance difference on Pydantic. I'll poke around some more.

---

_Comment by @AlexWaygood on 2025-12-18 23:34_

Seems like it could be worth it just for the memory reduction reported by mypy_primer (though I don't fully understand why this would reduce memory usage)

---

_Comment by @zanieb on 2025-12-19 00:28_

Before https://github.com/astral-sh/ruff/pull/22044, this was the best commit from https://github.com/astral-sh/ruff/pull/22052 but now the rest of https://github.com/astral-sh/ruff/pull/22052 looks more promising again 🙃 

This commit alone looks like ~no difference on pydantic locally (and in CI) now.

---

_Comment by @MichaReiser on 2025-12-19 09:01_

It could make sense to run ty locally with `TY_MEMORY_REPORT=full` between main/this branch. 

One reason why memory usage might be reduced is that this PR deduplicates some salsa metadata because queries calling `nearest_disjoint_base` now track a single dependency on that query whereas before, they each tracked all the dependencies read in the `nearest_disjoint_base` function. But it's hard to tell from the aggregated metrics.

---

_Comment by @zanieb on 2025-12-19 14:46_

After rebasing on `main` the sphinx memory difference went away and now there's a diff on prefect, feeling wary.

---

_Comment by @MichaReiser on 2025-12-19 14:53_

> After rebasing on main the sphinx memory difference went away and now there's a diff on prefect, feeling wary.

We round the the memory usage numbers to get more stable results. However, this also means that not all changes show in the memory usage report. You can run ty locally with `TY_MEMORY_REPORT=full` to get a detailed report without rounded numbers (which I find very useful because it also helps you understand the differences at the query level)

---

_Comment by @zanieb on 2025-12-19 14:59_

I'm doing that, and the diff in sphinx went away :p

I also can't see a diff in Prefect locally.

---

_Comment by @AlexWaygood on 2026-01-02 20:41_

I ran ty with the environment variable `TY_MEMORY_REPORT=full` set on a large project locally to measure the impact of this PR on memory usage.

On `main`:

```
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 30385.89MB
    struct metadata = 2214.05MB
    struct fields = 2102.11MB
    memo metadata = 10230.30MB
    memo fields = 15839.43MB
```

On this PR (after rebasing it on `main`):

```
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 30379.73MB
    struct metadata = 2214.07MB
    struct fields = 2102.13MB
    memo metadata = 10197.79MB
    memo fields = 15865.74MB
```

So, this PR does not lead to an especially _significant_ memory reduction when run on a _large_ project -- but it is still a reduction in memory usage. Given the performance improvement reported by codspeed, and the fact that we know that this method is very hot, I think this PR is very worthwhile.

Here are the full Salsa reports.

<details>
<summary>main</summary>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=252.59MB fields=353.62MB count=6314730
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=262.61MB fields=187.58MB count=4689420
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=215.69MB fields=184.88MB count=3851619
`FunctionType`                                     metadata=42.43MB  fields=176.09MB count=757733
`is_redundant_with_impl::interned_arguments`       metadata=265.57MB fields=151.75MB count=4742318
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=164.39MB fields=140.91MB count=2935573
`IntersectionType`                                 metadata=33.00MB  fields=128.93MB count=589290
`CallableType`                                     metadata=26.97MB  fields=105.86MB count=481562
`Expression`                                       metadata=200.50MB fields=100.25MB count=4177081
`UnionType`                                        metadata=52.41MB  fields=77.93MB  count=935839
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=84.14MB  fields=72.12MB  count=1502483
`StringLiteralType`                                metadata=61.91MB  fields=69.26MB  count=1105538
`File`                                             metadata=17.50MB  fields=61.71MB  count=273385
`OverloadLiteral`                                  metadata=32.94MB  fields=43.45MB  count=588224
`place_by_id::interned_arguments`                  metadata=96.80MB  fields=37.23MB  count=1861536
`Specialization`                                   metadata=33.75MB  fields=36.44MB  count=602648
`Type < 'db >::apply_specialization_::interned_arguments` metadata=72.97MB  fields=31.27MB  count=1303118
`BoundMethodType`                                  metadata=52.26MB  fields=22.40MB  count=933263
`BoundTypeVarInstance`                             metadata=37.27MB  fields=14.33MB  count=716733
`ScopeId`                                          metadata=28.97MB  fields=12.42MB  count=1034660
`ClassLiteral`                                     metadata=8.43MB   fields=11.87MB  count=150561
`TupleType`                                        metadata=7.86MB   fields=10.94MB  count=140302
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=30.17MB  fields=8.62MB   count=538728
`TypeVarInstance`                                  metadata=8.59MB   fields=7.36MB   count=153391
`GenericAlias`                                     metadata=24.73MB  fields=7.07MB   count=441659
`FileModule`                                       metadata=2.82MB   fields=4.94MB   count=50427
`FunctionLiteral`                                  metadata=32.94MB  fields=4.71MB   count=588224
`ExpressionWithContext`                            metadata=10.88MB  fields=4.66MB   count=194215
`code_generator_of_class::interned_arguments`      metadata=15.78MB  fields=4.51MB   count=281793
`ModuleLiteralType`                                metadata=11.21MB  fields=4.31MB   count=215489
`ModuleNameIngredient`                             metadata=2.86MB   fields=4.19MB   count=51006
`ProtocolInterface`                                metadata=2.00MB   fields=4.03MB   count=35628
`TypeVarIdentity`                                  metadata=5.18MB   fields=3.70MB   count=92496
`Unpack`                                           metadata=2.93MB   fields=2.34MB   count=73171
`BytesLiteralType`                                 metadata=0.49MB   fields=2.01MB   count=8761
`EnumLiteralType`                                  metadata=2.02MB   fields=1.44MB   count=35995
`Project`                                          metadata=0.00MB   fields=0.92MB   count=1
`GenericContext`                                   metadata=0.53MB   fields=0.85MB   count=9437
`lookup_dunder_new_inner::interned_arguments`      metadata=2.99MB   fields=0.85MB   count=53350
`StarImportPlaceholderPredicate`                   metadata=1.19MB   fields=0.85MB   count=42390
`BoundSuperType`                                   metadata=1.35MB   fields=0.77MB   count=24023
`ClassLiteral < 'db >::fields_::interned_arguments` metadata=1.34MB   fields=0.72MB   count=25710
`PropertyInstanceType`                             metadata=0.96MB   fields=0.55MB   count=17185
`InteriorNode`                                     metadata=0.44MB   fields=0.37MB   count=7805
`PatternPredicate`                                 metadata=0.10MB   fields=0.26MB   count=3194
`ConstrainedTypeVar`                               metadata=0.29MB   fields=0.21MB   count=5127
`is_equivalent_to_object_inner::interned_arguments` metadata=0.83MB   fields=0.19MB   count=16045
`FieldInstance`                                    metadata=0.14MB   fields=0.13MB   count=2487
`UnionTypeInstance`                                metadata=0.08MB   fields=0.11MB   count=1394
`NewType`                                          metadata=0.06MB   fields=0.05MB   count=1109
`InteriorNode < 'db >::and_::interned_arguments`   metadata=0.12MB   fields=0.05MB   count=2064
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`TypeIsType`                                       metadata=0.02MB   fields=0.01MB   count=392
`ClassLiteral < 'db >::variance_of_::interned_arguments` metadata=0.02MB   fields=0.01MB   count=384
`InteriorNode < 'db >::exists_one_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=177
`InternedType`                                     metadata=0.02MB   fields=0.00MB   count=281
`TypeVarConstraints`                               metadata=0.00MB   fields=0.00MB   count=68
`TypeGuardType`                                    metadata=0.01MB   fields=0.00MB   count=120
`GenericAlias < 'db >::variance_of_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=160
`InteriorNode < 'db >::or_::interned_arguments`    metadata=0.00MB   fields=0.00MB   count=74
`DataclassParams`                                  metadata=0.00MB   fields=0.00MB   count=28
`DeprecatedInstance`                               metadata=0.01MB   fields=0.00MB   count=121
`SynthesizedTypedDictType`                         metadata=0.00MB   fields=0.00MB   count=8
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=3
`DataclassTransformerParams`                       metadata=0.00MB   fields=0.00MB   count=5
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=8
`KnownClassArgument`                               metadata=0.00MB   fields=0.00MB   count=51
`is_function_definition::interned_arguments`       metadata=0.00MB   fields=0.00MB   count=4
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=287.41MB fields=6808.72MB count=93616
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=7.15MB   fields=3423.52MB count=94120
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=1815.30MB fields=1301.33MB count=5351320
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=738.85MB fields=1284.62MB count=914079
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=1903.29MB fields=824.99MB count=3818754
`source_text -> ruff_db::source::SourceText`
    metadata=6.02MB   fields=771.83MB count=94120
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=21.84MB  fields=202.58MB count=90643
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=522.42MB fields=150.06MB count=4689420
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=73.35MB  fields=145.39MB count=608882
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=214.61MB fields=132.10MB count=596964
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=1056.23MB fields=123.25MB count=3851619
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=1055.38MB fields=93.94MB  count=2935573
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=109.01MB fields=78.12MB  count=538728
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=217.82MB fields=68.22MB  count=528681
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=35.16MB  fields=66.13MB  count=291911
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=144.21MB fields=59.57MB  count=1861536
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=1.15MB   fields=56.77MB  count=22036
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=48.31MB  fields=37.59MB  count=305011
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=606.53MB fields=36.06MB  count=1502483
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=190.26MB fields=20.85MB  count=1303118
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=17.95MB  fields=18.61MB  count=25710
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=7.75MB   fields=16.42MB  count=67125
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=84.89MB  fields=16.11MB  count=1006724
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=6.89MB   fields=14.23MB  count=90643
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=42.32MB  fields=14.14MB  count=585863
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=55.73MB  fields=12.72MB  count=144525
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=20.78MB  fields=11.86MB  count=65431
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=30.47MB  fields=7.18MB   count=149579
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_module_resolver::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=2.85MB   fields=6.61MB   count=54883
`is_redundant_with_impl -> bool`
    metadata=515.67MB fields=4.74MB   count=4742318
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=12.46MB  fields=4.68MB   count=150353
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=36.97MB  fields=3.38MB   count=281793
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=20.08MB  fields=3.09MB   count=386067
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=47.08MB  fields=2.85MB   count=355991
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=10.85MB  fields=2.81MB   count=145838
`InteriorNode < 'db >::sequent_map_ -> ty_python_semantic::types::constraints::SequentMap<'_>`
    metadata=0.46MB   fields=1.84MB   count=6235
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=11.48MB  fields=1.77MB   count=220824
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=26.39MB  fields=1.71MB   count=53350
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.03MB  fields=1.20MB   count=150353
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.02MB  fields=1.20MB   count=150059
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.08MB   fields=1.12MB   count=915
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.38MB   fields=0.73MB   count=6423
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=6.53MB   fields=0.73MB   count=90643
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=62.69MB  fields=0.66MB   count=660174
`resolve_module_query -> core::option::Option<ty_module_resolver::module::Module<'_>>`
    metadata=22.03MB  fields=0.61MB   count=51006
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=3.76MB   fields=0.58MB   count=72265
`absolute_desperate_search_paths -> core::option::Option<alloc::vec::Vec<ty_module_resolver::path::SearchPath>>`
    metadata=0.19MB   fields=0.43MB   count=2196
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.41MB   fields=0.31MB   count=2133
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=17.59MB  fields=0.31MB   count=2306
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=9.35MB   fields=0.25MB   count=31679
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=11.58MB  fields=0.25MB   count=20799
`class_based_items -> ty_python_semantic::types::typed_dict::TypedDictSchema<'_>`
    metadata=0.05MB   fields=0.18MB   count=635
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=17.30MB  fields=0.15MB   count=150192
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=17.76MB  fields=0.15MB   count=148321
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.04MB   fields=0.08MB   count=360
`file_to_module -> core::option::Option<ty_module_resolver::module::Module<'_>>`
    metadata=0.35MB   fields=0.04MB   count=3267
`InteriorNode < 'db >::and_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.14MB   fields=0.02MB   count=2064
`is_equivalent_to_object_inner -> bool`
    metadata=4.05MB   fields=0.02MB   count=16045
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.16MB   fields=0.01MB   count=615
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.10MB   fields=0.01MB   count=889
`NewType < 'db >::lazy_base_ -> ty_python_semantic::types::newtype::NewTypeBase<'_>`
    metadata=0.13MB   fields=0.01MB   count=772
`analyze_pattern_predicate -> ty_python_semantic::types::Truthiness`
    metadata=26.13MB  fields=0.00MB   count=3048
`InteriorNode < 'db >::exists_one_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.01MB   fields=0.00MB   count=177
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.01MB   fields=0.00MB   count=57
`InteriorNode < 'db >::or_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.00MB   fields=0.00MB   count=74
`InteriorNode < 'db >::negate_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.00MB   fields=0.00MB   count=61
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=29
`known_class_to_class_literal -> core::option::Option<ty_python_semantic::types::class::ClassLiteral<'_>>`
    metadata=0.01MB   fields=0.00MB   count=51
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.04MB   fields=0.00MB   count=384
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.01MB   fields=0.00MB   count=160
`dynamic_resolution_paths -> alloc::vec::Vec<ty_module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
`relative_desperate_search_paths -> core::option::Option<ty_module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=6
`is_function_definition -> bool`
    metadata=0.00MB   fields=0.00MB   count=4
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 30385.89MB
    struct metadata = 2214.05MB
    struct fields = 2102.11MB
    memo metadata = 10230.30MB
    memo fields = 15839.43MB
```

</details>

<details>
<summary>This PR</summary>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=252.59MB fields=353.62MB count=6314730
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=262.61MB fields=187.58MB count=4689420
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=215.69MB fields=184.88MB count=3851617
`FunctionType`                                     metadata=42.43MB  fields=176.09MB count=757733
`is_redundant_with_impl::interned_arguments`       metadata=265.59MB fields=151.76MB count=4742596
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=164.40MB fields=140.91MB count=2935660
`IntersectionType`                                 metadata=33.00MB  fields=128.93MB count=589315
`CallableType`                                     metadata=26.97MB  fields=105.86MB count=481561
`Expression`                                       metadata=200.50MB fields=100.25MB count=4177081
`UnionType`                                        metadata=52.41MB  fields=77.93MB  count=935872
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=84.14MB  fields=72.12MB  count=1502480
`StringLiteralType`                                metadata=61.91MB  fields=69.26MB  count=1105538
`File`                                             metadata=17.50MB  fields=61.71MB  count=273385
`OverloadLiteral`                                  metadata=32.94MB  fields=43.45MB  count=588224
`place_by_id::interned_arguments`                  metadata=96.80MB  fields=37.23MB  count=1861536
`Specialization`                                   metadata=33.75MB  fields=36.44MB  count=602623
`Type < 'db >::apply_specialization_::interned_arguments` metadata=72.97MB  fields=31.27MB  count=1303122
`BoundMethodType`                                  metadata=52.26MB  fields=22.40MB  count=933263
`BoundTypeVarInstance`                             metadata=37.27MB  fields=14.33MB  count=716732
`ScopeId`                                          metadata=28.97MB  fields=12.42MB  count=1034660
`ClassLiteral`                                     metadata=8.43MB   fields=11.87MB  count=150561
`TupleType`                                        metadata=7.86MB   fields=10.94MB  count=140301
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=30.17MB  fields=8.62MB   count=538701
`TypeVarInstance`                                  metadata=8.59MB   fields=7.36MB   count=153390
`GenericAlias`                                     metadata=24.73MB  fields=7.07MB   count=441632
`FileModule`                                       metadata=2.82MB   fields=4.94MB   count=50427
`FunctionLiteral`                                  metadata=32.94MB  fields=4.71MB   count=588224
`ExpressionWithContext`                            metadata=10.88MB  fields=4.66MB   count=194232
`code_generator_of_class::interned_arguments`      metadata=15.78MB  fields=4.51MB   count=281786
`ModuleLiteralType`                                metadata=11.21MB  fields=4.31MB   count=215489
`ModuleNameIngredient`                             metadata=2.86MB   fields=4.19MB   count=51006
`ProtocolInterface`                                metadata=2.00MB   fields=4.04MB   count=35633
`TypeVarIdentity`                                  metadata=5.18MB   fields=3.70MB   count=92496
`Unpack`                                           metadata=2.93MB   fields=2.34MB   count=73171
`BytesLiteralType`                                 metadata=0.49MB   fields=2.01MB   count=8761
`EnumLiteralType`                                  metadata=2.02MB   fields=1.44MB   count=35995
`Project`                                          metadata=0.00MB   fields=0.92MB   count=1
`GenericContext`                                   metadata=0.53MB   fields=0.85MB   count=9437
`lookup_dunder_new_inner::interned_arguments`      metadata=2.99MB   fields=0.85MB   count=53350
`StarImportPlaceholderPredicate`                   metadata=1.19MB   fields=0.85MB   count=42390
`BoundSuperType`                                   metadata=1.35MB   fields=0.77MB   count=24023
`ClassLiteral < 'db >::fields_::interned_arguments` metadata=1.34MB   fields=0.72MB   count=25710
`PropertyInstanceType`                             metadata=0.96MB   fields=0.55MB   count=17185
`InteriorNode`                                     metadata=0.44MB   fields=0.37MB   count=7811
`PatternPredicate`                                 metadata=0.10MB   fields=0.26MB   count=3194
`ConstrainedTypeVar`                               metadata=0.29MB   fields=0.21MB   count=5128
`is_equivalent_to_object_inner::interned_arguments` metadata=0.83MB   fields=0.19MB   count=16045
`FieldInstance`                                    metadata=0.14MB   fields=0.13MB   count=2487
`UnionTypeInstance`                                metadata=0.08MB   fields=0.11MB   count=1394
`NewType`                                          metadata=0.06MB   fields=0.05MB   count=1109
`InteriorNode < 'db >::and_::interned_arguments`   metadata=0.12MB   fields=0.05MB   count=2087
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`TypeIsType`                                       metadata=0.02MB   fields=0.01MB   count=392
`ClassLiteral < 'db >::variance_of_::interned_arguments` metadata=0.02MB   fields=0.01MB   count=385
`InteriorNode < 'db >::exists_one_::interned_arguments` metadata=0.01MB   fields=0.01MB   count=179
`InternedType`                                     metadata=0.02MB   fields=0.00MB   count=281
`TypeVarConstraints`                               metadata=0.00MB   fields=0.00MB   count=68
`TypeGuardType`                                    metadata=0.01MB   fields=0.00MB   count=120
`GenericAlias < 'db >::variance_of_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=161
`InteriorNode < 'db >::or_::interned_arguments`    metadata=0.00MB   fields=0.00MB   count=75
`DataclassParams`                                  metadata=0.00MB   fields=0.00MB   count=28
`DeprecatedInstance`                               metadata=0.01MB   fields=0.00MB   count=121
`SynthesizedTypedDictType`                         metadata=0.00MB   fields=0.00MB   count=8
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=3
`DataclassTransformerParams`                       metadata=0.00MB   fields=0.00MB   count=5
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=8
`KnownClassArgument`                               metadata=0.00MB   fields=0.00MB   count=51
`is_function_definition::interned_arguments`       metadata=0.00MB   fields=0.00MB   count=4
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=287.41MB fields=6808.72MB count=93616
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=7.15MB   fields=3449.52MB count=94120
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=1809.18MB fields=1301.33MB count=5351320
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=718.16MB fields=1284.62MB count=914079
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=1890.31MB fields=825.00MB count=3818771
`source_text -> ruff_db::source::SourceText`
    metadata=6.02MB   fields=771.83MB count=94120
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=21.84MB  fields=202.58MB count=90643
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=522.43MB fields=150.06MB count=4689420
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=73.35MB  fields=145.39MB count=608882
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=214.63MB fields=132.10MB count=596964
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=1056.23MB fields=123.25MB count=3851617
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=1055.40MB fields=93.94MB  count=2935660
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=109.00MB fields=78.12MB  count=538701
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=217.80MB fields=68.22MB  count=528681
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=35.16MB  fields=66.13MB  count=291911
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=144.21MB fields=59.57MB  count=1861536
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=1.15MB   fields=56.77MB  count=22036
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=48.31MB  fields=37.59MB  count=305011
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=606.52MB fields=36.06MB  count=1502480
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=190.00MB fields=20.85MB  count=1303122
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=17.95MB  fields=18.61MB  count=25710
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=7.75MB   fields=16.42MB  count=67125
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=84.94MB  fields=16.11MB  count=1006724
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=6.89MB   fields=14.23MB  count=90643
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=42.32MB  fields=14.14MB  count=585863
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=55.73MB  fields=12.72MB  count=144525
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=20.78MB  fields=11.86MB  count=65431
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=30.47MB  fields=7.18MB   count=149579
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_module_resolver::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=2.85MB   fields=6.61MB   count=54883
`is_redundant_with_impl -> bool`
    metadata=512.63MB fields=4.74MB   count=4742596
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=12.46MB  fields=4.68MB   count=150353
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=36.97MB  fields=3.38MB   count=281786
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=20.08MB  fields=3.09MB   count=386067
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=47.08MB  fields=2.85MB   count=355991
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=10.85MB  fields=2.81MB   count=145838
`InteriorNode < 'db >::sequent_map_ -> ty_python_semantic::types::constraints::SequentMap<'_>`
    metadata=0.45MB   fields=1.84MB   count=6219
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=11.48MB  fields=1.77MB   count=220824
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=26.40MB  fields=1.71MB   count=53350
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.03MB  fields=1.20MB   count=150353
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=12.02MB  fields=1.20MB   count=150059
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.08MB   fields=1.12MB   count=915
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.38MB   fields=0.73MB   count=6423
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=6.53MB   fields=0.73MB   count=90643
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=62.71MB  fields=0.66MB   count=660174
`resolve_module_query -> core::option::Option<ty_module_resolver::module::Module<'_>>`
    metadata=22.04MB  fields=0.61MB   count=51006
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=3.76MB   fields=0.58MB   count=72265
`absolute_desperate_search_paths -> core::option::Option<alloc::vec::Vec<ty_module_resolver::path::SearchPath>>`
    metadata=0.19MB   fields=0.43MB   count=2196
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.41MB   fields=0.31MB   count=2133
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::narrow::NarrowingConstraint<'_>, rustc_hash::FxBuildHasher>>`
    metadata=17.59MB  fields=0.31MB   count=2306
`ClassType < 'db >::nearest_disjoint_base_ -> core::option::Option<ty_python_semantic::types::class::DisjointBase<'_>>`
    metadata=10.62MB  fields=0.31MB   count=25446
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=9.35MB   fields=0.25MB   count=31679
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=11.58MB  fields=0.25MB   count=20798
`class_based_items -> ty_python_semantic::types::typed_dict::TypedDictSchema<'_>`
    metadata=0.05MB   fields=0.18MB   count=635
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=17.30MB  fields=0.15MB   count=150192
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=17.76MB  fields=0.15MB   count=148321
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.04MB   fields=0.08MB   count=360
`file_to_module -> core::option::Option<ty_module_resolver::module::Module<'_>>`
    metadata=0.35MB   fields=0.04MB   count=3267
`InteriorNode < 'db >::and_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.14MB   fields=0.03MB   count=2087
`is_equivalent_to_object_inner -> bool`
    metadata=4.05MB   fields=0.02MB   count=16045
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.16MB   fields=0.01MB   count=615
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.10MB   fields=0.01MB   count=889
`NewType < 'db >::lazy_base_ -> ty_python_semantic::types::newtype::NewTypeBase<'_>`
    metadata=0.13MB   fields=0.01MB   count=772
`analyze_pattern_predicate -> ty_python_semantic::types::Truthiness`
    metadata=26.04MB  fields=0.00MB   count=3048
`InteriorNode < 'db >::exists_one_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.01MB   fields=0.00MB   count=179
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.01MB   fields=0.00MB   count=57
`InteriorNode < 'db >::or_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.00MB   fields=0.00MB   count=75
`InteriorNode < 'db >::negate_ -> ty_python_semantic::types::constraints::Node<'_>`
    metadata=0.00MB   fields=0.00MB   count=67
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.00MB   fields=0.00MB   count=29
`known_class_to_class_literal -> core::option::Option<ty_python_semantic::types::class::ClassLiteral<'_>>`
    metadata=0.01MB   fields=0.00MB   count=51
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.04MB   fields=0.00MB   count=385
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.01MB   fields=0.00MB   count=161
`dynamic_resolution_paths -> alloc::vec::Vec<ty_module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
`relative_desperate_search_paths -> core::option::Option<ty_module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=6
`is_function_definition -> bool`
    metadata=0.00MB   fields=0.00MB   count=4
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 30379.73MB
    struct metadata = 2214.07MB
    struct fields = 2102.13MB
    memo metadata = 10197.79MB
    memo fields = 15865.74MB
```

</details>

---

_Comment by @AlexWaygood on 2026-01-06 15:37_

@zanieb any reason why this is still in draft? Looks landable to me :-)

---

_Comment by @MichaReiser on 2026-01-06 15:40_

> So, this PR does not lead to an especially significant memory reduction when run on a large project -- but it is still a reduction in memory usage. Given the performance improvement reported by codspeed, and the fact that we know that this method is very hot, I think this PR is very worthwhile.

I think this is a bit more nuanced. Codspeed does report that performance regresses for some benchmarks. The memory improvement is nice but I expect it to be less (or total memory usage might even increase) if we reduce the metadata that salsa tracks, e.g. by https://github.com/salsa-rs/salsa/pull/1045

Because of that, I would suggest we don't give too much weight to the memory improvement and make the merge decision solely based on the perf improvement.

---

_Comment by @AlexWaygood on 2026-01-06 15:49_

> I think this is a bit more nuanced. Codspeed does report that performance regresses for some benchmarks. The memory improvement is nice but I expect it to be less (or total memory usage might even increase) if we reduce the metadata that salsa tracks, e.g. by [salsa-rs/salsa#1045](https://github.com/salsa-rs/salsa/pull/1045)
>
> Because of that, I would suggest we don't give too much weight to the memory improvement and make the merge decision solely based on the perf improvement.

For sure. The main point I wanted to make is that (unlike many instances where we add caches), this doesn't appear to lead to any memory-usage _regression_ (at least not right now).

There are only two reported performance regressions (vs seven reported improvements)... but I suppose it is a bit concerning that there is a 3% regression reported on the multithreaded benchmark. Most real-world uses of ty are going to invoke it with multithreading enabled, and the regression on that benchmark could indicate that the cache is leading to lock contention between threads, I suppose?

I guess it could be worth also running the ty_benchmark script to see if any regressions are reported there.

---
