```yaml
number: 22586
title: "[ty] Add support for dynamic dataclasses via `make_dataclass`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/functional-dict
created_at: 2026-01-14T22:20:58Z
updated_at: 2026-01-15T14:13:32Z
url: https://github.com/astral-sh/ruff/pull/22586
synced_at: 2026-01-15T14:51:20Z
```

# [ty] Add support for dynamic dataclasses via `make_dataclass`

---

_@charliermarsh_

## Summary

Like #22327, but for dataclasses.


---

_Label `ty` added by @charliermarsh on 2026-01-14 22:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Typing conformance

No changes



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/experimental/pydantic/error_type.py:149:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/experimental/pydantic/object_type.py:255:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 348 diagnostics
+ Found 350 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1824 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2060 diagnostics
+ Found 2059 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @carljm by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-15 02:58_

---

_Converted to draft by @charliermarsh on 2026-01-15 02:59_

---

_Marked ready for review by @charliermarsh on 2026-01-15 03:22_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-15 03:59_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-15 09:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 1 | 7 |
| `invalid-argument-type` | 2 | 1 | 3 |
| `invalid-assignment` | 0 | 0 | 5 |
| `unused-ignore-comment` | 2 | 2 | 0 |
| **Total** | **5** | **4** | **15** |


**[Full report with detailed diff](https://954a6c82.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://954a6c82.ty-ecosystem-ext.pages.dev/timing))



---

_Closed by @AlexWaygood on 2026-01-15 13:33_

---

_Reopened by @AlexWaygood on 2026-01-15 13:34_

---

_Comment by @AlexWaygood on 2026-01-15 13:46_

I'm a _bit_ hesitant about this one. The typing spec doesn't say anything about this function AFAIK, and I don't remember seeing any user requests on the issue tracker for us to add special-cased support. Mypy_primer only shows two diagnostics going away in the ecosystem, on `strawberry` -- and it looks like those lines already have `type: ignore`s on them for mypy.

Meanwhile, this is _quite_ a bit of new code, and the manual parsing of arguments in `infer/builder.rs` is quite complicated and error-prone.

Do any other type checkers have special-cased support for this function? We do have to draw the line _somewhere_.

I think I'd feel differently if we could share more of the call-expression-parsing logic with what we've already added for namedtuples (which definitely _was_ necessary), but the schema `make_dataclass()` expects is _just_ different enough that sharing more of the code seeems like it could be pretty difficult?

Another -- much simpler -- way to avoid false positives with the objects returned by this function would be to just special-case the return type so that we infer `type[Unknown]` rather than `type` -- that wouldn't involve any custom call-expression parsing.

---

_Comment by @charliermarsh on 2026-01-15 13:59_

AFAICT, Mypy and Pyright do not support it. But... I would still advocate for us to support it? We have all the infrastructure to do so! And almost all of the new code is in argument parsing -- which is very verbose, but at least fairly mechanical and testable? (E.g., it's much easier to know when we have that right, as opposed to something deep in type inference.) I think I don't agree that _this_ is where we should draw the line, unless it's a feature that we can't support for reasons that I don't yet understand or would be uncovered by your review.


---

_Comment by @AlexWaygood on 2026-01-15 14:01_

(I'm curious for @carljm's and @dcreager's opinions!)

---

_Comment by @charliermarsh on 2026-01-15 14:08_

Ditto and am totally fine to be overruled on it of course. I took it as a given that we'd support this given that it's in the [type system overview](https://github.com/astral-sh/ty/issues/1889); that may have been a mistake!

---

_Comment by @AlexWaygood on 2026-01-15 14:10_

> Ditto and am totally fine to be overruled on it of course. I took it as a given that we'd support this given that it's in the [type system overview](https://github.com/astral-sh/ty/issues/1889); that may have been a mistake!

Well, that issue just says that we don't have any tests for the function right now. But this PR adds a lot more than just test coverage 😆

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:7616 on 2026-01-15 14:12_

Can we match on the elements here. The same in other positions
```suggestion
                    [name_expr, type_expr] => {
```

---

_@MichaReiser reviewed on 2026-01-15 14:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:5817 on 2026-01-15 14:13_

It seems unfortunate that we have to repeat all those methods for every dynamic class literal. Can't we share more infrastructure?

---

_@MichaReiser reviewed on 2026-01-15 14:13_

---
