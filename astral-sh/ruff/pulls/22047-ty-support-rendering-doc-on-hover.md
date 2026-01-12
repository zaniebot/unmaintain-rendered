```yaml
number: 22047
title: "[ty] Support rendering `__doc__` on hover"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/dunder-doc
created_at: 2025-12-18T06:13:38Z
updated_at: 2025-12-18T11:50:04Z
url: https://github.com/astral-sh/ruff/pull/22047
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Support rendering `__doc__` on hover

---

_@Gankra_

## Summary

I got this far before I realized how nightmarish the pytorch `__doc__` uses are. Not even asking the type checker what the string's value is would help here (the version I have checked in here is actually way way easier than what pytorch actually has).

I don't really think this is worth landing if all the actual usecases of `__doc__` are still impossible for us to handle.

* Fixes https://github.com/astral-sh/ty/issues/2051

## Test Plan

Snapshots


---

_Label `ty` added by @Gankra on 2025-12-18 06:13_

---

_Label `server` added by @Gankra on 2025-12-18 06:13_

---

_Renamed from "[ty] support rendering `__doc__` on hover" to "[ty] Support rendering `__doc__` on hover" by @Gankra on 2025-12-18 06:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 06:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 06:16_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
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
- Found 44 diagnostics
+ Found 43 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/examples/jointree.py:38:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:38:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:39:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:39:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:40:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:40:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:41:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:41:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:42:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:42:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:43:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:43:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:53:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:53:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:64:22: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:64:22: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/array_api_jit.py:32:86: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown & BaseExpr`
+ python/egglog/exp/array_api_jit.py:32:86: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> BaseExpr & Unknown`
- python/egglog/exp/program_gen.py:131:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:131:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:181:46: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:181:46: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:183:39: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:183:39: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:188:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:188:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:198:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:198:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:224:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:224:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:228:46: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:228:46: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:231:66: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:231:66: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:234:46: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:234:46: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:234:67: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:234:67: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:237:47: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:237:47: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:237:91: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:237:91: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:252:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:252:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:253:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:253:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:265:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:265:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:266:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:266:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:274:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:274:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:275:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:275:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:285:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:285:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:286:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:286:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:302:56: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:302:56: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:304:49: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:304:49: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:312:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:312:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:325:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:325:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:340:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:340:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:353:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:353:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:373:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:373:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:374:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:374:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:375:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:375:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:403:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:403:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:404:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:404:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:405:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:405:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:406:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:406:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/tests/test_array_api.py:290:92: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown & BaseExpr`
+ python/tests/test_array_api.py:290:92: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> BaseExpr & Unknown`
- python/tests/test_program_gen.py:87:47: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/tests/test_program_gen.py:87:47: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> BaseExpr & Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2795 diagnostics
+ Found 2792 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14392 diagnostics
+ Found 14391 diagnostics

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


```

</details>


No memory usage changes detected ✅



---

_Comment by @MichaReiser on 2025-12-18 06:57_

Agree, that we shouldn't implement this. I commented on the issue with where I think the difference to pylance and pyrefly comes from.

---

_Closed by @Gankra on 2025-12-18 11:50_

---
