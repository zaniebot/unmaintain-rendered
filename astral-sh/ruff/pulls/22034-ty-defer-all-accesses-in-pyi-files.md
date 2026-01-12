```yaml
number: 22034
title: "[ty] Defer all accesses in `.pyi` files"
type: pull_request
state: closed
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/always-defer
created_at: 2025-12-17T18:36:18Z
updated_at: 2026-01-06T02:07:11Z
url: https://github.com/astral-sh/ruff/pull/22034
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Defer all accesses in `.pyi` files

---

_@charliermarsh_

## Summary

An alternative to https://github.com/astral-sh/ruff/pull/22029.


---

_Comment by @astral-sh-bot[bot] on 2025-12-17 18:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-17 18:39_

---

_Renamed from "Defer all accesses in `.pyi` files" to "[ty] Defer all accesses in `.pyi` files" by @AlexWaygood on 2025-12-17 18:39_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 18:39_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
porcupine (https://github.com/Akuli/porcupine)
+ porcupine/plugins/fullscreen.py:7:5: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/fullscreen.py:7:5: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/fullscreen.py:7:5: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/fullscreen.py:7:5: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:37:13: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:37:13: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:37:13: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:37:13: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:46:29: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:46:29: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:46:29: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
+ porcupine/plugins/geometry.py:46:29: error[no-matching-overload] No overload of bound method `wm_attributes` matches arguments
- Found 18 diagnostics
+ Found 30 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/builtins.pyi:141:9: error[invalid-method-override] Invalid override of method `__subclasshook__`: Definition is incompatible with `builtins.object.__subclasshook__`
+ mypy/typeshed/stdlib/logging/config.pyi:108:13: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ mypy/typeshed/stdlib/multiprocessing/managers.pyi:87:13: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
+ mypy/typeshed/stdlib/multiprocessing/managers.pyi:93:13: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableMapping.pop`
+ mypy/typeshed/stdlib/turtle.pyi:173:13: error[invalid-method-override] Invalid override of method `__mul__`: Definition is incompatible with `tuple.__mul__`
- mypyc/test-data/fixtures/typing-full.pyi:50:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- mypyc/test-data/fixtures/typing-full.pyi:56:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- mypyc/test-data/fixtures/typing-full.pyi:61:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- mypyc/test-data/fixtures/typing-full.pyi:66:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- mypyc/test-data/fixtures/typing-full.pyi:100:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- mypyc/test-data/fixtures/typing-full.pyi:118:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- mypyc/test-data/fixtures/typing-full.pyi:123:2: error[unresolved-reference] Name `runtime_checkable` used when not defined
- Found 1785 diagnostics
+ Found 1781 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2792 diagnostics
+ Found 2789 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5081 diagnostics
+ Found 5078 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14392 diagnostics
+ Found 14391 diagnostics

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


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-17 18:55_

Oh, this was naive... You do need some kind of selective deferral otherwise you trivially hit cycles.

---

_Comment by @carljm on 2025-12-17 21:51_

I do think this approach is plausible. Those `linter_no_panic` tests are quite aggressive stress tests of stub file handling, in that they take regular (and a lot of quite pathological) `.py` files, rename them to `.pyi`, and check them as stubs. Which means we see a lot of stuff (and a lot more non-trivial assignment right-hand-sides and function bodies) than you'd ever tend to see in a real stub file.

We'd obviously want to track down and fix this specific panic in order to land this, but I'm still optimistic that this could land without causing lots of issues or performance regressions in real stub files (as evidenced by the fact that the benchmarks and primer all seem happy here, it's just `linter_no_panic` with issues.)

---

_Comment by @carljm on 2025-12-17 21:53_

(I'm also curious if we could get away with doing a lot less type inference in right-hand-sides and function bodies in stub files in general -- but again this would probably help `linter_no_panic` but not really move the performance needle in the real world.)

---

_Closed by @charliermarsh on 2026-01-06 02:07_

---
