```yaml
number: 22044
title: "[ty] Implement disjointness for TypedDicts"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
assignees: []
merged: true
base: main
head: typeddict_disjoint
created_at: 2025-12-18T00:51:30Z
updated_at: 2025-12-19T17:58:18Z
url: https://github.com/astral-sh/ruff/pull/22044
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Implement disjointness for TypedDicts

---

_@oconnor663_

This is a preliminary step to tagged union narrowing for `TypedDict`: https://github.com/astral-sh/ty/issues/1479

---

_Review requested from @carljm by @oconnor663 on 2025-12-18 00:51_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-18 00:51_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-18 00:51_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-18 00:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 00:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@oconnor663 reviewed on 2025-12-18 00:53_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1896 on 2025-12-18 00:53_

Is this sort of "one dynamic type winds up in multiple fields" situation ever something we reason about? Is there a name for this?

---

_@oconnor663 reviewed on 2025-12-18 00:54_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:4050 on 2025-12-18 00:54_

Could we just make this `ConstraintSet::from(true)` and declare that `TypedDict`s are disjoint from anything that isn't a `TypedDict`? I assume cases like `object` and `Mapping` are handled above? (But I should probably test for that...)

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 00:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | int | T@resolve_variables | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/lists/model.py:430:24: warning[possibly-missing-attribute] Attribute `key` may be missing on object of type `(Thing & ~str & ~Top[dict[Unknown, Unknown]]) | (AnnotatedSeed & ~str & ~Top[dict[Unknown, Unknown]])`
+ openlibrary/core/lists/model.py:430:24: warning[possibly-missing-attribute] Attribute `key` may be missing on object of type `(Thing & ~str & ~Top[dict[Unknown, Unknown]]) | (AnnotatedSeed & ~Top[dict[Unknown, Unknown]])`
- openlibrary/core/lists/model.py:431:13: error[invalid-assignment] Object of type `(Thing & ~str & ~Top[dict[Unknown, Unknown]]) | (AnnotatedSeed & ~str & ~Top[dict[Unknown, Unknown]])` is not assignable to attribute `value` of type `Thing | str`
+ openlibrary/core/lists/model.py:431:13: error[invalid-assignment] Object of type `(Thing & ~str & ~Top[dict[Unknown, Unknown]]) | (AnnotatedSeed & ~Top[dict[Unknown, Unknown]])` is not assignable to attribute `value` of type `Thing | str`
+ openlibrary/plugins/openlibrary/lists.py:89:19: error[invalid-key] Unknown key "key" for TypedDict `AnnotatedSeedDict`: Unknown key "key"
- Found 1152 diagnostics
+ Found 1153 diagnostics

altair (https://github.com/vega/altair)
- altair/vegalite/v6/api.py:655:21: error[invalid-assignment] Object of type `(OperatorMixin & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (Expr & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (_ConditionExtra & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression)` is not assignable to `_ConditionExtra`
+ altair/vegalite/v6/api.py:655:21: error[invalid-assignment] Object of type `(OperatorMixin & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (Expr & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (_ConditionExtra & Top[dict[Unknown, Unknown]])` is not assignable to `_ConditionExtra`

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
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`

zulip (https://github.com/zulip/zulip)
+ zerver/lib/markdown/__init__.py:1122:54: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str]]`
+ zerver/lib/markdown/__init__.py:1131:54: error[invalid-argument-type] Argument to bound method `handle_video_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str]]`
- Found 3674 diagnostics
+ Found 3676 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/aprilaire/coordinator.py:106:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/nexia/entity.py:110:9: error[unsupported-operator] Operator `|=` is not supported between objects of type `DeviceInfo & ~None` and `dict[Unknown | str, Unknown | set[Unknown | tuple[str, Unknown]] | tuple[str, Unknown]]`
+ homeassistant/components/nexia/entity.py:110:9: error[unsupported-operator] Operator `|=` is not supported between objects of type `DeviceInfo` and `dict[Unknown | str, Unknown | set[Unknown | tuple[str, Unknown]] | tuple[str, Unknown]]`
- Found 14415 diagnostics
+ Found 14414 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_generate_schema.py:2773:21: error[unresolved-attribute] Object of type `DefinitionReferenceSchema & ~None` has no attribute `clear`
+ pydantic/_internal/_generate_schema.py:2773:21: error[unresolved-attribute] Object of type `DefinitionReferenceSchema` has no attribute `clear`
- pydantic/_internal/_generate_schema.py:2780:21: error[unresolved-attribute] Object of type `DefinitionReferenceSchema & ~None` has no attribute `clear`
+ pydantic/_internal/_generate_schema.py:2780:21: error[unresolved-attribute] Object of type `DefinitionReferenceSchema` has no attribute `clear`


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-18 01:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeddict_disjoint?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22044 will **improve performances by 70.52%**

<sub>Comparing <code>typeddict_disjoint</code> (18c5ae7) with <code>main</code> (2e44a86)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/typeddict_disjoint?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 219.4 s | 128.7 s | +70.52% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/typeddict_disjoint?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@oconnor663 reviewed on 2025-12-18 01:21_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:4050 on 2025-12-18 01:21_

I added a "Disjointness with other types" section to the mdtests.

---

_Label `ty` added by @MichaReiser on 2025-12-18 06:58_

---

_Renamed from "[ty] `TypedDictType::is_disjoint_from_impl`" to "[ty] Implement disjointness for TypedDicts" by @AlexWaygood on 2025-12-18 12:07_

---

_@AlexWaygood reviewed on 2025-12-18 12:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4052 on 2025-12-18 12:26_

I'm not sure cases like `object` and `Mapping` _are_ handled above. But yes, `TypedDict` types are disjoint from almost all non-`TypedDict` types. For now, I would do this:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/typed_dict.md b/crates/ty_python_semantic/resources/mdtest/typed_dict.md
index c303a61642..f7cff1f03b 100644
--- a/crates/ty_python_semantic/resources/mdtest/typed_dict.md
+++ b/crates/ty_python_semantic/resources/mdtest/typed_dict.md
@@ -2002,9 +2002,11 @@ class RegularNonTD: ...
 
 static_assert(not is_disjoint_from(TD, object))
 static_assert(not is_disjoint_from(TD, Mapping[str, object]))
-# TODO: We should be able to assert that these are disjoint.
-static_assert(not is_disjoint_from(TD, Mapping[int, object]))
-static_assert(not is_disjoint_from(TD, RegularNonTD))
+static_assert(is_disjoint_from(TD, Mapping[int, object]))
+static_assert(is_disjoint_from(TD, RegularNonTD))
+
+# TODO: should pass
+static_assert(is_disjoint_from(TD, dict[str, str]))  # error: [static-assert-error]
 ```
 
 [subtyping section]: https://typing.python.org/en/latest/spec/typeddict.html#subtyping-between-typeddict-types
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index edb68a5b2c..d79173686e 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -4046,10 +4046,17 @@ impl<'db> Type<'db> {
                 })
             }
 
-            (Type::TypedDict(_), _) | (_, Type::TypedDict(_)) => {
-                // TODO: Implement disjointness for TypedDict with other types.
-                ConstraintSet::from(false)
-            }
+            (Type::TypedDict(_), other) | (other, Type::TypedDict(_)) => KnownClass::Dict
+                .to_specialized_instance(db, [KnownClass::Str.to_instance(db), Type::any()])
+                .has_relation_to_impl(
+                    db,
+                    other,
+                    inferable,
+                    TypeRelation::Assignability,
+                    relation_visitor,
+                    disjointness_visitor,
+                )
+                .negate(db),
         }
     }
````

What this is saying is: for any type `T`, if `dict[str, Any]` is not assignable to `T`, then all `TypedDict` types will always be disjoint from `T`. (Actually, that might be good to add as a comment, if you accept my patch above.)

This still leave us with some false negatives: you can see the added failing test I added in my patch above. But it gets rid fo most false negatives, and it's okay for our disjointness implementation to have some false negatives. The main thing we want to avoid is false positives.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1 on 2025-12-18 14:09_

Fantastic test suite!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1896 on 2025-12-18 14:11_

hmm, yeah, this is pretty interesting. No, I haven't come across this problem before, but thinking about it, it must exist for other generic types in our model too. I wouldn't worry about it too much right now, but it could be worth opening a followup issue to discuss it.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:348 on 2025-12-18 14:57_

nit: if this whole comment uses `///` rather than `//`, it'll be rendered as a *beautiful* tooltip when I hover over the method in VSCode locally :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:406 on 2025-12-18 15:00_

🙃

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:416 on 2025-12-18 15:05_

if we never look at the name here, can we simplify slightly by having `btreemap_overlapping_items` only yield the value pairs?

```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index d392a85db5..2d080f9104 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -413,7 +413,7 @@ impl<'db> TypedDictType<'db> {
         relation_visitor: &HasRelationToVisitor<'db>,
     ) -> ConstraintSet<'db> {
         let fields_in_common = btreemap_overlapping_items(self.items(db), other.items(db));
-        fields_in_common.when_any(db, |(_name, (self_field, other_field))| {
+        fields_in_common.when_any(db, |(self_field, other_field)| {
             // Condition 1 above.
             if self_field.is_required() || other_field.is_required() {
                 if (!self_field.is_required() && !self_field.is_read_only())
@@ -1051,14 +1051,14 @@ bitflags! {
 
 impl get_size2::GetSize for TypedDictFieldFlags {}
 
-/// Yield all the key/val pairs where the same key is present in both `BTreeMap`s. Take advantage
+/// Yield all the values where the same key is present in both `BTreeMap`s. Take advantage
 /// of the fact that keys are sorted to walk through each map once without doing any lookups. It
 /// would be nice if `BTreeMap` had something like `BTreeSet::intersection` that did this for us,
 /// but as far as I know we have to do it ourselves. Life is hard.
 fn btreemap_overlapping_items<'a, K, V1, V2>(
     left: &'a BTreeMap<K, V1>,
     right: &'a BTreeMap<K, V2>,
-) -> impl Iterator<Item = (&'a K, (&'a V1, &'a V2))>
+) -> impl Iterator<Item = (&'a V1, &'a V2)>
 where
     K: Ord,
 {
@@ -1073,7 +1073,7 @@ where
                     // Matching keys. Yield this pair of values and advance both iterators.
                     left_items.next();
                     right_items.next();
-                    return Some((left_key, (left_val, right_val)));
+                    return Some((left_val, right_val));
                 }
                 Ordering::Less => {
                     // `left_items` is behind `right_items` in key order. Advance `left_items`.
@@ -1097,11 +1097,11 @@ fn test_btreemap_overlapping_items() {
     let right = BTreeMap::from_iter([("b", 2.0), ("d", 4.0), ("f", 6.0)]);
     assert_eq!(
         btreemap_overlapping_items(&left, &right).collect::<Vec<_>>(),
-        vec![(&"b", (&2, &2.0)), (&"d", (&4, &4.0))],
+        vec![(&2, &2.0), (&4, &4.0)],
     );
     assert_eq!(
         btreemap_overlapping_items(&right, &left).collect::<Vec<_>>(),
-        vec![(&"b", (&2.0, &2)), (&"d", (&4.0, &4))],
+        vec![(&2.0, &2), (&4.0, &4)],
     );
 
     // A case where one side is empty.
```

---

_@AlexWaygood approved on 2025-12-18 15:07_

This looks great!! Really good job

---

_@oconnor663 reviewed on 2025-12-18 16:33_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:348 on 2025-12-18 16:33_

I had it that way until 367e2f7ddff57ca2c048d9951dc2403f00732044 :sob::
```
Clippy and Cargo want very different things here. Clippy demands that
the numbered list is indented, but that makes Cargo think it's Rust code
and start trying to compile it. I don't know how to make them both
happy, so I'm making it a regular `//` comment block instead of an
actual `///` doc comment, which seems to shut them both up...
```

---

_@AlexWaygood reviewed on 2025-12-18 16:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:348 on 2025-12-18 16:57_

Bah, that's annoying. You could put the numbered list inside a ```` ```none ```` fence? This seems to appease both Clippy and Cargo:

````diff

diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index d392a85db5..e25ad80d07 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -343,67 +343,73 @@ impl<'db> TypedDictType<'db> {
         )
     }
 
-    // Two `TypedDict`s `A` and `B` are disjoint if it's impossible to come up with a third
-    // `TypedDict` `C` that's fully-static and assignable to both of them.
-    //
-    // `TypedDict` assignability is determined field-by-field, so we determine disjointness
-    // similarly. For any field that's only in `A`, it's always possible for our hypothetical `C`
-    // to copy/paste that field without losing assignability to `B` (and vice versa), so we only
-    // need to consider fields that are present in both `A` and `B`.
-    //
-    // There are three properties of each field to consider: the declared type, whether it's
-    // mutable ("mut" vs "imm" below), and whether it's required ("req" vs "opt" below). Here's a
-    // table summary of the restrictions on the declared type of a source field (for us that means
-    // in `C`, which we want to be assignable to both `A` and `B`) given a destination field (for
-    // us that means in either `A` or `B`). For completeness we'll also include the possibility
-    // that the source field is missing entirely, though we'll soon see that we can ignore that
-    // case. This table is essentially what `has_relation_to_impl` implements above. Here
-    // "equivalent" means the source and destination types must be equivalent/compatible,
-    // "assignable" means the source must be assignable to the destination, and "-" means the
-    // assignment is never allowed:
-    //
-    //    source→ | mut + req  | mut + opt  | imm + req  | imm + opt  |   [missing]   |
-    // ↓dest      |            |            |            |            |               |
-    // -----------|------------|------------|------------|------------|---------------|
-    // mut + req  | equivalent |     -      |     -      |     -      |       -       |
-    // mut + opt  |     -      | equivalent |     -      |     -      |       -       |
-    // imm + req  | assignable |     -      | assignable |     -      |       -       |
-    // imm + opt  | assignable | assignable | assignable | assignable | [dest is obj] |
-    //
-    // We can cut that table down substantially by noticing two things:
-    //
-    // - We don't need to consider the cases where the source field (in `C`) is `ReadOnly`/"imm",
-    //   because the mutable version of the same field is always "strictly more assignable". In
-    //   other words, nothing in the `TypedDict` assignability rules ever requires a source field
-    //   to be immutable.
-    // - We don't need to consider the special case where the source field is missing, because
-    //   that's only allowed when the destination is `ReadOnly[NotRequired[object]]`, which is
-    //   compatible with *any* choice of source field.
-    //
-    // The cases we actually need to reason about are this smaller table:
-    //
-    //    source→ | mut + req  | mut + opt  |
-    // ↓dest      |            |            |
-    // -----------|------------|------------|
-    // mut + req  | equivalent |     -      |
-    // mut + opt  |     -      | equivalent |
-    // imm + req  | assignable |     -      |
-    // imm + opt  | assignable | assignable |
-    //
-    // So, given a field name that's in both `A` and `B`, here are the conditions where it's
-    // *impossible* to choose a source field for `C` that's compatible with both destinations,
-    // which tells us that `A` and `B` are disjoint:
-    //
-    //  1. If one side is "mut+opt" (which forces the field in `C` to be "opt") and the other
-    //     side is "req" (which forces the field in `C` to be "req").
-    // 2a. If both sides are mutable, and their types are not equivalent/compatible. (Because
-    //     the type in `C` must be compatible with both of them.)
-    // 2b. If one sides is mutable, and its type is not assignable to the immutable side's type.
-    //     (Because the type in `C` must be compatible with the mutable side.)
-    // 2c. If both sides are immutable, and their types are disjoint. (Because the type in `C`
-    //      must be assignable to both.)
-    //
-    // TODO: Adding support for `closed` and `extra_items` will complicate this.
+    /// Two `TypedDict`s `A` and `B` are disjoint if it's impossible to come up with a third
+    /// `TypedDict` `C` that's fully-static and assignable to both of them.
+    ///
+    /// `TypedDict` assignability is determined field-by-field, so we determine disjointness
+    /// similarly. For any field that's only in `A`, it's always possible for our hypothetical `C`
+    /// to copy/paste that field without losing assignability to `B` (and vice versa), so we only
+    /// need to consider fields that are present in both `A` and `B`.
+    ///
+    /// There are three properties of each field to consider: the declared type, whether it's
+    /// mutable ("mut" vs "imm" below), and whether it's required ("req" vs "opt" below). Here's a
+    /// table summary of the restrictions on the declared type of a source field (for us that means
+    /// in `C`, which we want to be assignable to both `A` and `B`) given a destination field (for
+    /// us that means in either `A` or `B`). For completeness we'll also include the possibility
+    /// that the source field is missing entirely, though we'll soon see that we can ignore that
+    /// case. This table is essentially what `has_relation_to_impl` implements above. Here
+    /// "equivalent" means the source and destination types must be equivalent/compatible,
+    /// "assignable" means the source must be assignable to the destination, and "-" means the
+    /// assignment is never allowed:
+    ///
+    /// ```none
+    ///    source→ | mut + req  | mut + opt  | imm + req  | imm + opt  |   [missing]   |
+    /// ↓dest      |            |            |            |            |               |
+    /// -----------|------------|------------|------------|------------|---------------|
+    /// mut + req  | equivalent |     -      |     -      |     -      |       -       |
+    /// mut + opt  |     -      | equivalent |     -      |     -      |       -       |
+    /// imm + req  | assignable |     -      | assignable |     -      |       -       |
+    /// imm + opt  | assignable | assignable | assignable | assignable | [dest is obj] |
+    /// ```
+    ///
+    /// We can cut that table down substantially by noticing two things:
+    ///
+    /// - We don't need to consider the cases where the source field (in `C`) is `ReadOnly`/"imm",
+    ///   because the mutable version of the same field is always "strictly more assignable". In
+    ///   other words, nothing in the `TypedDict` assignability rules ever requires a source field
+    ///   to be immutable.
+    /// - We don't need to consider the special case where the source field is missing, because
+    ///   that's only allowed when the destination is `ReadOnly[NotRequired[object]]`, which is
+    ///   compatible with *any* choice of source field.
+    ///
+    /// The cases we actually need to reason about are this smaller table:
+    ///
+    /// ```none
+    ///    source→ | mut + req  | mut + opt  |
+    /// ↓dest      |            |            |
+    /// -----------|------------|------------|
+    /// mut + req  | equivalent |     -      |
+    /// mut + opt  |     -      | equivalent |
+    /// imm + req  | assignable |     -      |
+    /// imm + opt  | assignable | assignable |
+    /// ```
+    ///
+    /// So, given a field name that's in both `A` and `B`, here are the conditions where it's
+    /// *impossible* to choose a source field for `C` that's compatible with both destinations,
+    /// which tells us that `A` and `B` are disjoint:
+    ///
+    /// ```none
+    /// 1. If one side is "mut+opt" (which forces the field in `C` to be "opt") and the other
+    ///    side is "req" (which forces the field in `C` to be "req").
+    /// 2a. If both sides are mutable, and their types are not equivalent/compatible. (Because
+    ///    the type in `C` must be compatible with both of them.)
+    /// 2b. If one sides is mutable, and its type is not assignable to the immutable side's type.
+    ///    (Because the type in `C` must be compatible with the mutable side.)
+    /// 2c. If both sides are immutable, and their types are disjoint. (Because the type in `C`
+    ///    must be assignable to both.)
+    /// ```
+    ///
+    /// TODO: Adding support for `closed` and `extra_items` will complicate this.
     pub(crate) fn is_disjoint_from_impl(
         self,
         db: &'db dyn Db,

````

---

_@oconnor663 reviewed on 2025-12-18 19:53_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:416 on 2025-12-18 19:53_

1addb0bdbd

---

_@oconnor663 reviewed on 2025-12-18 19:54_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:4052 on 2025-12-18 19:54_

Oh that's sick, ty. df5bc749a8

---

_@oconnor663 reviewed on 2025-12-18 19:55_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:348 on 2025-12-18 19:55_

I replaced 2a/2b/2c with 2/3/4, and that seemed to work: f64830ab4a

---

_Comment by @oconnor663 on 2025-12-18 21:16_

This mypy_primer hit is in fact a new false positive:
```diff
+ openlibrary/plugins/openlibrary/lists.py:89:19: error[invalid-key] Unknown key "key" for TypedDict `AnnotatedSeedDict`: Unknown key "key"
```
Previously [the type of `seed` on this line](https://github.com/internetarchive/openlibrary/blob/ee5541d5a9fce74a60a6f6685c519b1c271832c8/openlibrary/plugins/openlibrary/lists.py#L89) was `(ThingReferenceDict & ~str) | (AnnotatedSeedDict & ~str)`, which I guess(?) dodged our check that `["key"]` matches a field name in all of the possible `TypedDict`s. As of this PR, that simplifies to `ThingReferenceDict | AnnotatedSeedDict`, which does get checked, and we complain that `"key"` isn't a valid field name for `AnnotatedSeedDict`. ~~What we're missing is that the previous `elif 'thing' in seed` check was guaranteed to match `AnnotatedSeedDict`, so in fact the type of `seed` here should be just `ThingReferenceDict`, which makes `["key"]` valid. I think my follow-up work on union simplification is likely to help with this, and a minimized version of this will make a good test case for it.~~

Edit: This is mistaken, see the next comment.

---

_Merged by @oconnor663 on 2025-12-18 21:20_

---

_Closed by @oconnor663 on 2025-12-18 21:20_

---

_Branch deleted on 2025-12-18 21:20_

---

_Comment by @oconnor663 on 2025-12-19 17:56_

Hmm, actually my comment above is mistaken. `elif 'thing' in seed` _looks_ like it should narrow the union to `AnnotatedSeedDict`, but unfortunately these `TypedDict`s are `closed=False` (the default and the only option prior to Python 3.15), which means that `ThingReferenceDict` is _allowed_ to contain the key `'thing'`. Concretely, it could've been assigned to with another `TypedDict` type that has all of these fields put together. This error is actually a true positive, which is the main motivating case for adding the `closed` feature: https://peps.python.org/pep-0728/#disallowing-extra-items-explicitly

---
