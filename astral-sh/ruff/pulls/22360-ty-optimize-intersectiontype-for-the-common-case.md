```yaml
number: 22360
title: " [ty] Optimize IntersectionType for the common case of a single negated element (v2)"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: alex/optimize-intersections-single-neg-slice
created_at: 2026-01-03T17:40:01Z
updated_at: 2026-01-04T21:45:39Z
url: https://github.com/astral-sh/ruff/pull/22360
synced_at: 2026-01-12T15:57:48Z
```

#  [ty] Optimize IntersectionType for the common case of a single negated element (v2)

---

_@AlexWaygood_

## Summary

Similar to https://github.com/astral-sh/ruff/pull/22344, but uses a smallvec inside `IntersectionType` itself. This means that we need to implement fewer methods on the custom enum, but may result in slower `self.negative(db).contains(...)` checks for intersections. We'll see!

## Test Plan

Existing tests / mypy_primer


---

_Label `performance` added by @AlexWaygood on 2026-01-03 17:40_

---

_Label `ty` added by @AlexWaygood on 2026-01-03 17:40_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 17:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-03 17:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
urllib3 (https://github.com/urllib3/urllib3)
+ src/urllib3/response.py:808:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 266 diagnostics
+ Found 267 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job.py:167:24: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job.py:167:24: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None | (Divergent & ~Divergent)`
- dragonchain/job_processor/contract_job.py:198:60: error[not-iterable] Object of type `Unknown | None | (Divergent & ~AlwaysFalsy)` may not be iterable
+ dragonchain/job_processor/contract_job.py:198:60: error[not-iterable] Object of type `Unknown | None | (Divergent & ~Divergent)` may not be iterable
- dragonchain/job_processor/contract_job.py:250:29: error[invalid-argument-type] Argument to bound method `pull_image` is incorrect: Expected `str`, found `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job.py:250:29: error[invalid-argument-type] Argument to bound method `pull_image` is incorrect: Expected `str`, found `Unknown | None | (Divergent & ~Divergent)`
- dragonchain/job_processor/contract_job.py:327:23: error[not-iterable] Object of type `Unknown | None | (Divergent & ~AlwaysFalsy)` may not be iterable
+ dragonchain/job_processor/contract_job.py:327:23: error[not-iterable] Object of type `Unknown | None | (Divergent & ~Divergent)` may not be iterable
- dragonchain/job_processor/contract_job.py:419:13: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job.py:419:13: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~Divergent)`
- dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~Divergent)`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

apprise (https://github.com/caronc/apprise)
- apprise/plugins/email/base.py:525:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[(@Todo & ~AlwaysFalsy) | Literal[False], @Todo]`
+ apprise/plugins/email/base.py:525:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `tuple[(@Todo & ~@Todo) | Literal[False], @Todo]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

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
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2095 diagnostics
+ Found 2093 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14435 diagnostics
+ Found 14436 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2026-01-03 18:03_

The codspeed report here actually looks basically as good as the report on https://github.com/astral-sh/ruff/pull/22344. The biggest speedups aren't quite as big, but they're in basically the same range, and speedups are reported across a broader range of benchmarks: https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-intersections-single-neg-slice?utm_source=github&utm_medium=check&utm_content=details

---

_@MichaReiser reviewed on 2026-01-04 17:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14433 on 2026-01-04 17:12_

Is the assumption here and below that `normalize_impl` never returns the same `Type` (two types that are `Eq`) if the input types were different?

---

_@AlexWaygood reviewed on 2026-01-04 17:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14433 on 2026-01-04 17:20_

Hmm, not sure I understand the question.

A type can normalize to itself, and two types with different Salsa IDs can normalize to the same type. But I don't understand how that's relevant to this change.

This code is just mapping over an existing `SmallVec<[Type<'db>; 1]>`, normalizing each element in that smallvec, and collecting it back into a new `SmallVec<[Type<'db>; 1]>`. It's exactly the same as the existing code, except that the existing code maps over an `FxOrderSet` and collects the normalized elements into a new `FxOrderSet`.

---

_@MichaReiser reviewed on 2026-01-04 18:48_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:14433 on 2026-01-04 18:48_

> It's exactly the same as the existing code, except that the existing code maps over an FxOrderSet and collects the normalized elements into a new FxOrderSet.

The difference is that the small vec doesn't deduplicate normalized elements that are equal. My question is whether this is okay or if this is a concern

---

_@AlexWaygood reviewed on 2026-01-04 18:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:14433 on 2026-01-04 18:52_

Ahh, I see. Hmmm, good question. In theory, we _should_ be okay there, I think, because two types with different Salsa IDs would only normalize to the same type if they are equivalent types, and we shouldn't ever have two equivalent types in the same intersection. In practice... I wouldn't bet my life on us upholding those invariants _everywhere_, currently 😄

If we do come across that, I think it indicates a bug elsewhere, so I'm not sure we should worry about that here. But the fact that this is a question that must be asked is maybe another point in favour of https://github.com/astral-sh/ruff/pull/22344 over this PR

---

_Comment by @AlexWaygood on 2026-01-04 21:45_

It looks like we're probably going with https://github.com/astral-sh/ruff/pull/22344

---

_Closed by @AlexWaygood on 2026-01-04 21:45_

---
