```yaml
number: 18948
title: "[ty] Support variable-length tuples in unpacking assignments"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/unpack-tuple
created_at: 2025-06-25T21:50:23Z
updated_at: 2025-07-01T07:11:36Z
url: https://github.com/astral-sh/ruff/pull/18948
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Support variable-length tuples in unpacking assignments

---

_Pull request opened by @dcreager on 2025-06-25 21:50_

This PR updates our unpacking assignment logic to use the new tuple machinery. As a result, we can now unpack variable-length tuples correctly.

As part of this, the `TupleSpec` classes have been renamed to `Tuple`, and can now contain any element (Rust) type, not just `Type<'db>`. The unpacker uses a tuple of `UnionBuilder`s to maintain the types that will be assigned to each target, as we iterate through potentially many union elements on the rhs. We also add a new consuming iterator for tuples, and update the `all_elements` methods to wrap the result in an enum (similar to `itertools::Position`) letting you know which part of the tuple each element appears in. I also added a new `UnionBuilder::try_build`, which lets you specify a different fallback type if the union contains no elements.

---

_Label `ty` added by @dcreager on 2025-06-25 21:50_

---

_Comment by @github-actions[bot] on 2025-06-25 21:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dedupe (https://github.com/dedupeio/dedupe)
+ error[invalid-argument-type] dedupe/convenience.py:77:16: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[Unknown]`
+ error[invalid-argument-type] dedupe/convenience.py:77:19: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[Unknown]`
- Found 59 diagnostics
+ Found 61 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-argument-type] src/jinja2/compiler.py:1133:38: Argument to bound method `ref` is incorrect: Expected `str`, found `str | (tuple[str, str] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/jinja2/compiler.py:1133:38: Argument to bound method `ref` is incorrect: Expected `str`, found `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])`
- error[invalid-argument-type] src/jinja2/compiler.py:1136:52: Argument to bound method `ref` is incorrect: Expected `str`, found `str | (tuple[str, str] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/jinja2/compiler.py:1136:52: Argument to bound method `ref` is incorrect: Expected `str`, found `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])`
- error[invalid-argument-type] src/jinja2/compiler.py:1149:38: Argument to bound method `ref` is incorrect: Expected `str`, found `str | (tuple[str, str] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/jinja2/compiler.py:1149:38: Argument to bound method `ref` is incorrect: Expected `str`, found `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])`

pywin32 (https://github.com/mhammond/pywin32)
- error[invalid-argument-type] win32/Demos/win32gui_menu.py:434:67: Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[@Todo(Unpack variable-length tuple), @Todo(Unpack variable-length tuple), @Todo(Unpack variable-length tuple), @Todo(Unpack variable-length tuple)]`
+ error[invalid-argument-type] win32/Demos/win32gui_menu.py:434:67: Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[Any, Any, Any, Any]`

sympy (https://github.com/sympy/sympy)
+ error[unsupported-operator] sympy/assumptions/tests/test_refine.py:53:26: Operator `-` is unsupported between objects of type `Unknown | int | float` and `Unknown | Half`
+ error[unsupported-operator] sympy/assumptions/tests/test_refine.py:54:26: Operator `+` is unsupported between objects of type `Unknown | int | float` and `Unknown | Half`
+ warning[possibly-unbound-attribute] sympy/core/tests/test_assumptions.py:714:14: Attribute `xreplace` on type `Unknown | int` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_evalf.py:371:12: Attribute `evalf` on type `Unknown | int` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_evalf.py:374:12: Attribute `evalf` on type `Unknown | int` is possibly unbound
+ error[invalid-argument-type] sympy/core/tests/test_evalf.py:435:39: Argument to bound method `evalf` is incorrect: Expected `dict[Basic, Basic | int | float] | None`, found `tuple[Unknown | Symbol, Literal[1]]`
+ warning[possibly-unbound-attribute] sympy/core/tests/test_evalf.py:651:12: Attribute `evalf` on type `Unknown | int` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_evalf.py:652:12: Attribute `evalf` on type `Unknown | int` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_expr.py:607:12: Attribute `is_polynomial` on type `Unknown | int` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_expr.py:608:12: Attribute `is_polynomial` on type `Unknown | int` is possibly unbound
- error[unsupported-operator] sympy/core/tests/test_expr.py:667:25: Operator `/` is unsupported between objects of type `Unknown | NaN | Infinity | NegativeInfinity | ComplexInfinity` and `Literal[1] | Unknown`
+ error[unsupported-operator] sympy/core/tests/test_expr.py:667:25: Operator `/` is unsupported between objects of type `Unknown | NaN | Infinity | NegativeInfinity | ComplexInfinity` and `Literal[1] | Unknown | Symbol`
+ warning[possibly-unbound-attribute] sympy/core/tests/test_expr.py:770:36: Attribute `cos` on type `Unknown | Symbol` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_expr.py:771:36: Attribute `sin` on type `Unknown | Symbol` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_expr.py:772:36: Attribute `exp` on type `Unknown | Symbol` is possibly unbound
+ error[invalid-argument-type] sympy/core/tests/test_expr.py:1669:43: Argument to bound method `has_xfree` is incorrect: Expected `set[Basic]`, found `Unknown | Symbol`
+ error[invalid-argument-type] sympy/core/tests/test_expr.py:1670:43: Argument to bound method `has_xfree` is incorrect: Expected `set[Basic]`, found `list[Unknown]`
+ warning[possibly-unbound-attribute] sympy/core/tests/test_expr.py:1888:12: Attribute `is_constant` on type `Unknown | int` is possibly unbound
- error[invalid-argument-type] sympy/core/tests/test_match.py:520:42: Argument to function `symbols` is incorrect: Expected `int`, found `tuple[Unknown]`
+ error[invalid-argument-type] sympy/core/tests/test_match.py:520:42: Argument to function `symbols` is incorrect: Expected `int`, found `tuple[Unknown | Symbol]`
+ error[unsupported-operator] sympy/core/tests/test_power.py:52:38: Operator `*` is unsupported between objects of type `Unknown | Half` and `Unknown | int`
+ warning[possibly-unbound-attribute] sympy/core/tests/test_power.py:282:12: Attribute `base` on type `Unknown | Expr` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_power.py:282:20: Attribute `exp` on type `Unknown | Expr` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_subs.py:750:12: Attribute `subs` on type `Unknown | int` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/tests/test_subs.py:751:12: Attribute `subs` on type `Unknown | int` is possibly unbound
+ error[call-non-callable] sympy/core/tests/test_symbol.py:338:31: Object of type `Symbol` is not callable
+ error[call-non-callable] sympy/core/tests/test_symbol.py:339:31: Object of type `Symbol` is not callable
+ error[call-non-callable] sympy/core/tests/test_symbol.py:340:31: Object of type `Symbol` is not callable
+ error[call-non-callable] sympy/core/tests/test_symbol.py:341:31: Object of type `Symbol` is not callable
+ error[call-non-callable] sympy/core/tests/test_symbol.py:342:31: Object of type `Symbol` is not callable
+ error[unsupported-operator] sympy/geometry/polygon.py:784:16: Operator `>` is not supported for types `int` and `Basic`, in comparing `Literal[0]` with `Basic`
+ error[unsupported-operator] sympy/geometry/polygon.py:785:20: Operator `<=` is not supported for types `int` and `Basic`, in comparing `Literal[0]` with `Basic`
+ error[unsupported-operator] sympy/geometry/polygon.py:786:24: Operator `<=` is not supported for types `int` and `Basic`, in comparing `Literal[0]` with `Basic`
+ error[unsupported-operator] sympy/geometry/polygon.py:788:40: Unary operator `-` is unsupported for type `Basic`
+ error[unsupported-operator] sympy/geometry/polygon.py:788:47: Operator `-` is unsupported between objects of type `Basic` and `Basic`
+ error[unsupported-operator] sympy/geometry/polygon.py:788:59: Operator `-` is unsupported between objects of type `Basic` and `Basic`
+ warning[possibly-unbound-attribute] sympy/series/tests/test_series.py:421:32: Attribute `series` on type `Unknown | int` is possibly unbound
+ error[unresolved-attribute] sympy/sets/sets.py:2526:12: Type `Basic` has no attribute `variables`
+ error[unresolved-attribute] sympy/sets/sets.py:2526:30: Type `Basic` has no attribute `expr`
+ error[unresolved-attribute] sympy/sets/sets.py:2534:54: Type `Basic` has no attribute `variables`
+ error[unresolved-attribute] sympy/sets/sets.py:2536:21: Type `Basic` has no attribute `variables`
+ error[unresolved-attribute] sympy/sets/sets.py:2538:31: Type `Basic` has no attribute `expr`
+ error[unresolved-attribute] sympy/sets/tests/test_sets.py:844:12: Type `Basic` has no attribute `_contains`
+ error[unresolved-attribute] sympy/sets/tests/test_sets.py:845:12: Type `Basic` has no attribute `_contains`
+ error[unresolved-attribute] sympy/simplify/hyperexpand.py:1798:17: Type `Expr` has no attribute `exp`
+ error[unresolved-attribute] sympy/simplify/hyperexpand.py:1799:19: Type `Expr` has no attribute `base`
+ warning[possibly-unbound-attribute] sympy/simplify/tests/test_powsimp.py:133:48: Attribute `subs` on type `Unknown | int` is possibly unbound
+ error[unsupported-operator] sympy/simplify/tests/test_powsimp.py:330:20: Operator `*` is unsupported between objects of type `Unknown | int` and `Rational`
+ error[unsupported-operator] sympy/solvers/tests/test_solveset.py:2207:63: Operator `/` is unsupported between objects of type `Unknown | Integer` and `Unknown | int`
- Found 13236 diagnostics
+ Found 13281 diagnostics

```
</details>


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/snapshots/unpacking.md_-_Unpacking_-_Too_few_values_to_un…_(cef19e6b2b58e6a3).snap`:27 on 2025-06-26 13:27_

These rewordings aren't because I had a strong opinion about the word choice; it's just that we're now using the same display machinery that we were using for e.g. the `index-out-of-bounds` diagnostics when indexing into a tuple.

---

_@dcreager reviewed on 2025-06-26 13:29_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:421 on 2025-06-26 14:42_

I don't love this, but it was necessary to implement `salsa::Update` _only if_ `T` also implements it. `UnionBuilder` does not, and it was looking very invasive to add it.

---

_@dcreager reviewed on 2025-06-26 15:22_

---

_Renamed from "[ty] WIP: Support variable-length tuples in unpacking assignments" to "[ty] Support variable-length tuples in unpacking assignments" by @dcreager on 2025-06-26 15:31_

---

_Comment by @dcreager on 2025-06-26 17:33_

I've spot-checked the new ecosystem results, and they all seem to be coming from unpacking variable-length tuples. For example, there are many new messages involving `Symbol` in `sympy` that come from these [large unpackings](https://github.com/sympy/sympy/blob/c3fc0267e306c858e51edb4723543338d2323027/sympy/abc.py#L67-L69):

```py
a, b, c, d, e, f, g, h, i, j = symbols('a, b, c, d, e, f, g, h, i, j', seq=True)
k, l, m, n, o, p, q, r, s, t = symbols('k, l, m, n, o, p, q, r, s, t', seq=True)
u, v, w, x, y, z = symbols('u, v, w, x, y, z', seq=True)
```

(`symbols` is defined to return either `tuple[Symbol, ...]` or `tuple[Any, ...]`)

---

_Marked ready for review by @dcreager on 2025-06-26 17:34_

---

_Review requested from @carljm by @dcreager on 2025-06-26 17:34_

---

_Review requested from @AlexWaygood by @dcreager on 2025-06-26 17:34_

---

_Review requested from @sharkdp by @dcreager on 2025-06-26 17:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:7220 on 2025-06-26 23:40_

We seem to do this a lot -- should it be a method?

---

_@carljm approved on 2025-06-26 23:53_

Nice!

It feels like a fair amount of complexity was introduced to our tuple representation in order to support it also representing a tuple of union-builders. I think this is probably still simpler / less duplicative than having a separate unpack-targets implementation, but it seems like some of this complexity currently leaks out to external users of tuple types. It seems like that can probably be cleaned up with some additional methods specific to tuples of `Type`?

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-27 08:16_

---

_@carljm approved on 2025-06-27 16:09_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:7220 on 2025-06-27 17:11_

Done — made separate `all_elements` vs `all_elements_with_kind`.  All of the existing callers still use `all_elements`, don't have to call `into_inner` (but do still have to add a `.copied()`)  The new code that needs to distinguish between the different tuple element positions now calls `all_elements_with_kind` and has to match on the result.

---

_@dcreager reviewed on 2025-06-27 17:13_

> It feels like a fair amount of complexity was introduced to our tuple representation in order to support it also representing a tuple of union-builders. I think this is probably still simpler / less duplicative than having a separate unpack-targets implementation, but it seems like some of this complexity currently leaks out to external users of tuple types. It seems like that can probably be cleaned up with some additional methods specific to tuples of `Type`?

I tweaked some things to make this less impactful on the existing `Type`-tuple callers. The main difference now is just that the iterators return `&Type` instead of `Type`, so there are some additional copies.

I also separated out the `unpack_tuple` logic into two steps: resizing the rhs tuple of types to have the same length as the lhs tuple of targets, and then pairwise adding each type to the target `UnionBuilder`. I think the new `resize` method will also be helpful in the forthcoming PR to splat arguments into parameters.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:969 on 2025-06-27 17:15_

Eventually it would be nice to make this work for any `Tuple<T>`, but then you'd have to pass in additional parameters to specify how to combine elements. For now I've just defined it only for `Tuple<Type<'db>>` and hard-coded combining the types by unioning them.

---

_@dcreager reviewed on 2025-06-27 17:15_

---

_@carljm approved on 2025-06-27 18:39_

Nice!

---

_Comment by @codspeed-hq[bot] on 2025-06-27 19:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Funpack-tuple?runnerMode=WallTime)

### Merging #18948 will **improve performances by 4.49%**

<sub>Comparing <code>dcreager/unpack-tuple</code> (ad3fad2) with <code>main</code> (a50a993)</sub>



### Summary

`⚡ 1` improvements  
`✅ 7` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` multithreaded[pydantic] `` | 8.7 s | 8.3 s | +4.49% |


---

_Merged by @dcreager on 2025-06-27 19:29_

---

_Closed by @dcreager on 2025-06-27 19:29_

---

_Branch deleted on 2025-06-27 19:29_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/tuple.rs`:1166 on 2025-07-01 07:03_

This is awesome!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/tuple.rs`:36 on 2025-07-01 07:05_

nit: it might be useful to document what these `usize`s are, especially the ones in `Variable` variant or just a reference to `VariableLengthTuple` to indicate that these are the length of `prefix` and `suffix` respectively

---

_@dhruvmanila reviewed on 2025-07-01 07:11_

---
