```yaml
number: 22040
title: "[ty] Fix some configuration panics in the LSP"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/sfinae2
created_at: 2025-12-17T22:35:29Z
updated_at: 2025-12-18T14:47:04Z
url: https://github.com/astral-sh/ruff/pull/22040
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Fix some configuration panics in the LSP

---

_@Gankra_

## Summary

This is a revival of https://github.com/astral-sh/ruff/pull/21047 now that we have a reproducer again.

* Fixes https://github.com/astral-sh/ty/issues/2031
* Fixes https://github.com/astral-sh/ty/issues/859

## Test Plan

e2e test from @zanieb 


---

_Review requested from @carljm by @Gankra on 2025-12-17 22:35_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-17 22:35_

---

_Review requested from @sharkdp by @Gankra on 2025-12-17 22:35_

---

_Review requested from @dcreager by @Gankra on 2025-12-17 22:35_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-17 22:35_

---

_Label `server` added by @Gankra on 2025-12-17 22:35_

---

_Label `ty` added by @Gankra on 2025-12-17 22:35_

---

_@Gankra reviewed on 2025-12-17 22:36_

---

_Review comment by @Gankra on `crates/ty_server/tests/e2e/initialize.rs`:498 on 2025-12-17 22:36_

Note this message is not "we crash and die" but rather "we are using the default fallback database that all python files outside the workspace have" (so hopefully graceful degradation, but enough of a degradation we probably want the user to look into it and fix it).

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 22:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 22:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2795 diagnostics
+ Found 2792 diagnostics

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
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14396 diagnostics
+ Found 14397 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @carljm removed by @carljm on 2025-12-17 22:47_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 22:55_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @Gankra on 2025-12-17 23:41_

I can't for the life of me fix this bloody windows path escaping, please someone save me.

---

_Comment by @Gankra on 2025-12-17 23:42_

I guess I can just, not snapshot the message.

---

_Comment by @codspeed-hq[bot] on 2025-12-18 00:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fsfinae2?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22040 will **not alter performance**

<sub>Comparing <code>gankra/sfinae2</code> (ee2f428) with <code>main</code> (8d32ad1)</sub>



### Summary

`✅ 52` untouched  





---

_Comment by @Gankra on 2025-12-18 00:43_

That codspeed result has to be a false positive

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/test.rs`:29 on 2025-12-18 09:27_

I'm inclined to add this `InMemorySystem` instead. 

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:103 on 2025-12-18 09:29_

The idea of `Options` is that they directly resemble the configuration file/CLI options. It shouldn't contain any program state or inferred values. I think I would either make `safe_mode` an argument (or provide methods like `ProjectDatabase::default(metadata_, system)` or/and add it to `ProjectMetadata` (we already store some derived CLI state on `ProjectMetadata`)

---

_@MichaReiser reviewed on 2025-12-18 09:37_

---

_@Gankra reviewed on 2025-12-18 13:07_

---

_Review comment by @Gankra on `crates/ruff_db/src/system/test.rs`:29 on 2025-12-18 13:07_

The e2e tests don't use InMemorySystem, alas.

---

_@Gankra reviewed on 2025-12-18 13:40_

---

_Review comment by @Gankra on `crates/ty_project/src/metadata/options.rs`:103 on 2025-12-18 13:40_

Pushed up a shot at this.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 13:49_

---

_@MichaReiser approved on 2025-12-18 13:51_

---

_Merged by @Gankra on 2025-12-18 14:47_

---

_Closed by @Gankra on 2025-12-18 14:47_

---

_Branch deleted on 2025-12-18 14:47_

---
