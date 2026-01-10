```yaml
number: 22344
title: "[ty] Optimize `IntersectionType` for the common case of a single negated element"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-intersections-single-neg
created_at: 2026-01-02T19:10:06Z
updated_at: 2026-01-05T15:27:29Z
url: https://github.com/astral-sh/ruff/pull/22344
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Optimize `IntersectionType` for the common case of a single negated element

---

_Pull request opened by @AlexWaygood on 2026-01-02 19:10_

## Summary

We call `Type::negate()` a lot across the codebase. In most cases, it creates an intersection type with 0 positive elements and a single negative element.

Our representation of `IntersectionType` is currently not very well optimised for this very common case, however: no matter how many negative elements are in the intersection, we always use an `FxOrderSet` to store these negative elements. I believe that, like most generic Rust collections, `OrderSet` does not allocate if it has 0 elements contained inside it, but performs a heap allocation as soon as a single element is allocated.

This PR gets rid of the heap allocation for the common case of a single negative element. This leads to across-the-board speedups of 1-4%, including a 4-5% speedup on colour-science, one of the benchmarks on which we currently do worst.

## Test Plan

Existing tests all pass and the primer report is clean (except for the known set of flakes).


---

_Label `performance` added by @AlexWaygood on 2026-01-02 19:10_

---

_Label `ty` added by @AlexWaygood on 2026-01-02 19:10_

---

_Label `performance` added by @AlexWaygood on 2026-01-02 19:10_

---

_Label `ty` added by @AlexWaygood on 2026-01-02 19:10_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 19:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-02 19:12_


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
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5153 diagnostics
+ Found 5156 diagnostics

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

_Comment by @codspeed-hq[bot] on 2026-01-02 19:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-intersections-single-neg?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22344 will **not alter performance**

<sub>Comparing <code>alex/optimize-intersections-single-neg</code> (e1cbb79) with <code>main</code> (24dd149)</sub>



### Summary

`✅ 23` untouched  
`⏩ 30` skipped[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-intersections-single-neg?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @AlexWaygood on 2026-01-02 19:49_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-02 19:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-02 19:49_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-02 19:49_

---

_Comment by @AlexWaygood on 2026-01-03 10:10_

https://github.com/astral-sh/ruff/pull/22353 applies the same optimisation to `IntersectionType::positive()`. It _might_ provide an additional speedup on top of the changes in this PR. But the effect of applying the optimisation to `IntersectionType::positive` looks to be much smaller than the effect of applying it to `IntersectionType::negative`, and the change adds further complexity. So I'd prefer to consider that as a standalone change after this lands (if this PR is accepted).

---

_@MichaReiser reviewed on 2026-01-03 16:52_

Nice find. 

I would be interested in exploring if we can use a `Box<[Type<'db>]>` in `IntersectionType` either by:

* Use a dedicated union with a `Slice` (empty or more than one element) and `Single` (exactly one element)
* Use a `SmallVec` instead

The advantage of using a slice is that it doesn't increase the complexity of the read methods (they can all be abstracted by implementing `Deref` where the `Target` is a `[T]` (use `std::slice::from_ref` to create a slice from a single element). The ordered set can be converted into a boxed slice using https://docs.rs/ordermap/latest/ordermap/set/struct.OrderSet.html#method.into_boxed_slice (which I believe is as expensive as calling `Vec::into_boxed_slice`)).


The `IntersectionBuilder` would keep your existing enum

---

_Comment by @AlexWaygood on 2026-01-03 17:10_

> I would be interested in exploring if we can use a `Box<[Type<'db>]>` in `IntersectionType`

I think this might degrade the performance of `intersection.negative(db).contains(...)`, which is called in some very hot methods. But I can try it!

---

_Comment by @MichaReiser on 2026-01-03 17:35_

> I think this might degrade the performance of intersection.negative(db).contains(...), which is called in some very hot methods. But I can try it!

I also just noticed that `into_boxed_slice` doesn't return `&[T]` but a custom `Slice` type... So what I suggested won't work 

---

_Review requested from @MichaReiser by @MichaReiser on 2026-01-03 17:35_

---

_Comment by @AlexWaygood on 2026-01-03 17:36_

> I also just noticed that `into_boxed_slice` doesn't return `&[T]` but a custom `Slice` type... So what I suggested won't work

right. But I'm trying out your suggestion of a smallvec, just for comparison.

---

_Comment by @MichaReiser on 2026-01-03 17:38_

> right. But I'm trying out your suggestion of a smallvec, just for comparison.

Doesn't that mean that we need to copy (and create a new allocation) for the *many elements* case?

---

_Comment by @AlexWaygood on 2026-01-03 17:40_

> Doesn't that mean that we need to copy (and create a new allocation) for the _many elements_ case?

Yes. Which is why I didn't go for that in this PR 😄

---

_Comment by @AlexWaygood on 2026-01-03 18:01_

The codspeed report on the smallvec version does actually seem to indicate a similar level of speedup, and it seems to indicate that a broader range of benchmarks benefit? https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-intersections-single-neg-slice?utm_source=github&utm_medium=check&utm_content=details

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14629 on 2026-01-04 19:19_

The duplication here is a bit unfortunate and it spills store logic into the intersection logic. Should we add a `map` and `try_map` (early returns if the mapping function returns `None`)?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14390 on 2026-01-04 19:21_

```suggestion
/// To avoid unnecessary allocations for the common case of 1 negative elements,
/// we use this enum to represent the negative elements of an intersection type.
```

Empty sets don't require any allocations

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14430 on 2026-01-04 19:22_

I suggest making this a struct comment instead of repeating it multiple times across the implementation. That also gives you an opportunity to explain why we don't do it (because it's all about avoiding allocations)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14454 on 2026-01-04 19:28_

Can we implement the `GetSize` trait instead

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14393 on 2026-01-04 19:32_

This might be tricky but we'll need to implement `PartialEq`, `Eq` and `Hash` manually so that `Empty` compares equal to `Multiple(empty set)` and `Single` compares equal to `Multiple(set_containing_one_element)`

---

_@MichaReiser reviewed on 2026-01-04 19:33_

This looks good. But I think we have to manually implement `PartialEq`, `Eq` and `Hash`

---

_@AlexWaygood reviewed on 2026-01-04 19:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14393 on 2026-01-04 19:34_

Good spot.

---

_@AlexWaygood reviewed on 2026-01-04 20:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14629 on 2026-01-04 20:28_

I think this would lead to more code overall because:
- We would need two new methods on `NegativeIntersectionElements` (`map` and `try_map`) -- one for this method and one for `recursive_type_normalized_impl`
- The new methods would not be able to reuse the `normalized_set` and `opt_normalized_set` helpers that we have already in `normalized_impl` and `recursive_type_normalized_impl`

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-04 20:29_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14605 on 2026-01-05 08:48_

I'm not sure if it's less efficient but I would implement this by
```suggestion
        self.len() == other.len() && std::iter::eq(self.iter(), other.iter())
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14621 on 2026-01-05 08:49_

Same as `eq`, I would implement this using the same implementation as `IndexSet`:

```rust
				self.len().hash(state);
        for value in self {
            value.hash(state);
        }
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14629 on 2026-01-05 08:52_

> We would need two new methods on NegativeIntersectionElements (map and try_map) -- one for this method and one for recursive_type_normalized_impl

But that's not different from the duplication in normalize.

> The new methods would not be able to reuse the normalized_set and opt_normalized_set helpers that we have already in normalized_impl and recursive_type_normalized_impl

I think we can avoid that by first mapping and then calling `sort_unstable_by`. It might suggest that this should be a method on `NegativeIntersectionElements` and not on `IntersectionType`

---

_@MichaReiser reviewed on 2026-01-05 08:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14629 on 2026-01-05 13:00_

I think https://github.com/astral-sh/ruff/pull/22344/commits/a293de809794ad669e7adda8e99a8e8ac9f4c584 is what you're asking for here. It's only 3 lines more code overall, but it does feel more repetitive to me. I still prefer the PR without that change, personally.

---

_@AlexWaygood reviewed on 2026-01-05 13:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-05 13:05_

---

_@MichaReiser reviewed on 2026-01-05 13:09_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14629 on 2026-01-05 13:09_

This is what I had in mind (but keeping `normalized_impl` where it was before)

```patch
Index: crates/ty_python_semantic/src/types.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
--- a/crates/ty_python_semantic/src/types.rs	(revision a293de809794ad669e7adda8e99a8e8ac9f4c584)
+++ b/crates/ty_python_semantic/src/types.rs	(date 1767618499646)
@@ -14525,22 +14525,41 @@
         }
     }
 
-    fn normalized_impl(&self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
+    fn map<F>(&self, mut map: F) -> Self
+    where
+        F: FnMut(&Type<'db>) -> Type<'db>,
+    {
         match self {
             NegativeIntersectionElements::Empty => NegativeIntersectionElements::Empty,
             NegativeIntersectionElements::Single(ty) => {
-                NegativeIntersectionElements::Single(ty.normalized_impl(db, visitor))
+                NegativeIntersectionElements::Single(map(ty))
+            }
+            NegativeIntersectionElements::Multiple(set) => {
+                NegativeIntersectionElements::Multiple(set.into_iter().map(map).collect())
+            }
+        }
+    }
+
+    fn try_map<F>(&self, mut map: F) -> Option<Self>
+    where
+        F: FnMut(&Type<'db>) -> Option<Type<'db>>,
+    {
+        Some(match self {
+            NegativeIntersectionElements::Empty => NegativeIntersectionElements::Empty,
+            NegativeIntersectionElements::Single(ty) => {
+                NegativeIntersectionElements::Single(map(ty)?)
             }
             NegativeIntersectionElements::Multiple(set) => {
-                let mut set: FxOrderSet<Type<'db>> = set
-                    .iter()
-                    .map(|ty| ty.normalized_impl(db, visitor))
-                    .collect();
-
-                set.sort_unstable_by(|l, r| union_or_intersection_elements_ordering(db, l, r));
-                NegativeIntersectionElements::Multiple(set)
+                let set: Option<FxOrderSet<_>> = set.into_iter().map(map).collect();
+                NegativeIntersectionElements::Multiple(set?)
             }
-        }
+        })
+    }
+
+    fn normalized_impl(&self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
+        let mut normalized = self.map(|ty| ty.normalized_impl(db, visitor));
+        normalized.sort_unstable_by(|l, r| union_or_intersection_elements_ordering(db, l, r));
+        normalized
     }
 
     fn recursive_type_normalized_impl(
@@ -14549,33 +14568,17 @@
         div: Type<'db>,
         nested: bool,
     ) -> Option<Self> {
-        match self {
-            NegativeIntersectionElements::Empty => Some(NegativeIntersectionElements::Empty),
-            NegativeIntersectionElements::Single(ty) => {
-                let ty = if nested {
-                    ty.recursive_type_normalized_impl(db, div, nested)?
-                } else {
-                    ty.recursive_type_normalized_impl(db, div, nested)
-                        .unwrap_or(div)
-                };
-                Some(NegativeIntersectionElements::Single(ty))
-            }
-            NegativeIntersectionElements::Multiple(set) => {
-                let set = if nested {
-                    set.iter()
-                        .map(|ty| ty.recursive_type_normalized_impl(db, div, nested))
-                        .collect::<Option<FxOrderSet<Type<'db>>>>()?
-                } else {
-                    set.iter()
-                        .map(|ty| {
-                            ty.recursive_type_normalized_impl(db, div, nested)
-                                .unwrap_or(div)
-                        })
-                        .collect()
-                };
-                Some(NegativeIntersectionElements::Multiple(set))
-            }
-        }
+        let normalized = if nested {
+            self.try_map(|ty| ty.recursive_type_normalized_impl(db, div, nested))?
+        } else {
+            self.map(|ty| {
+                ty.recursive_type_normalized_impl(db, div, nested)
+                    .unwrap_or(div)
+            })
+        };
+
+        // Interesting, we don't sort here???
+        Some(normalized)
     }
 }
 
```

I find this does a better job at abstracting how we represent the negated elements internally.

---

_@AlexWaygood reviewed on 2026-01-05 13:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14629 on 2026-01-05 13:24_

Ah, that _is_ nice, thanks! Applied.

---

_@MichaReiser approved on 2026-01-05 13:26_

---

_Merged by @AlexWaygood on 2026-01-05 13:41_

---

_Closed by @AlexWaygood on 2026-01-05 13:41_

---

_Branch deleted on 2026-01-05 13:41_

---

_Comment by @AlexWaygood on 2026-01-05 15:27_

> #22353 applies the same optimisation to `IntersectionType::positive()`. It _might_ provide an additional speedup on top of the changes in this PR. But the effect of applying the optimisation to `IntersectionType::positive` looks to be much smaller than the effect of applying it to `IntersectionType::negative`, and the change adds further complexity. So I'd prefer to consider that as a standalone change after this lands (if this PR is accepted).

I rebased #22353 on `main` after this landed, and the performance impact was not impressive: https://github.com/astral-sh/ruff/pull/22353#issuecomment-3710891185. So it looks like this is best kept as only an optimisation for `IntersectionType::negative`.

---
