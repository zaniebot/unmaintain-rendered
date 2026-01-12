```yaml
number: 21392
title: "[ty] Constraint sets compare generic callables correctly"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/coolable
created_at: 2025-11-11T22:42:41Z
updated_at: 2025-11-17T18:43:40Z
url: https://github.com/astral-sh/ruff/pull/21392
synced_at: 2026-01-12T15:57:22Z
```

# [ty] Constraint sets compare generic callables correctly

---

_@dcreager_

Constraint sets can now track subtyping/assignability/etc of generic callables correctly. For instance:

```py
def identity[T](t: T) -> T:
    return t

constraints = ConstraintSet.always()
static_assert(constraints.implies_subtype_of(TypeOf[identity], Callable[[int], int]))
static_assert(constraints.implies_subtype_of(TypeOf[identity], Callable[[str], str]))
```

A generic callable can be considered an intersection of all of its possible specializations, and an assignability check with an intersection as the lhs side succeeds of _any_ of the intersected types satisfies the check. Put another way, if someone expects to receive any function with a signature of `(int) -> int`, we can give them `identity`.

Note that the corresponding check using `is_subtype_of` directly does not yet work, since #20093 has not yet hooked up the core typing relationship logic to use constraint sets:

```py
# These currently fail
static_assert(is_subtype_of(TypeOf[identity], Callable[[int], int]))
static_assert(is_subtype_of(TypeOf[identity], Callable[[str], str]))
```

To do this, we add a new _existential quantification_ operation on constraint sets. This takes in a list of typevars and _removes_ those typevars from the constraint set. Conceptually, we return a new constraint set that evaluates to `true` when there was _any_ assignment of the removed typevars that caused the old constraint set to evaluate to `true`.

When comparing a generic constraint set, we add its typevars to the `inferable` set, and figure out whatever constraints would allow any specialization to satisfy the check. We then use the new existential quantification operator to remove those new typevars, since the caller doesn't (and shouldn't) know anything about them.

---

_Label `internal` added by @dcreager on 2025-11-11 22:42_

---

_Label `ty` added by @dcreager on 2025-11-11 22:42_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 22:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 22:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/series/test_cumulative.py:48:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:54:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:154:20: error[no-matching-overload] No overload matches arguments
- Found 3224 diagnostics
+ Found 3221 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/unit/test_frame.py:293:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Iterable[Any]] | ndarray[Any, Any] | TypeBlocks | Frame | Series[Any, Any]`, found `None`
+ static_frame/test/unit/test_frame.py:293:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Iterable[Any]] | TypeBlocks | Frame | Series[Any, Any]`, found `None`
- static_frame/test/unit/test_type_blocks.py:31:42: error[invalid-argument-type] Argument to bound method `from_blocks` is incorrect: Expected `ndarray[Any, Any] | Iterable[ndarray[Any, Any]]`, found `tuple[Literal[3], Literal[4]]`
+ static_frame/test/unit/test_type_blocks.py:31:42: error[invalid-argument-type] Argument to bound method `from_blocks` is incorrect: Expected `Iterable[ndarray[Any, Any]]`, found `tuple[Literal[3], Literal[4]]`

sympy (https://github.com/sympy/sympy)
+ sympy/polys/polyclasses.py:1293:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sympy/utilities/tests/test_pickling.py:56:65: error[invalid-argument-type] Argument to function `dumps` is incorrect: Expected `int | None`, found `Unknown | int | ((x: _T@copy) -> _T@copy) | ((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy)`
+ sympy/utilities/tests/test_pickling.py:56:65: error[invalid-argument-type] Argument to function `dumps` is incorrect: Expected `int | None`, found `Unknown | int | ((x: _T@copy) -> _T@copy)`
- sympy/utilities/tests/test_pickling.py:74:46: error[invalid-argument-type] Argument to function `dumps` is incorrect: Expected `int | None`, found `(Unknown & ~(() -> object) & ~ModuleType) | (int & ~(() -> object)) | (((x: _T@copy) -> _T@copy) & ~(() -> object) & ~ModuleType) | (((x: _T@deepcopy, memo: dict[int, Any] | None = None, _nil: Any = list[Unknown]) -> _T@deepcopy) & ~(() -> object) & ~ModuleType)`
+ sympy/utilities/tests/test_pickling.py:74:46: error[invalid-argument-type] Argument to function `dumps` is incorrect: Expected `int | None`, found `(Unknown & ~(() -> object) & ~ModuleType) | (int & ~(() -> object)) | (((x: _T@copy) -> _T@copy) & ~(() -> object) & ~ModuleType)`
- Found 14599 diagnostics
+ Found 14600 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsGt & int`
+ pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `date` on TypedDict `DateSchema`: value of type `int`
- pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsGt & int`
+ pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsGt & int`
+ pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `time` on TypedDict `TimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsGt & int`
+ pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `int`
- pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsGt & int`
+ pydantic/experimental/pipeline.py:443:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `int`
- pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsGt & float`
+ pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `date` on TypedDict `DateSchema`: value of type `float`
- pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsGt & float`
+ pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsGt & float`
+ pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `float`
- pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `int` on TypedDict `IntSchema`: value of type `SupportsGt & float`
+ pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `int` on TypedDict `IntSchema`: value of type `float`
- pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsGt & float`
+ pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `float`
- pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsGt & float`
+ pydantic/experimental/pipeline.py:445:27: error[invalid-assignment] Invalid assignment to key "gt" with declared type `time` on TypedDict `TimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsGe & int`
+ pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `date` on TypedDict `DateSchema`: value of type `int`
- pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsGe & int`
+ pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsGe & int`
+ pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `int`
- pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsGe & int`
+ pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `int`
- pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsGe & int`
+ pydantic/experimental/pipeline.py:459:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `time` on TypedDict `TimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsGe & float`
+ pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `date` on TypedDict `DateSchema`: value of type `float`
- pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsGe & float`
+ pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `int` on TypedDict `IntSchema`: value of type `SupportsGe & float`
+ pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `int` on TypedDict `IntSchema`: value of type `float`
- pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsGe & float`
+ pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `float`
- pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsGe & float`
+ pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `float`
- pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsGe & float`
+ pydantic/experimental/pipeline.py:461:27: error[invalid-assignment] Invalid assignment to key "ge" with declared type `time` on TypedDict `TimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsLt & int`
+ pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `date` on TypedDict `DateSchema`: value of type `int`
- pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsLt & int`
+ pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsLt & int`
+ pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `int`
- pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsLt & int`
+ pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `time` on TypedDict `TimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsLt & int`
+ pydantic/experimental/pipeline.py:474:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `int`
- pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsLt & float`
+ pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsLt & float`
+ pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `float`
- pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsLt & float`
+ pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `float`
- pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsLt & float`
+ pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `date` on TypedDict `DateSchema`: value of type `float`
- pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsLt & float`
+ pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `time` on TypedDict `TimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `int` on TypedDict `IntSchema`: value of type `SupportsLt & float`
+ pydantic/experimental/pipeline.py:476:27: error[invalid-assignment] Invalid assignment to key "lt" with declared type `int` on TypedDict `IntSchema`: value of type `float`
- pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsLe & int`
+ pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `time` on TypedDict `TimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsLe & int`
+ pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `int`
- pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsLe & int`
+ pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `date` on TypedDict `DateSchema`: value of type `int`
- pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsLe & int`
+ pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `int`
- pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsLe & int`
+ pydantic/experimental/pipeline.py:489:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `int`
- pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `date` on TypedDict `DateSchema`: value of type `SupportsLe & float`
+ pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `date` on TypedDict `DateSchema`: value of type `float`
- pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `SupportsLe & float`
+ pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `datetime` on TypedDict `DatetimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `SupportsLe & float`
+ pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `timedelta` on TypedDict `TimedeltaSchema`: value of type `float`
- pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `time` on TypedDict `TimeSchema`: value of type `SupportsLe & float`
+ pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `time` on TypedDict `TimeSchema`: value of type `float`
- pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `int` on TypedDict `IntSchema`: value of type `SupportsLe & float`
+ pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `int` on TypedDict `IntSchema`: value of type `float`
- pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `SupportsLe & float`
+ pydantic/experimental/pipeline.py:491:27: error[invalid-assignment] Invalid assignment to key "le" with declared type `Decimal` on TypedDict `DecimalSchema`: value of type `float`

scipy (https://github.com/scipy/scipy)
- subprojects/highs/src/highspy/highs.py:1185:13: error[invalid-assignment] Object of type `(Iterable[highs_var | highs_linear_expression] & ndarray[object, dtype[object]]) | ndarray[Any, dtype[object_]]` is not assignable to `ndarray[Any, dtype[object_]]`
+ subprojects/highs/src/highspy/highs.py:1185:13: error[invalid-assignment] Object of type `ndarray[object, dtype[object]]` is not assignable to `ndarray[Any, dtype[object_]]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-11-11 22:54_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fcoolable?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21392 will **not alter performance**

<sub>Comparing <code>dcreager/coolable</code> (d8f5876) with <code>main</code> (ac2d07e)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fcoolable?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @dcreager on 2025-11-14 14:01_

---

_Review requested from @carljm by @dcreager on 2025-11-14 14:01_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-14 14:01_

---

_Review requested from @sharkdp by @dcreager on 2025-11-14 14:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:436 on 2025-11-14 22:14_

This seems like a significant issue -- is there a next step / plan for how to fix this TODO?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:1250 on 2025-11-14 22:17_

Hmm, why do we do better in these tests than in the subtype-of tests below? I don't see any gradual forms, so I would think they should have the same results.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:2229 on 2025-11-14 22:18_

I assume that (to the extent the constraint-set versions of these tests pass correctly) we would expect these TODOs to be fixed by https://github.com/astral-sh/ruff/pull/20093 ?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:156 on 2025-11-14 22:26_

I think probably the move would be to define your own iterator type in that case (in which case naming it is easy! `InferableTypeVarsIterator`), rather than trying to name a type composed of double-nested `Either` and boxed/chained iterators :) But I think what you've got here is just fine.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:649 on 2025-11-14 22:30_

Was this just wrong? Or unnecessary

I don't see any new/changed tests above regarding equivalence of callables, but apparently removing this doesn't break any tests?

---

_@carljm approved on 2025-11-14 23:06_

Nice!

---

_@dcreager reviewed on 2025-11-14 23:31_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:436 on 2025-11-14 23:31_

We will need to alpha-rename the typevars that we add to the inferable set when checking generic signatures. I'll add a TODO

---

_@sharkdp reviewed on 2025-11-17 12:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:383 on 2025-11-17 12:42_

I tried to see if tests like the following would pass here …
```py
type GenericIdentity[T] = Callable[[T], T]
type GenericFunction[T, R] = Callable[[T], R]

static_assert(constraints.implies_subtype_of(TypeOf[identity], GenericIdentity))
static_assert(constraints.implies_subtype_of(CallableTypeOf[identity], GenericFunction))
```
… but I get a panic due to a [failing debug assertion](https://github.com/astral-sh/ruff/blob/9269d9a36e04b4908a379ff535e0574d5f906e40/crates/ty_python_semantic/src/types/constraints.rs#L377)


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:1250 on 2025-11-17 14:34_

It's because of these lines:

https://github.com/astral-sh/ruff/blob/9269d9a36e04b4908a379ff535e0574d5f906e40/crates/ty_python_semantic/src/types.rs#L2042-L2045

I can remove the `is_assignable` part of the test, and it does cause the subtype checks to line up with the assignability checks here. But it causes other tests to start failing, because this interacts poorly with the union specialization heuristics here:

https://github.com/astral-sh/ruff/blob/9269d9a36e04b4908a379ff535e0574d5f906e40/crates/ty_python_semantic/src/types/generics.rs#L1445-L1455

Did I mention that the current setup is very brittle? 😅 

I can add a TODO in the code and here calling out that this is a known inconsistency we're working towards removing

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:2229 on 2025-11-17 14:34_

Exactly, that's the patch that would update the "normal" `is_assignable_to` / `is_subtype_of` calls over to using the constraint set machinery for typevars

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:649 on 2025-11-17 14:39_

Bad copy/pasta. I did at one point think that we only needed to force-to-inferable the typevars of one side of the callable comparison, thinking we needed that to handle a co/contravariance thing. But that's already being handled correctly by how we perform the recursive subtyping calls below, so I think it's correct to mark both sides as inferable. (This is a mumbly explanation since much of that detail has paged out of my memory at this point)

But in the world where that hunch was correct, then we wouldn't do _any_ force-to-inferable marking here, because if we only mark one side in `has_relation_to`, and `is_equivalent_to` is akin to `has_relation_to` in both directions, then neither side is always forced-to-inferable. (That inconsistency was another hint that made me think my first hunch was wrong.)

---

_@dcreager reviewed on 2025-11-17 14:39_

---

_@dcreager reviewed on 2025-11-17 16:22_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:383 on 2025-11-17 16:22_

I traced this a bit, and the issue is that [`type_alias.value_type`](https://github.com/astral-sh/ruff/blob/9269d9a36e04b4908a379ff535e0574d5f906e40/crates/ty_python_semantic/src/types.rs#L11629) is returning a dynamic callable for these type aliases. I think that's a separate issue from this PR, though I will add some logic here to make this not cause a panic!

---

_@dcreager reviewed on 2025-11-17 16:28_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:383 on 2025-11-17 16:28_

If you do

```py
static_assert(constraints.implies_subtype_of(TypeOf[identity], GenericIdentity[int]))
```

the test passes. `value_type` is eagerly specializing the type alias to `GenericIdentity[Unknown]`. So other than the panic, this might be the correct expected behavior?

---

_@carljm reviewed on 2025-11-17 16:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:383 on 2025-11-17 16:43_

I think it is correct/expected that a not-explicitly-specialized use of a generic type alias in a type expression is implicitly default-specialized.

---

_@dcreager reviewed on 2025-11-17 18:17_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:383 on 2025-11-17 18:17_

Fixed the panic and added these as additional tests

---

_Merged by @dcreager on 2025-11-17 18:43_

---

_Closed by @dcreager on 2025-11-17 18:43_

---

_Branch deleted on 2025-11-17 18:43_

---
