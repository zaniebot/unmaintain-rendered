```yaml
number: 22687
title: "[ty] Avoid overload errors when detecting dataclass-on-tuple"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/data-decorator
created_at: 2026-01-18T19:34:12Z
updated_at: 2026-01-19T13:55:12Z
url: https://github.com/astral-sh/ruff/pull/22687
synced_at: 2026-01-19T14:36:03Z
```

# [ty] Avoid overload errors when detecting dataclass-on-tuple

---

_@charliermarsh_

## Summary

Fixes some TODOs introduced in #22672 around cases like the following:


```python
from collections import namedtuple
from dataclasses import dataclass

NT = namedtuple("NT", "x y")

# error: [invalid-dataclass] "Cannot use `dataclass()` on a `NamedTuple` class"
dataclass(NT)
```

On main, `dataclass(NT)` emits `# error: [no-matching-overload]` instead, which is wrong -- the overload does match! On main, the logic proceeds as follows:

1. `dataclass` has two overloads:
  - `dataclass(cls: type[_T], ...) -> type[_T]`
  - `dataclass(cls: None = None, ...) -> Callable[[type[_T]], type[_T]]`
2. When `dataclass(NT)` is called:
  - Arity check: Both overloads accept one positional argument, so both pass.
  - Type checking on first overload: `NT` matches `type[_T]`... but then `invalid_dataclass_target()` runs and adds `InvalidDataclassApplication` error
  - Type checking on second overload: `NT` doesn't match `None`, so we have a type error.
3. After type checking, both overloads have errors.
4. `matching_overload_index()` filters by `overload.as_result().is_ok()`, which checks if `errors.is_empty()`. Since both overloads have errors, neither matches...
5. We emit the "No overload matches arguments" error.

Instead, we now differentiate between non-matching errors, and errors that occur when we _do_ match, but the call has some other semantic failure.


---

_Review requested from @dhruvmanila by @charliermarsh on 2026-01-18 19:34_

---

_Label `ty` added by @charliermarsh on 2026-01-18 19:34_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 19:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 19:37_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

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
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5411 diagnostics
+ Found 5406 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1821 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14497 diagnostics
+ Found 14498 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@charliermarsh reviewed on 2026-01-18 19:39_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/call/bind.rs`:2450 on 2026-01-18 19:39_

I... _think_ this was impossible on main? Because if we had exactly one matching overload without errors, we'd never call `report_diagnostics`? But now, we _can_ have one overload that matches but still emits an error.

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-18 19:40_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 19:46_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-return-type` | 0 | 1 | 6 |
| `invalid-argument-type` | 0 | 1 | 4 |
| `invalid-assignment` | 0 | 0 | 5 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **7** | **25** |


**[Full report with detailed diff](https://c1dd5435.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://c1dd5435.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @charliermarsh on 2026-01-18 19:57_

So in `aioredis-py`, we now show this:

```
error[invalid-argument-type]: Argument to bound method `split` is incorrect
    --> aioredis-py/aioredis/client.py:3898:53
     |
3896 |         if isinstance(response, bytes):
3897 |             response = self.connection.encoder.decode(response, force=True)
3898 |         command_time, command_data = response.split(" ", 1)
     |                                                     ^^^ Expected `Buffer | None`, found `Literal[" "]`
3899 |         m = self.monitor_re.match(command_data)
3900 |         if m is None:
     |
info: Method defined here
    --> stdlib/builtins.pyi:1761:9
     |
1759 |         """
1760 |
1761 |     def split(self, sep: ReadableBuffer | None = None, maxsplit: SupportsIndex = -1) -> list[bytes]:
     |         ^^^^^       --------------------------------- Parameter declared here
1762 |         """Return a list of the sections in the bytes, using sep as the delimiter.
```

But _not_ the following:

```
error[no-matching-overload]: No overload of bound method `split` matches arguments
    --> aioredis-py/aioredis/client.py:3898:38
     |
3896 |         if isinstance(response, bytes):
3897 |             response = self.connection.encoder.decode(response, force=True)
3898 |         command_time, command_data = response.split(" ", 1)
     |                                      ^^^^^^^^^^^^^^^^^^^^^^
3899 |         m = self.monitor_re.match(command_data)
3900 |         if m is None:
     |
info: First overload defined here
    --> stdlib/builtins.pyi:1276:9
     |
1274 |     def rstrip(self, chars: str | None = None, /) -> str: ...  # type: ignore[misc]
1275 |     @overload
1276 |     def split(self: LiteralString, sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString]:
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1277 |         """Return a list of the substrings in the string, using sep as the separator string.
     |
info: Possible overloads for bound method `split`:
info:   (self: LiteralString, sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString]
info:   (self, sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]
info: Union variant `Overload[(sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString], (sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]]` is incompatible with this call site
info: Attempted to call union type `Unknown | (bound method bytes.split(sep: Buffer | None = None, maxsplit: SupportsIndex = -1) -> list[bytes]) | (Overload[(sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString], (sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]])`
info: rule `no-matching-overload` is enabled by default
```

On main, we show both.


---

_Comment by @charliermarsh on 2026-01-18 20:05_

I think that's actually... _correct_? Consider `x: str | bytes` and then `x.split(" ")`. Because we have a union, and at least one variant errors (`bytes` expects a `Buffer`, not a `str`), we call `binding.report_diagnostics` for each variant... For the `str` variant, it has two overloads that both match arity, but only one actually matches the signature... So `matching_overload_before_type_checking` is `None` (because they both match arity), but we don't actually have an error, and we fall through to `NO_MATCHING_OVERLOAD`.

In this branch, we have `if let MatchingOverloadIndex::Single(matching_overload_index)`, so we select the "valid" variant and it has no diagnostics. Arguably we shouldn't be in `binding.report_diagnostics` at all though for `str`?

---

_Comment by @charliermarsh on 2026-01-18 20:28_

I am going to see if I can fix that separately [here](https://github.com/astral-sh/ruff/pull/22688).

---

_Comment by @charliermarsh on 2026-01-18 21:00_

I moved that change into https://github.com/astral-sh/ruff/pull/22688.

---

_Marked ready for review by @charliermarsh on 2026-01-18 21:40_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 21:40_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 21:40_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 21:40_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 21:40_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2277 on 2026-01-19 06:00_

These errors would be "semantic" errors as defined in `affects_overload_resolution`, right? If so, the comment could be updated

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:3942 on 2026-01-19 06:07_

The `as_result` name for this method seems a bit misleading now that we don't account for all errors here (only the ones that affect overload resolution), maybe we should rename this? e.g., `has_errors_affecting_overload_resolution` (too long), `failed_overload_resolution`, etc. This method is also only called in `matching_overloads` and `matching_overloads_mut` so it seems ok to rename it to be specific to this usage.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2450 on 2026-01-19 06:10_

Yeah, I think that's correct, I reached the same conclusion in https://github.com/astral-sh/ruff/pull/22688#discussion_r2703241568.

I think the body of this if branch is same as the one right above it, so both of these can be merged? Edit: Oh, merging that might be difficult but maybe we could extract out the logic into an inner function.

---

_@dhruvmanila approved on 2026-01-19 06:12_

---

_Merged by @charliermarsh on 2026-01-19 13:55_

---

_Closed by @charliermarsh on 2026-01-19 13:55_

---

_Branch deleted on 2026-01-19 13:55_

---
