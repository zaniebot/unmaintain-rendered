```yaml
number: 22123
title: "[ty] Add a dedicated diagnostic for TypedDict deletions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/del-typed-diagnostic
created_at: 2025-12-21T01:33:47Z
updated_at: 2025-12-24T03:49:43Z
url: https://github.com/astral-sh/ruff/pull/22123
synced_at: 2026-01-12T15:57:42Z
```

# [ty] Add a dedicated diagnostic for TypedDict deletions

---

_@charliermarsh_

## Summary

Provides a message like:

```
  error[invalid-argument-type]: Cannot delete required key "name" from TypedDict `Movie`
    --> test.py:15:7
     |
  15 | del m["name"]
     |       ^^^^^^
     |
  info: Field defined here
   --> test.py:4:5
    |
  4 |     name: str
    |     --------- `name` declared as required here; consider making it `NotRequired`
    |
  info: Only keys marked as `NotRequired` (or in a TypedDict with `total=False`) can be deleted
```


---

_Label `ty` added by @charliermarsh on 2025-12-21 01:33_

---

_Marked ready for review by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @carljm by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-21 01:35_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 01:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-24 03:46:54.298013322 +0000
+++ new-output.txt	2025-12-24 03:46:56.985034012 +0000
@@ -968,8 +968,8 @@
 typeddicts_extra_items.py:29:23: error[invalid-key] Unknown key "novel_adaptation" for TypedDict `Movie`: Unknown key "novel_adaptation"
 typeddicts_extra_items.py:39:54: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `InheritedMovie`: value of type `None`
 typeddicts_extra_items.py:43:5: error[invalid-key] Unknown key "other_extra_key" for TypedDict `InheritedMovie`: Unknown key "other_extra_key"
-typeddicts_extra_items.py:128:9: error[invalid-argument-type] Method `__delitem__` of type `(key: Never, /) -> None` cannot be called with key of type `Literal["name"]` on object of type `MovieEI`
-typeddicts_extra_items.py:129:9: error[invalid-argument-type] Method `__delitem__` of type `(key: Never, /) -> None` cannot be called with key of type `Literal["year"]` on object of type `MovieEI`
+typeddicts_extra_items.py:128:15: error[invalid-argument-type] Cannot delete required key "name" from TypedDict `MovieEI`
+typeddicts_extra_items.py:129:15: error[invalid-argument-type] Cannot delete unknown key "year" from TypedDict `MovieEI`
 typeddicts_extra_items.py:254:63: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
 typeddicts_extra_items.py:255:63: error[invalid-key] Unknown key "description" for TypedDict `MovieExtraStr`: Unknown key "description"
 typeddicts_extra_items.py:266:64: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
@@ -1002,7 +1002,7 @@
 typeddicts_operations.py:32:36: error[invalid-key] Unknown key "other" for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:47:1: error[unresolved-attribute] Object of type `Movie` has no attribute `clear`
-typeddicts_operations.py:49:5: error[invalid-argument-type] Method `__delitem__` of type `(key: Never, /) -> None` cannot be called with key of type `Literal["name"]` on object of type `Movie`
+typeddicts_operations.py:49:11: error[invalid-argument-type] Cannot delete required key "name" from TypedDict `Movie`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-21 01:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

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
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2806 diagnostics
+ Found 2803 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2098 diagnostics
+ Found 2096 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5106 diagnostics
+ Found 5104 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:3776 on 2025-12-22 09:00_

Nit

```suggestion
    if let Some(field) = field && 
    	let Some(declaration) = field.first_declaration() {
          let file = declaration.file(db);
          let module = parsed_module(db, file).load(db);

          let mut sub = SubDiagnostic::new(SubDiagnosticSeverity::Info, "Field defined here");
          sub.annotate(
              Annotation::secondary(
                  Span::from(file).with_range(declaration.full_range(db, &module).range()),
              )
              .message(format_args!(
                  "`{field_name}` declared as required here; consider making it `NotRequired`"
              )),
          );
          diagnostic.sub(sub);
    }
```

but with nicer formatting ;)

---

_@MichaReiser approved on 2025-12-22 09:02_

---

_Merged by @charliermarsh on 2025-12-24 03:49_

---

_Closed by @charliermarsh on 2025-12-24 03:49_

---

_Branch deleted on 2025-12-24 03:49_

---
