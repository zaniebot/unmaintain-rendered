```yaml
number: 22356
title: "[ty] Never add elements to a `UnionBuilder` before setting `recursively_defined`"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/union-filter-cycle-recovery
created_at: 2026-01-03T14:00:26Z
updated_at: 2026-01-03T17:20:05Z
url: https://github.com/astral-sh/ruff/pull/22356
synced_at: 2026-01-12T15:57:47Z
```

# [ty] Never add elements to a `UnionBuilder` before setting `recursively_defined`

---

_@AlexWaygood_

## Summary

In several places, we set `UnionBuilder::recursively_defined()` only after adding many elements to that union builder. But the `recursively_defined` field is only respected in `UnionBuilder` methods such as `UnionBuilder::add_in_place_impl`; setting `recursively_defined` _after_ adding all the elements to the builder has no effect at all. There are similar issues with the various other union-builder settings such as `order_elements`, `cycle_recovery`, etc.

This PR fixes these bugs by adding a required `settings` parameter to `UnionBuilder::new()`.

## Test Plan

TODO.

From eyeballing the current code, it sure _looks_ buggy, but I'm not yet sure how that bug can actually manifest when checking real Python code...


---

_Label `ty` added by @AlexWaygood on 2026-01-03 14:00_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 14:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-03 17:10:45.232600215 +0000
+++ new-output.txt	2026-01-03 17:10:45.954598946 +0000
@@ -1,3 +1,4 @@
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/309c249/src/function/execute.rs:593:9 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(7430)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -68,23 +69,6 @@
 aliases_type_statement.py:82:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias3`
 aliases_type_statement.py:88:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias6`
 aliases_type_statement.py:89:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias7`
-aliases_typealiastype.py:32:7: error[unresolved-attribute] Object of type `TypeAliasType` has no attribute `other_attrib`
-aliases_typealiastype.py:52:40: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_typealiastype.py:53:40: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:54:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
-aliases_typealiastype.py:54:43: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:55:42: error[invalid-type-form] List comprehensions are not allowed in type expressions
-aliases_typealiastype.py:56:42: error[invalid-type-form] Dict literals are not allowed in type expressions
-aliases_typealiastype.py:57:42: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_typealiastype.py:58:42: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
-aliases_typealiastype.py:58:48: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_typealiastype.py:59:42: error[invalid-type-form] `if` expressions are not allowed in type expressions
-aliases_typealiastype.py:60:42: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
-aliases_typealiastype.py:61:42: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
-aliases_typealiastype.py:62:42: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_typealiastype.py:63:42: error[invalid-type-form] Boolean operations are not allowed in type expressions
-aliases_typealiastype.py:64:42: error[invalid-type-form] F-strings are not allowed in type expressions
-aliases_typealiastype.py:66:47: error[unresolved-reference] Name `BadAlias21` used when not defined
 annotations_forward_refs.py:47:10: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
@@ -1020,4 +1004,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1022 diagnostics
+Found 1006 diagnostics
+WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-03 14:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/topology_description.py:611:51: error[unsupported-operator] Operator `<` is not supported between two objects of type `tuple[int | None, ObjectId | None]`
+ pymongo/topology_description.py:611:51: error[unsupported-operator] Operator `<` is not supported between objects of type `tuple[int | None, ObjectId | None]` and `tuple[int | None, ObjectId | None]`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/commands/telescope.py:146:12: error[unsupported-operator] Operator `>` is not supported between two objects of type `int | None`
+ pwndbg/commands/telescope.py:146:12: error[unsupported-operator] Operator `>` is not supported between objects of type `int | None` and `int | None`
- pwndbg/commands/telescope.py:158:22: error[unsupported-operator] Operator `-` is not supported between two objects of type `int | None`
+ pwndbg/commands/telescope.py:158:22: error[unsupported-operator] Operator `-` is not supported between objects of type `int | None` and `int | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown] | AwsCredentials`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown] | AwsCredentials`
- src/integrations/prefect-aws/tests/test_s3.py:1141:24: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `credentials`
+ src/integrations/prefect-aws/tests/test_s3.py:1141:24: warning[possibly-missing-attribute] Attribute `credentials` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:30:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:40:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-email/tests/test_credentials.py:91:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
- src/integrations/prefect-email/tests/test_credentials.py:92:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
+ src/integrations/prefect-email/tests/test_credentials.py:91:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-email/tests/test_credentials.py:92:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-email/tests/test_credentials.py:108:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
- src/integrations/prefect-email/tests/test_credentials.py:109:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `verify`
- src/integrations/prefect-email/tests/test_credentials.py:110:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
+ src/integrations/prefect-email/tests/test_credentials.py:108:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-email/tests/test_credentials.py:109:12: warning[possibly-missing-attribute] Attribute `verify` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-email/tests/test_credentials.py:110:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/execute.py:30:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/upload.py:54:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `Unknown | Coroutine[Any, Any, Unknown]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `Unknown | Coroutine[Any, Any, Unknown]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
- src/prefect/results.py:181:44: error[unknown-argument] Argument `_sync` does not match any known parameter
- Found 5531 diagnostics
+ Found 5518 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`

sympy (https://github.com/sympy/sympy)
- sympy/diffgeom/diffgeom.py:507:24: error[unsupported-operator] Operator `<=` is not supported between two objects of type `Unknown | int | list[Unknown]`
+ sympy/diffgeom/diffgeom.py:507:24: error[unsupported-operator] Operator `<=` is not supported between objects of type `Unknown | int | list[Unknown]` and `Unknown | int | list[Unknown]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5160 diagnostics
+ Found 5159 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2093 diagnostics
+ Found 2095 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14436 diagnostics
+ Found 14435 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2026-01-03 16:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-filter-cycle-recovery?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22356 will **degrade performance by 7.22%**

<sub>Comparing <code>alex/union-filter-cycle-recovery</code> (4447497) with <code>main</code> (e1439be)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-filter-cycle-recovery?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-filter-cycle-recovery?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.5 s | 11.4 s | -7.22% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-filter-cycle-recovery?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2026-01-03 16:39_

The failing fuzzer snippets both involve implicit calls to dunder methods on objects that inhabit recursive type-alias types:

```py
 try:
    pass
except:
    type name_3 = name_3
else:
    from ... import name_3

async def name_0(name_2: name_3, /):
    async for unique_name_0 in name_2:
        pass
```

and

```py
 try:
    try:
        pass
    except* 0 as name_0:
        pass
except* 0:
    type name_0 = name_0

async def name_2(name_5: name_0):
    await name_5
```

Looking at the looping part of the stack trace, the cause of the stack overflow is pretty clear (and I'm honestly surprised it doesn't overflow on `main`?):

```
frame #2875: 0x00000001014e8a18 ty`ty_python_semantic::types::UnionType::map::h88e20701319913fd(self=UnionType @ 0x000000016ff16e58, db=&dyn ty_python_semantic::db::Db @ 0x000000016ff16e60, transform_fn={closure_env#0} @ 0x000000016ff16e70) at types.rs:14055:14
frame #2876: 0x0000000101491fec ty`ty_python_semantic::types::Type::to_meta_type::hf17de695465aca64(self=<incomplete type>, db=&dyn ty_python_semantic::db::Db @ 0x000000016ff170e0) at types.rs:7809:41
frame #2877: 0x00000001014921ec ty`ty_python_semantic::types::Type::to_meta_type::hf17de695465aca64(self=<incomplete type>, db=&dyn ty_python_semantic::db::Db @ 0x000000016ff17400) at types.rs:7851:60
frame #2878: 0x00000001014d87b8 ty`ty_python_semantic::types::Type::to_meta_type::_$u7b$$u7b$closure$u7d$$u7d$::heca5961951528140(ty=0x0000600000ff9dc0) at types.rs:7809:57
frame #2879: 0x0000000100a25d74 ty`core::iter::adapters::map::map_fold::_$u7b$$u7b$closure$u7d$$u7d$::ha564dd4859174586(acc=<unavailable>, elt=0x0000600000ff9dc0) at map.rs:88:28
frame #2880: 0x0000000100c1f1a8 ty`_$LT$core..slice..iter..Iter$LT$T$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::fold::hcc34235e70409dc2(self=Iter<ty_python_semantic::types::Type> @ 0x000000016ff17790, init=UnionBuilder @ 0x000000016ff178e0, f={closure_env#0}<&ty_python_semantic::types::Type, ty_python_semantic::types::Type, ty_python_semantic::types::builder::UnionBuilder, ty_python_semantic::types::{impl#172}::to_meta_type::{closure_env#0}, fn(ty_python_semantic::types::builder::UnionBuilder, ty_python_semantic::types::Type) -> ty_python_semantic::types::builder::UnionBuilder> @ 0x000000016ff17698) at macros.rs:255:27
frame #2881: 0x00000001009f5390 ty`_$LT$core..iter..adapters..map..Map$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::fold::h97aeca3a0b0f2f4d(self=<unavailable>, init=<unavailable>, g=0x00000170e03dc000) at map.rs:128:19
frame #2882: 0x00000001014e8a18 ty`ty_python_semantic::types::UnionType::map::h88e20701319913fd(self=UnionType @ 0x000000016ff17918, db=&dyn ty_python_semantic::db::Db @ 0x000000016ff17920, transform_fn={closure_env#0} @ 0x000000016ff17930) at types.rs:14055:14
```

---
