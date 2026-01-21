```yaml
number: 22678
title: "[ty] Support solving generics involving PEP 695 type aliases"
type: pull_request
state: merged
author: bxff
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: fix/pep695-type-alias-solver
created_at: 2026-01-18T13:12:15Z
updated_at: 2026-01-21T22:34:41Z
url: https://github.com/astral-sh/ruff/pull/22678
synced_at: 2026-01-21T23:08:24Z
```

# [ty] Support solving generics involving PEP 695 type aliases

---

_@bxff_

## Summary

Fixes type variable inference when a PEP 695 type alias is used as a function parameter.

**Before:**
```python
type MyList[T] = list[T]
def head[T](my_list: MyList[T]) -> T: ...
reveal_type(head([1, 2]))  # Unknown
```

**After:** Correctly infers `int` by expanding `MyList[T]` to `list[T]` during solving.

Changes in `generics.rs`:
- **Early formal expansion**: Expand parameter type aliases immediately so underlying structure is visible for structural matching
- **Late actual expansion**: Expand argument type aliases after direct matching attempts to preserve alias identity for `reveal_type()` and literal promotion
- **Union induction**: Recursively match formal type against each union element to support nested aliases like `ListOfPairs[T] = list[Pair[T]]`

The early/late expansion asymmetry is intentional: formal needs structural visibility; actual needs identity preservation.

## Test Plan

- New tests in `aliases.md` covering simple, nested, and union alias cases
- All 313 existing mdtests pass

Closes https://github.com/astral-sh/ty/issues/1851

---

_Review requested from @carljm by @bxff on 2026-01-18 13:12_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-18 13:12_

---

_Review requested from @sharkdp by @bxff on 2026-01-18 13:12_

---

_Review requested from @dcreager by @bxff on 2026-01-18 13:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | dict[str, Any] | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 7 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 8 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2058 diagnostics
+ Found 2057 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-18 13:21_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-18 13:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:33_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-await` | 0 | 0 | 6 |
| `invalid-return-type` | 1 | 4 | 1 |
| `invalid-argument-type` | 1 | 2 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| **Total** | **4** | **6** | **7** |


**[Full report with detailed diff](https://c1fe87ca.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://c1fe87ca.ty-ecosystem-ext.pages.dev/timing))



---

_Renamed from "[ty] Support solving generics involving PEP 695 type aliases (#1851)" to "[ty] Support solving generics involving PEP 695 type aliases" by @AlexWaygood on 2026-01-18 16:22_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:2085 on 2026-01-21 02:28_

This seems unrelated to the type alias change? Handling of unions is a known limitation of the current constraint solver, I don't think it should be addressed in this PR.

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1761 on 2026-01-21 02:31_

I'd prefer if we put this branch into the match statement directly (as the first branch, if necessary).

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:2078 on 2026-01-21 02:32_

```suggestion
            // Expand type aliases in the actual type.
            //
            // This is placed at the end of the match block to avoid expanding the type alias
            // when it can be matched directly against a type variable in the formal type,
            // e.g., `reveal_type(alias)` should reveal the type alias, not its value type.
```

---

_@ibraheemdev reviewed on 2026-01-21 02:33_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-21 08:30_

---

_Review requested from @ibraheemdev by @bxff on 2026-01-21 14:24_

---

_@ibraheemdev reviewed on 2026-01-21 21:26_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:351 on 2026-01-21 21:26_

Can we also add tests for the reverse direction, e.g.,
```py
type MyList[T] = list[T]

def head[T](list: list[T]) -> T:
    return list[0]

def _(x: MyList[int]):
    reveal_type(head(x))  # revealed: int
```

This test fails on main.

---

_Comment by @ibraheemdev on 2026-01-21 21:29_

Interestingly there seems to be no ecosystem impact (I believe most of the changes are spurious), but this should be good to land after the added test.  A rebase should also fix the failing benchmark job.


---

_@bxff reviewed on 2026-01-21 21:31_

---

_Review comment by @bxff on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:351 on 2026-01-21 21:31_

Done

---

_Comment by @ibraheemdev on 2026-01-21 22:34_

Thanks!

---

_Merged by @ibraheemdev on 2026-01-21 22:34_

---

_Closed by @ibraheemdev on 2026-01-21 22:34_

---
