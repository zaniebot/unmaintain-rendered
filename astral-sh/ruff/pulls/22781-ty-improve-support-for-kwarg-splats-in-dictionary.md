```yaml
number: 22781
title: "[ty] Improve support for kwarg splats in dictionary literals"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/dict-splat
created_at: 2026-01-21T02:16:04Z
updated_at: 2026-01-21T21:35:35Z
url: https://github.com/astral-sh/ruff/pull/22781
synced_at: 2026-01-21T22:07:17Z
```

# [ty] Improve support for kwarg splats in dictionary literals

---

_@ibraheemdev_

Resolves https://github.com/astral-sh/ty/issues/1332.

---

_Label `ty` added by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @carljm by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-21 02:16_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-21 02:16_

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyjwt (https://github.com/jpadilla/pyjwt)
- jwt/api_jws.py:50:13: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `options` of type `SigOptions`
+ jwt/api_jws.py:50:13: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | bool]` is not assignable to attribute `options` of type `SigOptions`
- jwt/api_jwt.py:83:16: error[invalid-return-type] Return type does not match returned value: expected `FullOptions`, found `dict[Unknown, Unknown]`
+ jwt/api_jwt.py:83:16: error[invalid-return-type] Return type does not match returned value: expected `FullOptions`, found `dict[Unknown | str, Unknown]`

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/kcidb/test_validate.py:200:40: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | str | dict[Unknown | str, Unknown | int | list[Unknown | dict[Unknown | str, Unknown | str]] | ... omitted 3 union elements]`
- Found 242 diagnostics
+ Found 243 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_core_metadata.py:87:54: error[invalid-assignment] Invalid assignment to key "pydantic_js_extra" with declared type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | ((dict[str, Divergent], type[Any], /) -> None)` on TypedDict `CoreMetadata`: value of type `dict[object, object]`
- Found 3155 diagnostics
+ Found 3156 diagnostics

meson (https://github.com/mesonbuild/meson)
+ docs/refman/loaderyaml.py:205:47: error[invalid-argument-type] Argument is incorrect: Expected `Type`, found `Unknown | str`
+ docs/refman/loaderyaml.py:212:47: error[invalid-argument-type] Argument is incorrect: Expected `Type`, found `Unknown | str`
+ docs/refman/loaderyaml.py:218:30: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `Type`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
- Found 2168 diagnostics
+ Found 2177 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/tests/test_build_py.py:424:15: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]] | set[Unknown | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]]`
+ setuptools/tests/test_build_py.py:438:15: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]] | set[Unknown | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]]`
- Found 1131 diagnostics
+ Found 1133 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:2181:82: error[invalid-assignment] Object of type `object` is not assignable to `((str, /) -> bool) | Collection[str | int]`
- Found 521 diagnostics
+ Found 522 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | dict[str, Any] | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any] | int | T@resolve_block_document_references | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 7 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 7 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | IndexHierarchy | TypeBlocks | ... omitted 8 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 8 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 7 union elements, object_]`

materialize (https://github.com/MaterializeInc/materialize)
- misc/python/materialize/mzcompose/services/kafka.py:66:27: error[invalid-argument-type] Invalid argument to key "depends_on" with declared type `list[str] | dict[str, ServiceDependency]` on TypedDict `ServiceConfig`: value of type `dict[str | Unknown, ServiceDependency | Unknown | dict[Unknown | str, Unknown | str]]`
- misc/python/materialize/mzcompose/services/schema_registry.py:45:27: error[invalid-argument-type] Invalid argument to key "depends_on" with declared type `list[str] | dict[str, ServiceDependency]` on TypedDict `ServiceConfig`: value of type `dict[str | Unknown, ServiceDependency | Unknown | dict[Unknown | str, Unknown | str]]`
- Found 509 diagnostics
+ Found 507 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/io/formats/style.py:2828:27: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | None | dict[Unknown | str, Unknown | str]`
- Found 3748 diagnostics
+ Found 3749 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2058 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:294:32: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | int | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str]`
+ homeassistant/components/risco/config_flow.py:294:32: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | bool | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str] | Divergent`
- homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:302:23: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | int | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str]`
+ homeassistant/components/risco/config_flow.py:302:23: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | bool | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str] | Divergent`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, int | _R@ignore_variance | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14469 diagnostics
+ Found 14470 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:21_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 15 | 4 | 3 |
| `invalid-return-type` | 5 | 0 | 5 |
| `invalid-assignment` | 2 | 0 | 6 |
| `invalid-await` | 2 | 0 | 6 |
| `possibly-missing-attribute` | 3 | 0 | 3 |
| `unused-ignore-comment` | 2 | 1 | 0 |
| `not-subscriptable` | 0 | 0 | 2 |
| `unresolved-attribute` | 0 | 0 | 2 |
| **Total** | **29** | **5** | **27** |


**[Full report with detailed diff](https://8ba7b997.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://8ba7b997.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @MichaReiser on 2026-01-21 07:56_

Huh, the ecosystem changes suggest that there are about 100 new diagnostics for prefect, but the ecosystem analyzer only reports 16? 

---

_Comment by @AlexWaygood on 2026-01-21 07:58_

> Huh, the ecosystem changes suggest that there are about 100 new diagnostics for prefect, but the ecosystem analyzer only reports 16?

Prefect is one of our most non-deterministic repos currently. I think those would all go away if you reran mypy_primer ☹️

---

_Comment by @AlexWaygood on 2026-01-21 08:16_

E.g. You see the same prefect diagnostics going away on https://github.com/astral-sh/ruff/pull/22067#issuecomment-3673257468

<details>
<summary>Screenshot</summary>

![IMG_1479](https://github.com/user-attachments/assets/1256abce-0521-4aee-ba38-5564c28e512a)

</details>


---

_Comment by @MichaReiser on 2026-01-21 08:17_

Wow, so it's getting worse? I mean, 100 non-deterministic diagnostics that don't just differ by text sounds unusable to me

---

_Comment by @AlexWaygood on 2026-01-21 08:22_

I wouldn't say it's got worse recently, but yes, it's been very bad since https://github.com/astral-sh/ruff/pull/21551 landed and hasn't got better since. It only happens on specific repos and most of the time it does not manifest as 100 new diagnostics coming or going. Prefect is by far our most non-deterministic repo. I've been rerunning primer often to check if diagnostic changes persist when they're on known flaky repos and I'm unsure whether they're our regular flakes or not.

---

_@dhruvmanila reviewed on 2026-01-21 09:05_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:3593 on 2026-01-21 09:05_

Should this TODO be moved to the new function `unpack_keys_and_items`?

---

_@AlexWaygood reviewed on 2026-01-21 09:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:3593 on 2026-01-21 09:08_

I'm not sure I agree with the todo 😆 I think the current way of doing it is fine and may be more efficient and/or less code even when we support generic protocols in the generics solver 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:10070 on 2026-01-21 11:20_

do we not want to pass in the type context of the outer mapping when inferring the type of the inner mapping here?

---

_@AlexWaygood approved on 2026-01-21 11:21_

Code looks good (with one question); haven't looked at the ecosystem report though

---

_Comment by @ibraheemdev on 2026-01-21 21:35_

The relevant ecosystem changes all look good to me. Some instances of https://github.com/astral-sh/ty/issues/1248, but those are unrelated.

---

_Merged by @ibraheemdev on 2026-01-21 21:35_

---

_Closed by @ibraheemdev on 2026-01-21 21:35_

---

_Branch deleted on 2026-01-21 21:35_

---
