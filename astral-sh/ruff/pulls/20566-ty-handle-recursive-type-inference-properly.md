```yaml
number: 20566
title: "[ty] handle recursive type inference properly"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: recursive-inference
created_at: 2025-09-25T05:47:21Z
updated_at: 2025-11-27T06:49:33Z
url: https://github.com/astral-sh/ruff/pull/20566
synced_at: 2026-01-12T15:57:05Z
```

# [ty] handle recursive type inference properly

---

_@mtshiba_

## Summary

Derived from #17371

Fixes astral-sh/ty#256
Fixes https://github.com/astral-sh/ty/issues/1415
Fixes https://github.com/astral-sh/ty/issues/1433
Fixes https://github.com/astral-sh/ty/issues/1524

Properly handles any kind of recursive inference and prevents panics.

---

Let me explain techniques for converging fixed-point iterations during recursive type inference.
There are two types of type inference that naively don't converge (causing salsa to panic): divergent type inference and oscillating type inference.

### Divergent type inference

Divergent type inference occurs when eagerly expanding a recursive type. A typical example is this:

```python
class C:
    def f(self, other: "C"):
        self.x = (other.x, 1)

reveal_type(C().x) # revealed: Unknown | tuple[Unknown | tuple[Unknown | tuple[..., Literal[1]], Literal[1]], Literal[1]]
```

To solve this problem, we have already introduced `Divergent` types (https://github.com/astral-sh/ruff/pull/20312). `Divergent` types are treated as a kind of dynamic type [^1].

```python
Unknown | tuple[Unknown | tuple[Unknown | tuple[..., Literal[1]], Literal[1]], Literal[1]]
=> Unknown | tuple[Divergent, Literal[1]]
```

When a query function that returns a type enters a cycle, it sets `Divergent` as the cycle initial value (instead of `Never`). Then, in the cycle recovery function, it reduces the nesting of types containing `Divergent` to converge.

```python
0th: Divergent
1st: Unknown | tuple[Divergent, Literal[1]]
2nd: Unknown | tuple[Unknown | tuple[Divergent, Literal[1]], Literal[1]]
=> Unknown | tuple[Divergent, Literal[1]]
```

Each cycle recovery function for each query should operate only on the `Divergent` type originating from that query.
For this reason, while `Divergent` appears the same as `Any` to the user, it internally carries some information: the location where the cycle occurred. Previously, we roughly identified this by having the scope where the cycle occurred, but with the update to salsa, functions that create cycle initial values ​​can now receive a `salsa::Id` (https://github.com/salsa-rs/salsa/pull/1012). This is an opaque ID that uniquely identifies the cycle head (the query that is the starting point for the fixed-point iteration). `Divergent` now has this `salsa::Id`. 

### Oscillating type inference

Now, another thing to consider is oscillating type inference. Oscillating type inference arises from the fact that monotonicity is broken. Monotonicity here means that for a query function, if it enters a cycle, the calculation must start from a "bottom value" and progress towards the final result with each cycle. Monotonicity breaks down in type systems that have features like overloading and overriding.

```python
class Base:
    def flip(self) -> "Sub":
        return Sub()

class Sub(Base):
    def flip(self) -> "Base":
        return Base()

class C:
    def __init__(self, x: Sub):
        self.x = x

    def replace_with(self, other: "C"):
        self.x = other.x.flip()

reveal_type(C(Sub()).x)
```

Naive fixed-point iteration results in `Divergent -> Sub -> Base -> Sub -> ...`, which oscillates forever without diverging or converging. To address this, the salsa API has been modified so that the cycle recovery function receives the value of the previous cycle (https://github.com/salsa-rs/salsa/pull/1012).
The cycle recovery function returns the union type of the current cycle and the previous cycle. In the above example, the result type for each cycle is `Divergent -> Sub -> Base (= Sub | Base) -> Base`, which converges.

The final result of oscillating type inference does not contain `Divergent` because `Divergent` that appears in a union type can be removed, as is clear from the expansion. This simplification is performed at the same time as nesting reduction.

```
T | Divergent = T | (T | (T | ...)) = T
```

[^1]: In theory, it may be possible to strictly treat types containing `Divergent` types as recursive types, but we probably shouldn't go that deep yet. (AFAIK, there are no PEPs that specify how to handle implicitly recursive types that aren't named by type aliases)

## Performance analysis

A happy side effect of this PR is that we've observed widespread performance improvements!
This is likely due to the removal of the `ITERATIONS_BEFORE_FALLBACK` and max-specialization depth trick (https://github.com/astral-sh/ty/issues/1433, https://github.com/astral-sh/ty/issues/1415), which means we reach a fixed point much sooner.

## Ecosystem analysis

The changes look good overall.
You may notice changes in the converged values ​​for recursive types, this is because the way recursive types are normalized has been changed. Previously, types containing `Divergent` types were normalized by replacing them with the `Divergent` type itself, but in this PR, types with a nesting level of 2 or more that contain `Divergent` types are normalized by replacing them with a type with a nesting level of 1. This means that information about the non-divergent parts of recursive types is no longer lost.

```python
# previous
tuple[tuple[Divergent, int], int] => Divergent
# now
tuple[tuple[Divergent, int], int] => tuple[Divergent, int]
```

The false positive error introduced in this PR occurs in class definitions with self-referential base classes, such as the one below.

```python
from typing_extensions import Generic, TypeVar

T = TypeVar("T")
U = TypeVar("U")

class Base2(Generic[T, U]): ...

# TODO: no error
# error: [unsupported-base] "Unsupported class base with type `<class 'Base2[Sub2, U@Sub2]'> | <class 'Base2[Sub2[Unknown], U@Sub2]'>`"
class Sub2(Base2["Sub2", U]): ...
```

This is due to the lack of support for unions of MROs, or because cyclic legacy generic types are not inferred as generic types early in the query cycle.

## Test Plan

All samples listed in astral-sh/ty#256 are tested and passed without any panic!

## Acknowledgments

Thanks to @MichaReiser for working on bug fixes and improvements to salsa for this PR. @carljm also contributed early on to the discussion of the query convergence mechanism proposed in this PR.

---

_Label `ty` added by @AlexWaygood on 2025-09-25 09:15_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7409 on 2025-10-22 06:56_

It seems we only use the identity of the divergent type and don't care that much about about its content. Could we use a `u64` instead (with a global counter of the last issued id)?

---

_@MichaReiser reviewed on 2025-10-22 06:56_

---

_Comment by @MichaReiser on 2025-10-22 06:56_

What are the salsa features that are required for this PR to land?

---

_Comment by @MichaReiser on 2025-10-22 08:24_

I put up a PR that implements the extensions that I think are needed for this to work:

* Pass the query id to the cycle recovery function. It can be used as a cheap identifier of the divergent value
* Pass the last provisional value to the cycle recovery function

See https://github.com/salsa-rs/salsa/pull/1012/

> Therefore, I think it's necessary to specify an additional value joining operation in the tracked function. For example, like this:

I didn't understand this part or the semantics of the join operation. Specifically, how the join operation is different from `fallback` because `cycle_recovery` already runs **after** the query. It can override the value from this iteration. That's why it isn't clear to me why the fallback implementation can't call the join internally when necessary

With those features in place, is this PR something you could drive forward? I'm asking because we're most likely going to land https://github.com/astral-sh/ruff/pull/20988 as is because it unblocks type of self with a minimal regression but it would be great to have a proper fix in place.


---

_Comment by @MichaReiser on 2025-10-22 09:07_

One thing I wonder. Should queries be allowed to customize `converged` so that we can use an operation other than `Eq`. That would allow us to return `list[Diverged]` as fallback from the cycle recovery function, it would lead to `list[list[Diverged]]` in the next iteration when we have `while True: x = [x]`, but the `has_converged` function could decide to still return `true` in that case, saying that the query converged. We would then still need to replace the query result with `list[Converged]` to ensure the last provisional value and the new value are indeed equal. 

So maybe, `cycle_recovery` should be called for all values and it returns:

* `Converged(T)`: The query has convered, use `T` as the final query result (allows customizing `equal` and overriding the final value to go from `list[list[Converged]]` back to `list[Converged]`)
* `Iterate(T)`: Keep iterating, use the given value (should be `new_value` in the common case)

---

_Comment by @mtshiba on 2025-10-22 09:52_

> I put up a PR that implements the extensions that I think are needed for this to work:

Thanks, it'll be helpful, but I'll have to elaborate on my thoughts.

> I didn't understand this part or the semantics of the join operation. Specifically, how the join operation is different from fallback because cycle_recovery already runs after the query. It can override the value from this iteration. That's why it isn't clear to me why the fallback implementation can't call the join internally when necessary

@carljm had a similar question, and my answer is given [here](https://github.com/astral-sh/ruff/pull/17371#discussion_r2359458856).

> With those features in place, is this PR something you could drive forward?

For the reason stated above, I believe that simply providing a "last provisional value" in the cycle_recovery function will not achieve fully-converged type inference.

>  I'm asking because we're most likely going to land https://github.com/astral-sh/ruff/pull/20988 as is because it unblocks type of self with a minimal regression but it would be great to have a proper fix in place.

In my opinion, there is no need to set a "threshold" at which to give up on further type inference and switch to `Divergent` (for reasons other than performance). That's the purpose of #20566.
However, I think it's OK to merge the PR as a provisional implementation to push forward with the implementation of `self`-related features, as I can create a follow-up PR.

---

_Comment by @mtshiba on 2025-10-22 10:38_

> One thing I wonder. Should queries be allowed to customize `converged` so that we can use an operation other than `Eq`. That would allow us to return `list[Diverged]` as fallback from the cycle recovery function, it would lead to `list[list[Diverged]]` in the next iteration when we have `while True: x = [x]`, but the `has_converged` function could decide to still return `true` in that case, saying that the query converged. We would then still need to replace the query result with `list[Converged]` to ensure the last provisional value and the new value are indeed equal.
> 
> So maybe, `cycle_recovery` should be called for all values and it returns:
> 
> * `Converged(T)`: The query has convered, use `T` as the final query result (allows customizing `equal` and overriding the final value to go from `list[list[Converged]]` back to `list[Converged]`)
> * `Iterate(T)`: Keep iterating, use the given value (should be `new_value` in the common case)

I think that leaving the decision of convergence to the user risks making inference unstable: that is, salsa users may force (or erroneously) declare convergence when it has not actually converged, leading to non-determinism.

---

_Comment by @MichaReiser on 2025-10-22 10:48_

> I think that leaving the decision of convergence to the user risks making inference unstable: that is, salsa users may force (or erroneously) declare convergence when it has not actually converged, leading to non-determinism.

Yeah, agree. But I think we can enable this functionality by comparing if the value returned by `Fallback` is identical with the last provisional and, if that's the case, consider the cycle as converged (which is true, because all queries would read exactly the same value when we iterate again).

> @carljm had a similar question, and my answer is given https://github.com/astral-sh/ruff/pull/17371#discussion_r2359458856.

I read that conversation and it's the part I don't understand. It also seems specific to return type inference because the lattice isn't monotonic. Which seems different from diverging cases that continue going forever.

Edit: Instead of having specific handling in many places. Could the cycle function instead:

If the previous value contains `Divergent`: 

Replace all instance of `previous_value` in `new_value` with `Divergent`

This is a no-op for `Divergent`, but should reduce `list[list[Divergent]]` to `list[Divergent]` if the previous_type was `list[Divergent]` and the new type is `list[list[Divergent]]`. 

The change I proposed in Salsa upstream to consider a cycle head as converged when the `last_provisional_value == Fallback_value` should then mark this cycle head as converged (which might result in the entire cycle to converge, if all other cycle heads have converged too)


---

_Comment by @mtshiba on 2025-10-23 17:36_

> I read that conversation and it's the part I don't understand. It also seems specific to return type inference because the lattice isn't monotonic. Which seems different from diverging cases that continue going forever.
> 
> Edit: Instead of having specific handling in many places. Could the cycle function instead:
> 
> If the previous value contains `Divergent`:
> 
> Replace all instance of `previous_value` in `new_value` with `Divergent`
> 
> This is a no-op for `Divergent`, but should reduce `list[list[Divergent]]` to `list[Divergent]` if the previous_type was `list[Divergent]` and the new type is `list[list[Divergent]]`.
> 
> The change I proposed in Salsa upstream to consider a cycle head as converged when the `last_provisional_value == Fallback_value` should then mark this cycle head as converged (which might result in the entire cycle to converge, if all other cycle heads have converged too)

Consider inferring the return type of the following function:

```python
# 0th: Divergent (cycle_initial)
# 1st: None | tuple[Divergent]
# 2nd: None | tuple[None | tuple[Divergent]]
# ...
def div(x: int):
    if x == 0:
        return None
    else:
        return (div(x-1),)
```

If the values ​​from the previous and current cycles do not match, then the `cycle_recovery` function is called. However, no matter what replacement we make to the current value (the type cached for the next cycle) within `cycle_recovery`, it will not match the result of the next cycle. This is because if the type after replacement is `T`, the type for the next cycle will be `None | tuple[T]`. Therefore, any manipulation of the type to converge type inference must be done before the equality test.
This is why cycle_recovery handling cannot be integrated with divergence suppression handling. As already explained, suppressing oscillating type inference also needs to be done separately from `cycle_recovery`. Oscillating type inference can occur not only in function return type inference as in #17371, but also in implicit instance attribute type inference. For example, this type inference should oscillate if `self` is typed and overload resolution is implemented correctly:

```python
from typing import overload

class A: ...
class B(A): ...

@overload
def flip(x: B) -> A: ...
@overload
def flip(x: A) -> B: ...
def flip(x): ...

class C:
    def __init__(self, x: B):
        self.x = x

    def flip(self):
        # 0th: Divergent
        # 1st: B
        # 2nd: A
        # 3rd: B
        # ...
        self.x = flip(self.x)

reveal_type(C(B()).x)
```

Therefore, I believe the correct approach here is to allow `salsa::tracked` to specify a `cycle_join` function as an argument, as explained in https://github.com/astral-sh/ruff/pull/17371#discussion_r2359458856. This function joins the previous and current values ​​before the equality check.

---

_Comment by @MichaReiser on 2025-10-23 17:40_

> Therefore, I believe the correct approach here is to allow salsa::tracked to specify a cycle_join function as an argument, as explained in https://github.com/astral-sh/ruff/pull/17371#discussion_r2359458856. This function joins the previous and current values ​​before the equality check.

I think the new salsa version supports this now. The `cycle_fn` gets the old and new value and it can return `Fallback(V)`. If `V == last_provisional`, then salsa considers the cycle as converged (which I think is the same as you want with your join function)

https://github.com/salsa-rs/salsa/blob/d38145c29574758de7ffbe8a13cd4584c3b09161/src/function/execute.rs#L350-L360

---

_Comment by @mtshiba on 2025-10-23 17:56_

> > Therefore, I believe the correct approach here is to allow salsa::tracked to specify a cycle_join function as an argument, as explained in [#17371 (comment)](https://github.com/astral-sh/ruff/pull/17371#discussion_r2359458856). This function joins the previous and current values ​​before the equality check.
> 
> I think the new salsa version supports this now. The `cycle_fn` gets the old and new value and it can return `Fallback(V)`. If `V == last_provisional`, then salsa considers the cycle as converged (which I think is the same as you want with your join function)
> 
> https://github.com/salsa-rs/salsa/blob/d38145c29574758de7ffbe8a13cd4584c3b09161/src/function/execute.rs#L350-L360

Ah, so you've made it so that the equality test is performed again after the fallback. I overlooked that. So, I think there's no problem and and I can move this PR forward.

---

_Comment by @github-actions[bot] on 2025-10-27 05:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-26 16:08:51.526490508 +0000
+++ new-output.txt	2025-11-26 16:08:54.894507078 +0000
@@ -1,4 +1,3 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:469:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1a6a3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -72,6 +71,27 @@
 aliases_type_statement.py:79:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
 aliases_type_statement.py:80:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
 aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_type_statement.py:82:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias3`
+aliases_type_statement.py:88:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias6`
+aliases_type_statement.py:89:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias7`
+aliases_typealiastype.py:32:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
+aliases_typealiastype.py:39:26: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_typealiastype.py:52:40: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_typealiastype.py:53:40: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_typealiastype.py:54:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
+aliases_typealiastype.py:54:43: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_typealiastype.py:55:42: error[invalid-type-form] List comprehensions are not allowed in type expressions
+aliases_typealiastype.py:56:42: error[invalid-type-form] Dict literals are not allowed in type expressions
+aliases_typealiastype.py:57:42: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_typealiastype.py:58:42: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
+aliases_typealiastype.py:58:48: error[invalid-type-form] Int literals are not allowed in this context in a type expression
+aliases_typealiastype.py:59:42: error[invalid-type-form] `if` expressions are not allowed in type expressions
+aliases_typealiastype.py:60:42: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
+aliases_typealiastype.py:61:42: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
+aliases_typealiastype.py:62:42: error[invalid-type-form] Int literals are not allowed in this context in a type expression
+aliases_typealiastype.py:63:42: error[invalid-type-form] Boolean operations are not allowed in type expressions
+aliases_typealiastype.py:64:42: error[invalid-type-form] F-strings are not allowed in type expressions
+aliases_typealiastype.py:66:47: error[unresolved-reference] Name `BadAlias21` used when not defined
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
@@ -1011,5 +1031,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1013 diagnostics
-WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
+Found 1033 diagnostics

```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-10-27 05:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20566 will **improve performances by 6.95%**

<sub>Comparing <code>mtshiba:recursive-inference</code> (278607f) with <code>main</code> (adf4f1e)</sub>



### Summary

`⚡ 5` improvements  
`✅ 17` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` ty_micro[complex_constrained_attributes_1] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_1%3A%3Aty_micro%5Bcomplex_constrained_attributes_1%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 70.8 ms | 67.5 ms | +4.91% |
| ⚡ | Simulation | [`` ty_micro[complex_constrained_attributes_2] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_2%3A%3Aty_micro%5Bcomplex_constrained_attributes_2%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 70.6 ms | 67.3 ms | +4.92% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.1 s | 1 s | +4.44% |
| ⚡ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 57.5 s | 55 s | +4.42% |
| ⚡ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 17.4 s | 16.2 s | +6.95% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Arecursive-inference?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2025-10-27 06:21_

Ohh... do we also need to pass the id to the initial function?

---

_Comment by @mtshiba on 2025-10-27 08:04_

> Ohh... do we also need to pass the id to the initial function?

Yes. For now I'm using https://github.com/mtshiba/salsa/tree/input_id?rev=9ea5289bc6a87943b8a8620df8ff429062c56af0, but is there a better way? There will be a lot of changes.

---

_Comment by @MichaReiser on 2025-10-27 08:12_

> Yes. For now I'm using [mtshiba/salsa@input_id?rev=9ea5289bc6a87943b8a8620df8ff429062c56af0](https://github.com/mtshiba/salsa/tree/input_id?rev=9ea5289bc6a87943b8a8620df8ff429062c56af0&rgh-link-date=2025-10-27T08%3A04%3A47Z), but is there a better way? There will be a lot of changes.

No, I don't think there's a better way. It's a breaking change that requires updating all cycle initial functions. Do you want to put up a Salsa PR (Claude's really good at updating the function signatures, but not very fast :))

---

_Comment by @mtshiba on 2025-10-27 08:36_

> > Yes. For now I'm using [mtshiba/salsa@input_id?rev=9ea5289bc6a87943b8a8620df8ff429062c56af0](https://github.com/mtshiba/salsa/tree/input_id?rev=9ea5289bc6a87943b8a8620df8ff429062c56af0&rgh-link-date=2025-10-27T08%3A12%3A11Z), but is there a better way? There will be a lot of changes.
> 
> No, I don't think there's a better way. It's a breaking change that requires updating all cycle initial functions. Do you want to put up a Salsa PR (Claude's really good at updating the function signatures, but not very fast :))

OK, PR submitted (https://github.com/salsa-rs/salsa/pull/1015).

---

_Comment by @mtshiba on 2025-10-27 08:45_

The test results seem to suggest implementation errors, but I also see salsa panics. Is this a bug in salsa?

<details>
<summary>Details</summary>

$ cargo nextest run -p ty_python_semantic --no-fail-fast
warning: unused import: `OsSystem`
   --> crates\ty_python_semantic\src\module_resolver\list.rs:387:49
    |
387 |         DbWithTestSystem, DbWithWritableSystem, OsSystem, SystemPath, SystemPathBuf,
    |                                                 ^^^^^^^^
    |
    = note: `#[warn(unused_imports)]` on by default

warning: unused import: `PythonVersionSource`
   --> crates\ty_python_semantic\src\module_resolver\list.rs:399:33
    |
399 |     use crate::{PythonPlatform, PythonVersionSource, PythonVersionWithSource};
    |                                 ^^^^^^^^^^^^^^^^^^^

warning: `ty_python_semantic` (lib test) generated 2 warnings (run `cargo fix --lib -p ty_python_semantic --tests` to apply 2 suggestions)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.54s
    Starting 548 tests across 3 binaries (37 skipped; run ID: 05eddc2b-381d-4161-9e0e-6ae680a02d5d, nextest profile: default)
        PASS [   0.127s] ty_python_semantic list::tests::can_insert_into_map
        PASS [   0.124s] ty_python_semantic list::tests::can_insert_into_set
        PASS [   0.120s] ty_python_semantic list::tests::can_intersect_maps
        PASS [   0.116s] ty_python_semantic list::tests::can_intersect_sets
        PASS [   0.148s] ty_python_semantic module_resolver::list::tests::adding_file_on_which_module_resolution_depends_invalidates_previously_failing_query_that_now_succeeds
        PASS [   0.144s] ty_python_semantic module_resolver::list::tests::adding_file_to_search_path_with_higher_priority_invalidates_the_query
        PASS [   0.150s] ty_python_semantic module_resolver::list::tests::builtins_custom
        PASS [   0.153s] ty_python_semantic module_resolver::list::tests::adding_file_to_search_path_with_lower_priority_does_not_change_results
        PASS [   0.160s] ty_python_semantic module_resolver::list::tests::builtins_vendored
        PASS [   0.151s] ty_python_semantic module_resolver::list::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query
        PASS [   0.171s] ty_python_semantic module_resolver::list::tests::deleting_pth_file_on_which_module_resolution_depends_invalidates_cache
        PASS [   0.166s] ty_python_semantic module_resolver::list::tests::editable_install_absolute_path
        PASS [   0.180s] ty_python_semantic module_resolver::list::tests::deleting_editable_install_on_which_module_resolution_depends_invalidates_cache
        PASS [   0.174s] ty_python_semantic module_resolver::list::tests::editable_install_pth_file_with_whitespace
        PASS [   0.180s] ty_python_semantic module_resolver::list::tests::editable_install_multiple_pth_files_with_multiple_paths
        PASS [   0.181s] ty_python_semantic module_resolver::list::tests::editable_install_relative_path
        PASS [   0.172s] ty_python_semantic module_resolver::list::tests::first_party_module
        PASS [   0.166s] ty_python_semantic module_resolver::list::tests::first_party_precedence_over_stdlib
        PASS [   0.185s] ty_python_semantic module_resolver::list::tests::editable_installs_into_first_party_search_path
        PASS [   0.198s] ty_python_semantic module_resolver::list::tests::file_to_module_where_one_search_path_is_subdirectory_of_other
        PASS [   0.202s] ty_python_semantic module_resolver::list::tests::module_resolution_paths_cached_between_different_module_resolutions
        PASS [   0.203s] ty_python_semantic module_resolver::list::tests::module_search_path_priority
        PASS [   0.204s] ty_python_semantic module_resolver::list::tests::namespace_package
        PASS [   0.198s] ty_python_semantic module_resolver::list::tests::package_over_module1
        PASS [   0.212s] ty_python_semantic module_resolver::list::tests::namespace_package_precedence
        PASS [   0.200s] ty_python_semantic module_resolver::list::tests::package_over_module2
        PASS [   0.246s] ty_python_semantic module_resolver::list::tests::multiple_site_packages_with_editables
        PASS [   0.042s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_existing_namespace_stdlib_pkg_py38
        PASS [   0.057s] ty_python_semantic module_resolver::module::tests::known_module_roundtrip_from_str
        PASS [   0.213s] ty_python_semantic module_resolver::list::tests::package_priority_over_module
        PASS [   0.236s] ty_python_semantic module_resolver::list::tests::resolve_package
        PASS [   0.243s] ty_python_semantic module_resolver::list::tests::removing_file_on_which_module_resolution_depends_invalidates_previously_successful_query_that_now_fails
        PASS [   0.073s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_existing_namespace_stdlib_pkg_py39
        PASS [   0.066s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_existing_regular_stdlib_pkgs_py39
        PASS [   0.224s] ty_python_semantic module_resolver::list::tests::stdlib_resolution_respects_versions_file_py38_existing_modules
        PASS [   0.085s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_existing_regular_stdlib_pkg_py38
        PASS [   0.077s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_nonexistent_namespace_stdlib_pkg_py38
        PASS [   0.250s] ty_python_semantic module_resolver::list::tests::stdlib
        PASS [   0.071s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_nonexistent_namespace_stdlib_pkg_py39
        PASS [   0.256s] ty_python_semantic module_resolver::list::tests::root_directory_for_search_path_is_okay
        PASS [   0.231s] ty_python_semantic module_resolver::list::tests::stdlib_resolution_respects_versions_file_py38_nonexisting_modules
        PASS [   0.218s] ty_python_semantic module_resolver::list::tests::stdlib_resolution_respects_versions_file_py39_nonexisting_modules
        PASS [   0.224s] ty_python_semantic module_resolver::list::tests::stdlib_resolution_respects_versions_file_py39_existing_modules
        PASS [   0.248s] ty_python_semantic module_resolver::list::tests::stdlib_uses_vendored_typeshed_when_no_custom_typeshed_supplied
        PASS [   0.107s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_nonexistent_regular_stdlib_pkg_py38
        PASS [   0.080s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_nonexistent_single_file_module_py38
        PASS [   0.060s] ty_python_semantic module_resolver::path::tests::module_name_2_parts
        PASS [   0.247s] ty_python_semantic module_resolver::list::tests::stub_file_module_not_allowed
        PASS [   0.073s] ty_python_semantic module_resolver::path::tests::mocked_typeshed_single_file_stdlib_module_py38
        PASS [   0.255s] ty_python_semantic module_resolver::list::tests::stub_package
        PASS [   0.248s] ty_python_semantic module_resolver::list::tests::stub_package_not_allowed_in_typeshed
        PASS [   0.244s] ty_python_semantic module_resolver::list::tests::stub_package_precedence
        PASS [   0.084s] ty_python_semantic module_resolver::path::tests::module_name_3_parts
        PASS [   0.097s] ty_python_semantic module_resolver::path::tests::module_name_1_part
        PASS [   0.078s] ty_python_semantic module_resolver::path::tests::no_strip_with_extension
        PASS [   0.242s] ty_python_semantic module_resolver::list::tests::sub_packages
        PASS [   0.255s] ty_python_semantic module_resolver::list::tests::stubs_over_module_source
        PASS [   0.249s] ty_python_semantic module_resolver::list::tests::stubs_over_package_source
        PASS [   0.068s] ty_python_semantic module_resolver::path::tests::relativize_path
        PASS [   0.073s] ty_python_semantic module_resolver::path::tests::relativize_non_stdlib_path_errors
        PASS [   0.075s] ty_python_semantic module_resolver::path::tests::relativize_stdlib_path_errors
        PASS [   0.060s] ty_python_semantic module_resolver::path::tests::strip_not_top_level_stubs_suffix
        PASS [   0.048s] ty_python_semantic module_resolver::resolver::tests::adding_file_to_search_path_with_lower_priority_does_not_invalidate_query
        PASS [   0.285s] ty_python_semantic module_resolver::list::tests::typing_stub_over_module
        PASS [   0.108s] ty_python_semantic module_resolver::path::tests::strip_top_level_stubs_suffix
        PASS [   0.104s] ty_python_semantic module_resolver::resolver::tests::adding_file_on_which_module_resolution_depends_invalidates_previously_failing_query_that_now_succeeds
        PASS [   0.108s] ty_python_semantic module_resolver::path::tests::with_extension_methods
        PASS [   0.104s] ty_python_semantic module_resolver::resolver::tests::adding_file_to_search_path_with_higher_priority_invalidates_the_query
        PASS [   0.057s] ty_python_semantic module_resolver::resolver::tests::editable_install_multiple_pth_files_with_multiple_paths
        PASS [   0.100s] ty_python_semantic module_resolver::resolver::tests::builtins_custom
        PASS [   0.094s] ty_python_semantic module_resolver::resolver::tests::builtins_vendored
        PASS [   0.102s] ty_python_semantic module_resolver::resolver::tests::deleting_an_unrelated_file_doesnt_change_module_resolution
        PASS [   0.089s] ty_python_semantic module_resolver::resolver::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query
        PASS [   0.096s] ty_python_semantic module_resolver::resolver::tests::deleting_editable_install_on_which_module_resolution_depends_invalidates_cache
        PASS [   0.083s] ty_python_semantic module_resolver::resolver::tests::deleting_pth_file_on_which_module_resolution_depends_invalidates_cache
        PASS [   0.078s] ty_python_semantic module_resolver::resolver::tests::editable_install_absolute_path
        PASS [   0.037s] ty_python_semantic module_resolver::resolver::tests::module_resolution_paths_cached_between_different_module_resolutions
        PASS [   0.072s] ty_python_semantic module_resolver::resolver::tests::editable_install_pth_file_with_whitespace
        PASS [   0.064s] ty_python_semantic module_resolver::resolver::tests::editable_install_relative_path
        PASS [   0.067s] ty_python_semantic module_resolver::resolver::tests::file_to_module_where_one_search_path_is_subdirectory_of_other
        PASS [   0.063s] ty_python_semantic module_resolver::resolver::tests::first_party_module
        PASS [   0.057s] ty_python_semantic module_resolver::resolver::tests::first_party_precedence_over_stdlib
        PASS [   0.100s] ty_python_semantic module_resolver::resolver::tests::module_search_path_priority
        PASS [   0.099s] ty_python_semantic module_resolver::resolver::tests::multiple_site_packages_with_editables
        PASS [   0.094s] ty_python_semantic module_resolver::resolver::tests::no_duplicate_search_paths_added
        PASS [   0.095s] ty_python_semantic module_resolver::resolver::tests::package_priority_over_module
        PASS [   0.059s] ty_python_semantic module_resolver::resolver::tests::stdlib_resolution_respects_versions_file_py39_nonexisting_modules
        PASS [   0.053s] ty_python_semantic module_resolver::resolver::tests::stdlib_uses_vendored_typeshed_when_no_custom_typeshed_supplied
        PASS [   0.094s] ty_python_semantic module_resolver::resolver::tests::removing_file_on_which_module_resolution_depends_invalidates_previously_successful_query_that_now_fails
        PASS [   0.089s] ty_python_semantic module_resolver::resolver::tests::resolve_package
        PASS [   0.090s] ty_python_semantic module_resolver::resolver::tests::stdlib
        PASS [   0.027s] ty_python_semantic module_resolver::typeshed::tests::implicit_submodule_queried_correctly
        PASS [   0.081s] ty_python_semantic module_resolver::resolver::tests::stdlib_resolution_respects_versions_file_py39_existing_modules
        PASS [   0.073s] ty_python_semantic module_resolver::resolver::tests::stubs_over_module_source
        PASS [   0.072s] ty_python_semantic module_resolver::resolver::tests::stubs_over_package_source
        PASS [   0.067s] ty_python_semantic module_resolver::resolver::tests::sub_packages
        PASS [   0.062s] ty_python_semantic module_resolver::resolver::tests::typing_stub_over_module
        PASS [   0.060s] ty_python_semantic module_resolver::typeshed::tests::can_parse_vendored_versions_file
        PASS [   0.118s] ty_python_semantic module_resolver::resolver::tests::stdlib_resolution_respects_versions_file_py38_existing_modules
        PASS [   0.113s] ty_python_semantic module_resolver::resolver::tests::stdlib_resolution_respects_versions_file_py38_nonexisting_modules
        PASS [   0.088s] ty_python_semantic module_resolver::typeshed::tests::explicit_submodule_parsed_correctly
        PASS [   0.082s] ty_python_semantic module_resolver::typeshed::tests::invalid_typeshed_versions_bad_period_number
        PASS [   0.092s] ty_python_semantic module_resolver::typeshed::tests::invalid_typeshed_versions_bad_colon_number
        PASS [   0.094s] ty_python_semantic module_resolver::typeshed::tests::invalid_typeshed_versions_bad_hyphen_number
        PASS [   0.090s] ty_python_semantic module_resolver::typeshed::tests::invalid_typeshed_versions_non_digits
        PASS [   0.081s] ty_python_semantic module_resolver::typeshed::tests::typeshed_versions_consistent_with_vendored_stubs
        PASS [   0.077s] ty_python_semantic module_resolver::typeshed::tests::version_from_range_parsed_correctly
        PASS [   0.080s] ty_python_semantic module_resolver::typeshed::tests::version_within_range_parsed_correctly
        PASS [   0.105s] ty_python_semantic module_resolver::typeshed::tests::invalid_typeshed_versions_non_identifier_modules
        PASS [   0.100s] ty_python_semantic module_resolver::typeshed::tests::nonexistent_module_queried_correctly
        PASS [   0.061s] ty_python_semantic place::tests::test_symbol_or_fall_back_to
        PASS [   0.055s] ty_python_semantic semantic_index::member::tests::test_member_expr_ref_different_segments
        PASS [   0.159s] ty_python_semantic module_resolver::typeshed::tests::can_parse_mock_versions_file
        PASS [   0.040s] ty_python_semantic semantic_index::tests::annotation_only
        PASS [   0.067s] ty_python_semantic semantic_index::member::tests::test_member_expr_ref_parent
        PASS [   0.096s] ty_python_semantic semantic_index::member::tests::test_member_expr_ref_hash_and_eq_small_heap
        PASS [   0.074s] ty_python_semantic semantic_index::tests::augmented_assignment
        PASS [   0.094s] ty_python_semantic semantic_index::member::tests::test_member_expr_small_vs_heap_allocation
        PASS [   0.061s] ty_python_semantic semantic_index::tests::dupes
        PASS [   0.088s] ty_python_semantic semantic_index::tests::assign
        PASS [   0.164s] ty_python_semantic place::implicit_globals::tests::module_type_symbols_includes_declared_types_but_not_referenced_types
        PASS [   0.088s] ty_python_semantic semantic_index::tests::class_scope
        PASS [   0.081s] ty_python_semantic semantic_index::tests::comprehension_scope
        PASS [   0.080s] ty_python_semantic semantic_index::tests::empty
        PASS [   0.073s] ty_python_semantic semantic_index::tests::expression_scope
        PASS [   0.066s] ty_python_semantic semantic_index::tests::for_loops_complex_unpacking
        PASS [   0.078s] ty_python_semantic semantic_index::tests::for_loops_simple_unpacking
        PASS [   0.072s] ty_python_semantic semantic_index::tests::for_loops_single_assignment
        PASS [   0.094s] ty_python_semantic semantic_index::tests::function_parameter_symbols
        PASS [   0.088s] ty_python_semantic semantic_index::tests::function_scope
        PASS [   0.073s] ty_python_semantic semantic_index::tests::generic_function
        PASS [   0.066s] ty_python_semantic semantic_index::tests::import
        PASS [   0.086s] ty_python_semantic semantic_index::tests::generic_class
        PASS [   0.071s] ty_python_semantic semantic_index::tests::import_as
        PASS [   0.059s] ty_python_semantic semantic_index::tests::import_sub
        PASS [   0.065s] ty_python_semantic semantic_index::tests::import_from
        PASS [   0.052s] ty_python_semantic semantic_index::tests::lambda_parameter_symbols
        PASS [   0.058s] ty_python_semantic semantic_index::tests::match_stmt
        PASS [   0.303s] ty_python_semantic module_resolver::typeshed::tests::invalid_huge_versions_file
        PASS [   0.052s] ty_python_semantic semantic_index::tests::multiple_generators
        PASS [   0.079s] ty_python_semantic semantic_index::tests::reachability_trivial
        PASS [   0.090s] ty_python_semantic semantic_index::tests::nested_generators
        PASS [   0.085s] ty_python_semantic semantic_index::tests::nested_match_case
        PASS [   0.056s] ty_python_semantic semantic_index::tests::with_item_unpacked_definition
        PASS [   0.074s] ty_python_semantic semantic_index::tests::scope_iterators
        PASS [   0.062s] ty_python_semantic semantic_index::tests::with_item_definition
        PASS [   0.068s] ty_python_semantic semantic_index::tests::simple
        PASS [   0.099s] ty_python_semantic semantic_index::use_def::place_state::tests::merge
        PASS [   0.093s] ty_python_semantic semantic_index::use_def::place_state::tests::no_declaration
        PASS [   0.087s] ty_python_semantic semantic_index::use_def::place_state::tests::record_constraint
        PASS [   0.083s] ty_python_semantic semantic_index::use_def::place_state::tests::record_declaration
        PASS [   0.077s] ty_python_semantic semantic_index::use_def::place_state::tests::record_declaration_merge
        PASS [   0.081s] ty_python_semantic semantic_index::use_def::place_state::tests::record_declaration_merge_partial_undeclared
        PASS [   0.075s] ty_python_semantic semantic_index::use_def::place_state::tests::record_declaration_override
        PASS [   0.070s] ty_python_semantic semantic_index::use_def::place_state::tests::unbound
        PASS [   0.378s] ty_python_semantic place::tests::implicit_sys_globals
        PASS [   0.049s] ty_python_semantic site_packages::tests::can_find_site_packages_directory_no_virtual_env
        PASS [   0.099s] ty_python_semantic semantic_index::use_def::place_state::tests::with
        PASS [   0.373s] ty_python_semantic place::tests::implicit_typing_extensions_globals
        PASS [   0.385s] ty_python_semantic place::tests::implicit_builtin_globals
        PASS [   0.368s] ty_python_semantic place::tests::implicit_typing_globals
        PASS [   0.091s] ty_python_semantic semantic_model::tests::class_type
        PASS [   0.064s] ty_python_semantic semantic_model::tests::function_type
        PASS [   0.038s] ty_python_semantic site_packages::tests::can_find_site_packages_directory_venv_style_version_field_in_pyvenv_cfg
        PASS [   0.059s] ty_python_semantic site_packages::tests::can_find_site_packages_directory_freethreaded_build
        PASS [   0.052s] ty_python_semantic site_packages::tests::can_find_site_packages_directory_no_virtual_env_freethreaded
        PASS [   0.077s] ty_python_semantic site_packages::tests::can_find_site_packages_directory_uv_style_version_field_in_pyvenv_cfg
        PASS [   0.135s] ty_python_semantic semantic_model::tests::alias_type
        PASS [   0.072s] ty_python_semantic site_packages::tests::can_find_site_packages_directory_virtualenv_style_version_field_in_pyvenv_cfg
        PASS [   0.068s] ty_python_semantic site_packages::tests::cannot_find_site_packages_directory
        PASS [   0.091s] ty_python_semantic site_packages::tests::cannot_find_site_packages_directory_no_virtual_env_at_origin_local_venv
        PASS [   0.087s] ty_python_semantic site_packages::tests::cannot_find_site_packages_directory_no_virtual_env_at_origin_virtual_env_var
        PASS [   0.082s] ty_python_semantic site_packages::tests::cannot_read_lib_directory
        PASS [   0.737s] ty_python_semantic module_resolver::path::tests::stdlib_path_invalid_join_rs
        PASS [   0.079s] ty_python_semantic site_packages::tests::detects_cpython_implementation
        PASS [   0.074s] ty_python_semantic site_packages::tests::detects_graalpy_implementation
        PASS [   0.066s] ty_python_semantic site_packages::tests::detects_unknown_implementation
        LEAK [   0.861s] ty_python_semantic module_resolver::path::tests::non_stdlib_path_invalid_join_rs
        LEAK [   0.166s] ty_python_semantic site_packages::tests::detects_pypy_implementation
        LEAK [   0.158s] ty_python_semantic site_packages::tests::finds_system_site_packages
        PASS [   0.158s] ty_python_semantic site_packages::tests::parsing_pyvenv_cfg_with_invalid_home_key_fails
        LEAK [   0.848s] ty_python_semantic module_resolver::path::tests::stdlib_path_invalid_join_py
        PASS [   0.158s] ty_python_semantic site_packages::tests::parsing_pyvenv_cfg_with_key_but_no_value_fails
        PASS [   0.153s] ty_python_semantic site_packages::tests::parsing_pyvenv_cfg_with_no_home_key_fails
        PASS [   0.133s] ty_python_semantic site_packages::tests::reject_env_that_does_not_exist
        LEAK [   0.148s] ty_python_semantic site_packages::tests::parsing_pyvenv_cfg_with_value_but_no_key_fails
        PASS [   0.127s] ty_python_semantic site_packages::tests::reject_env_that_is_not_a_directory
        PASS [   0.143s] ty_python_semantic site_packages::tests::pyvenv_cfg_with_carriage_return_line_endings_parses
        PASS [   0.146s] ty_python_semantic site_packages::tests::pyvenv_cfg_with_strange_whitespace_parses
        PASS [   0.878s] ty_python_semantic module_resolver::path::tests::too_many_extensions
        PASS [   0.180s] ty_python_semantic subscript::tests::py_index_empty
        PASS [   0.170s] ty_python_semantic subscript::tests::py_slice_mixed_positive_negative_indices
        PASS [   0.195s] ty_python_semantic subscript::tests::py_index_single_element
        PASS [   0.199s] ty_python_semantic subscript::tests::py_index_more_elements
        PASS [   0.184s] ty_python_semantic subscript::tests::py_slice_negative_indices
        PASS [   0.092s] ty_python_semantic subscript::tests::py_slice_single_element_input
        PASS [   0.096s] ty_python_semantic subscript::tests::py_slice_nonnegative_indices
        PASS [   0.210s] ty_python_semantic subscript::tests::py_index_uses_full_index_range
        PASS [   0.204s] ty_python_semantic subscript::tests::py_slice_empty_input
        PASS [   0.099s] ty_python_semantic subscript::tests::py_slice_step_backward
        PASS [   0.095s] ty_python_semantic subscript::tests::py_slice_step_forward
        PASS [   0.068s] ty_python_semantic types::builder::tests::build_intersection_empty_intersection_equals_object
        PASS [   0.140s] ty_python_semantic suppression::tests::invalid_type_ignore_valid_type_ignore
        PASS [   0.065s] ty_python_semantic types::builder::tests::build_union_no_elements
        PASS [   0.059s] ty_python_semantic types::builder::tests::build_union_single_element
        PASS [   0.053s] ty_python_semantic types::builder::tests::build_union_two_elements
        PASS [   0.148s] ty_python_semantic suppression::tests::multiple_type_ignore_comments
        PASS [   0.151s] ty_python_semantic suppression::tests::type_ignore_explanation
        PASS [   0.168s] ty_python_semantic suppression::tests::fmt_comment_before_type_ignore
        PASS [   0.156s] ty_python_semantic suppression::tests::type_ignore_before_fmt_off
        PASS [   0.169s] ty_python_semantic suppression::tests::type_ignore_multiple_codes
        PASS [   0.165s] ty_python_semantic suppression::tests::type_ignore_no_codes
        PASS [   0.060s] ty_python_semantic types::class::tests::known_class_roundtrip_from_str
        PASS [   0.160s] ty_python_semantic suppression::tests::type_ignore_single_code
        PASS [   0.154s] ty_python_semantic suppression::tests::valid_type_ignore_invalid_type_ignore
        PASS [   0.060s] ty_python_semantic types::display::tests::string_literal_display
        PASS [   0.325s] ty_python_semantic types::builder::tests::build_intersection_simplify_split_bool::type_alwaysfalsy_expects
        PASS [   0.332s] ty_python_semantic types::builder::tests::build_intersection_simplify_split_bool::type_booleanliteral_false_expects
        PASS [   0.353s] ty_python_semantic types::builder::tests::build_intersection_simplify_split_bool::type_booleanliteral_true_expects
        PASS [   0.347s] ty_python_semantic types::call::arguments::tests::expand_tuple_type
        PASS [   0.363s] ty_python_semantic types::call::arguments::tests::expand_bool_type
        PASS [   0.418s] ty_python_semantic types::builder::tests::build_intersection_enums
        PASS [   0.295s] ty_python_semantic types::infer::tests::call_type_doesnt_rerun_when_only_callee_changed
        PASS [   0.413s] ty_python_semantic types::builder::tests::build_intersection_simplify_split_bool::type_alwaystruthy_expects
        PASS [   0.349s] ty_python_semantic types::constraints::tests::test_display_graph_output
        PASS [   0.381s] ty_python_semantic types::call::arguments::tests::expand_union_type
        PASS [   0.294s] ty_python_semantic types::infer::tests::dependency_internal_symbol_change
        PASS [   0.305s] ty_python_semantic types::infer::tests::dependency_unrelated_symbol
        PASS [   0.409s] ty_python_semantic types::infer::tests::adding_string_literals_and_literal_string
        PASS [   0.108s] ty_python_semantic types::signatures::tests::empty
        PASS [   0.438s] ty_python_semantic types::infer::tests::dependency_implicit_instance_attribute
        PASS [   0.452s] ty_python_semantic types::infer::tests::dependency_implicit_class_member
        PASS [   0.483s] ty_python_semantic types::display::tests::synthesized_protocol_display
        PASS [   0.430s] ty_python_semantic types::infer::tests::multiplied_literal_string
        PASS [   0.442s] ty_python_semantic types::infer::tests::dependency_public_symbol_type_change
        PASS [   0.474s] ty_python_semantic types::infer::tests::dependency_own_instance_member
        PASS [   0.535s] ty_python_semantic types::display::tests::signature_display
        PASS [   0.540s] ty_python_semantic types::display::tests::signature_display_multiline
        PASS [   0.647s] ty_python_semantic types::class::tests::known_class_doesnt_fallback_to_unknown_unexpectedly_on_latest_version
        PASS [   0.323s] ty_python_semantic types::signatures::tests::deferred_in_stub
        PASS [   0.321s] ty_python_semantic types::signatures::tests::generic_deferred_in_stub
        PASS [   0.334s] ty_python_semantic types::signatures::tests::external_signature_no_decorator
        PASS [   0.454s] ty_python_semantic types::infer::tests::multiplied_string
        PASS [   0.450s] ty_python_semantic types::infer::tests::not_literal_string
        PASS [   0.356s] ty_python_semantic types::signatures::tests::generic_not_deferred
        PASS [   0.416s] ty_python_semantic types::infer::tests::truncated_string_literals_become_literal_string
        PASS [   0.467s] ty_python_semantic types::infer::tests::pep695_type_params
        PASS [   0.396s] ty_python_semantic types::signatures::tests::not_deferred
        PASS [   0.443s] ty_python_semantic types::signatures::tests::full
        PASS [   0.329s] ty_python_semantic types::tests::todo_types
        PASS [   0.550s] ty_python_semantic types::infer::tests::unbound_symbol_no_reachability_constraint_check
        PASS [   0.448s] ty_python_semantic types::tests::no_default_type_is_singleton::pythonversion_py312_expects
        PASS [   0.490s] ty_python_semantic types::tests::divergent_type
        PASS [   0.919s] ty_python_semantic types::function::tests::known_function_roundtrip_from_str
        PASS [   0.452s] ty_python_semantic types::tests::typing_vs_typeshed_no_default
        PASS [   0.481s] ty_python_semantic types::tests::no_default_type_is_singleton::pythonversion_py313_expects
        PASS [   0.712s] ty_python_semantic::corpus linter_no_panic::_a_z_expects
        PASS [   0.685s] ty_python_semantic::mdtest mdtest__annotations_annotated
        PASS [   0.739s] ty_python_semantic::mdtest mdtest__annotations_literal
        PASS [   0.800s] ty_python_semantic::mdtest mdtest__annotations_int_float_complex
        PASS [   1.005s] ty_python_semantic::mdtest mdtest__annotations_any
        PASS [   1.995s] ty_python_semantic types::class::tests::known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version
        PASS [   1.329s] ty_python_semantic::mdtest mdtest__annotations_literal_string
        PASS [   0.840s] ty_python_semantic::mdtest mdtest__annotations_new_types
        FAIL [   1.877s] ty_python_semantic types::tests::type_alias_variance

--- STDOUT:              ty_python_semantic types::tests::type_alias_variance ---

running 1 test
test types::tests::type_alias_variance ... FAILED

failures:

failures:
    types::tests::type_alias_variance

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 294 filtered out; finished in 1.81s


--- STDERR:              ty_python_semantic types::tests::type_alias_variance ---

thread 'types::tests::type_alias_variance' panicked at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\cycle.rs:355:17:
assertion `left == right` failed: Can't merge cycle heads infer_scope_types(Id(818)) with different iteration counts (IterationCount(0), IterationCount(1))
  left: IterationCount(0)
 right: IterationCount(1)
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
   1: core::panicking::panic_fmt
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
   2: core::panicking::assert_failed_inner
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:443
   3: core::panicking::assert_failed<salsa::cycle::IterationCount,salsa::cycle::IterationCount>
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panicking.rs:403
   4: salsa::cycle::CycleHeads::insert
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\cycle.rs:355
   5: salsa::active_query::ActiveQuery::add_read
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\active_query.rs:106
   6: salsa::zalsa_local::impl$0::report_tracked_read::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\zalsa_local.rs:282
   7: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::fetch<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:38
   8: ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:480
   9: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:79
  10: salsa::attach::attach::closure$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:135
  11: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_v
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  12: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_value
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  13: salsa::attach::attach<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:133
  14: ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:470
  15: ty_python_semantic::types::PEP695TypeAliasType::raw_value_type
             at .\src\types.rs:11272
  16: ty_python_semantic::types::PEP695TypeAliasType::value_type
             at .\src\types.rs:11282
  17: enum2$<ty_python_semantic::types::TypeAliasType>::value_type
             at .\src\types.rs:11504
  18: ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl
             at .\src\types\builder.rs:276
  19: ty_python_semantic::types::builder::UnionBuilder::add_in_place
             at .\src\types\builder.rs:256
  20: ty_python_semantic::types::builder::UnionBuilder::add
             at .\src\types\builder.rs:250
  21: ty_python_semantic::types::impl$65::from_elements::closure$0<array$<enum2$<ty_python_semantic::types::Type>,2>,enum2$<ty_python_semantic::types::Type> >
             at .\src\types.rs:11597
  22: core::ops::try_trait::impl$0::wrap_mut_2::closure$0<ty_python_semantic::types::builder::UnionBuilder,ty_python_semantic::types::builder::UnionBuilder,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::impl$65::from_elements::closure_env$0<a
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\try_trait.rs:401
  23: core::array::iter::iter_inner::impl$8::try_fold::closure$0<enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::builder::UnionBuilder,core::ops::try_trait::impl$0::wrap_mut_2::closure_env$0<ty_python_semantic::types::builder::UnionBuilder,ty_
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\array\iter\iter_inner.rs:220
  24: core::ops::index_range::impl$1::try_fold<ty_python_semantic::types::builder::UnionBuilder,core::array::iter::iter_inner::impl$8::try_fold::closure_env$0<enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::builder::UnionBuilder,core::ops::try
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\index_range.rs:169
  25: core::array::iter::impl$4::fold<enum2$<ty_python_semantic::types::Type>,2,ty_python_semantic::types::builder::UnionBuilder,ty_python_semantic::types::impl$65::from_elements::closure_env$0<array$<enum2$<ty_python_semantic::types::Type>,2>,enum2$<ty_python_s
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\array\iter.rs:253
  26: ty_python_semantic::types::UnionType::from_elements<array$<enum2$<ty_python_semantic::types::Type>,2>,enum2$<ty_python_semantic::types::Type> >
             at .\src\types.rs:11596
  27: ty_python_semantic::types::infer::scope_cycle_recover
             at .\src\types\infer.rs:104
  28: ty_python_semantic::types::infer::infer_scope_types::impl$2::recover_from_cycle
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:317
  29: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:344
  30: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:107
  31: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:179
  32: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  33: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_scope_types::infer_s
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1647
  34: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  35: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::fetch<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:31
  36: ty_python_semantic::types::infer::infer_scope_types::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:480
  37: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::ScopeInference>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:79
  38: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:135
  39: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_sco
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  40: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_t
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  41: salsa::attach::attach<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:133
  42: ty_python_semantic::types::infer::infer_scope_types
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:470
  43: ty_python_semantic::types::infer::infer_scope_expression_type<ref$<enum2$<ruff_python_ast::generated::Expr> > >
             at .\src\types\infer.rs:136
  44: ty_python_semantic::types::definition_expression_type
             at .\src\types.rs:224
  45: ty_python_semantic::types::impl$410::raw_value_type::impl$0::raw_value_type_
             at .\src\types.rs:11293
  46: ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::impl$2::execute::inner_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_method_body.rs:24
  47: ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::impl$2::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:302
  48: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::execute_query<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:507
  49: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::execute_maybe_iterate<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:210
  50: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::execute<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:107
  51: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::fetch_cold<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:179
  52: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  53: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::impl$410::raw_valu
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1647
  54: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  55: salsa::function::IngredientImpl<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>::fetch<ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::raw_value_type__Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:31
  56: ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:480
  57: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:79
  58: salsa::attach::attach::closure$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:135
  59: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_v
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  60: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_value
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  61: salsa::attach::attach<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:133
  62: ty_python_semantic::types::impl$410::raw_value_type::raw_value_type_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:470
  63: ty_python_semantic::types::PEP695TypeAliasType::raw_value_type
             at .\src\types.rs:11272
  64: enum2$<ty_python_semantic::types::TypeAliasType>::raw_value_type
             at .\src\types.rs:11511
  65: ty_python_semantic::types::impl$21::variance_of
             at .\src\types.rs:7829
  66: ty_python_semantic::types::tests::type_alias_variance
             at .\src\types.rs:12502
  67: ty_python_semantic::types::tests::type_alias_variance::closure$0
             at .\src\types.rs:12410
  68: core::ops::function::FnOnce::call_once<ty_python_semantic::types::tests::type_alias_variance::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
  69: core::ops::function::FnOnce::call_once
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

        PASS [   0.626s] ty_python_semantic::mdtest mdtest__annotations_starred
        PASS [   0.806s] ty_python_semantic::mdtest mdtest__annotations_optional
        PASS [   1.771s] ty_python_semantic::mdtest mdtest__annotations_deferred
        PASS [   2.129s] ty_python_semantic::corpus linter_no_panic::_a_e_expects
        PASS [   1.610s] ty_python_semantic::mdtest mdtest__annotations_never
        PASS [   0.611s] ty_python_semantic::mdtest mdtest__annotations_union
        PASS [   0.996s] ty_python_semantic::mdtest mdtest__annotations_stdlib_typing_aliases
        PASS [   2.071s] ty_python_semantic::mdtest mdtest__annotations_invalid
        PASS [   0.624s] ty_python_semantic::mdtest mdtest__annotations_unsupported_special_types
        PASS [   2.410s] ty_python_semantic::mdtest mdtest__annotations_callable
        PASS [   0.779s] ty_python_semantic::mdtest mdtest__annotations_unsupported_type_qualifiers
        PASS [   1.083s] ty_python_semantic::mdtest mdtest__annotations_string
        PASS [   2.865s] ty_python_semantic::corpus linter_no_panic::_g_o_expects
        PASS [   0.612s] ty_python_semantic::mdtest mdtest__assignment_unbound
        PASS [   0.917s] ty_python_semantic::mdtest mdtest__assignment_augmented
        PASS [   0.743s] ty_python_semantic::mdtest mdtest__assignment_multi_target
        PASS [   1.299s] ty_python_semantic::mdtest mdtest__annotations_unsupported_special_forms
        PASS [   3.204s] ty_python_semantic::corpus corpus_no_panic
        PASS [   0.765s] ty_python_semantic::mdtest mdtest__assignment_walrus
        PASS [   0.827s] ty_python_semantic::mdtest mdtest__binary_booleans
        PASS [   0.630s] ty_python_semantic::mdtest mdtest__binary_tuples
        PASS [   0.635s] ty_python_semantic::mdtest mdtest__binary_unions
        PASS [   0.642s] ty_python_semantic::mdtest mdtest__boolean_short_circuit
        PASS [   0.865s] ty_python_semantic::mdtest mdtest__binary_integers
        PASS [   1.009s] ty_python_semantic::mdtest mdtest__binary_custom
        PASS [   0.659s] ty_python_semantic::mdtest mdtest__call_annotation
        PASS [   1.518s] ty_python_semantic::mdtest mdtest__binary_classes
        PASS [   1.369s] ty_python_semantic::mdtest mdtest__binary_instances
        PASS [   4.230s] ty_python_semantic::corpus typeshed_no_panic::_f_k_expects
        PASS [   1.252s] ty_python_semantic::mdtest mdtest__boundness_declaredness_public
        PASS [   4.386s] ty_python_semantic::corpus linter_stubs_no_panic
        PASS [   0.895s] ty_python_semantic::mdtest mdtest__call_builtins
        PASS [   0.885s] ty_python_semantic::mdtest mdtest__call_callables_as_descriptors
        PASS [   3.531s] ty_python_semantic::mdtest mdtest__annotations_self
        PASS [   1.046s] ty_python_semantic::mdtest mdtest__call_callable_instance
        PASS [   0.921s] ty_python_semantic::mdtest mdtest__call_dunder
        PASS [   4.744s] ty_python_semantic::corpus typeshed_no_panic::_l_p_expects
        PASS [   0.679s] ty_python_semantic::mdtest mdtest__call_invalid_syntax
        PASS [   0.766s] ty_python_semantic::mdtest mdtest__call_getattr_static
        PASS [   1.351s] ty_python_semantic::mdtest mdtest__call_constructor
        PASS [   0.699s] ty_python_semantic::mdtest mdtest__call_replace
        PASS [   1.270s] ty_python_semantic::mdtest mdtest__call_dunder_import
        PASS [   5.349s] ty_python_semantic::corpus typeshed_no_panic::_a_z_expects
        PASS [   0.662s] ty_python_semantic::mdtest mdtest__call_subclass_of
        PASS [   1.080s] ty_python_semantic::mdtest mdtest__call_never
        PASS [   0.626s] ty_python_semantic::mdtest mdtest__comparison_byte_literals
        PASS [   1.369s] ty_python_semantic::mdtest mdtest__call_open
        PASS [   3.498s] ty_python_semantic::mdtest mdtest__async
        PASS [   5.853s] ty_python_semantic::corpus parser_no_panic
        PASS [   0.647s] ty_python_semantic::mdtest mdtest__comparison_enums
        PASS [   1.276s] ty_python_semantic::mdtest mdtest__call_str_startswith
        PASS [   0.840s] ty_python_semantic::mdtest mdtest__comparison_identity
        PASS [   0.872s] ty_python_semantic::mdtest mdtest__comparison_integers
        PASS [   0.988s] ty_python_semantic::mdtest mdtest__comparison_instances_membership_test
        PASS [   0.542s] ty_python_semantic::mdtest mdtest__comparison_strings
        PASS [   0.762s] ty_python_semantic::mdtest mdtest__comparison_intersections
        PASS [   1.627s] ty_python_semantic::mdtest mdtest__classes
        PASS [   0.911s] ty_python_semantic::mdtest mdtest__comparison_non_bool_returns
        FAIL [   4.152s] ty_python_semantic::mdtest mdtest__bidirectional

--- STDOUT:              ty_python_semantic::mdtest mdtest__bidirectional ---

running 1 test

bidirectional.md - Bidirectional type i… - Propagating target t… (e12a6635e7c4b433)

  crates\ty_python_semantic\resources\mdtest\bidirectional.md:79 unmatched assertion: error: [missing-typed-dict-key] "Missing required key 'x' in TypedDict `TD` constructor"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Propagating target t… (e12a6635e7c4b433)'
MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Propagating target t… (e12a6635e7c4b433)' cargo test -p ty_python_semantic --test mdtest -- mdtest__bidirectional

bidirectional.md - Bidirectional type i… - Propagating return t… (58105b30016e9e2e)

  crates\ty_python_semantic\resources\mdtest\bidirectional.md:105 unexpected error: [invalid-return-type] "Return type does not match returned value: expected `list[dict[Unknown, Unknown]]`, found `list[dict[Unknown, Unknown] & ~AlwaysFalsy]`"
  crates\ty_python_semantic\resources\mdtest\bidirectional.md:111 unexpected error: [invalid-return-type] "Return type does not match returned value: expected `list[dict[Unknown, Unknown]] | None`, found `list[dict[Unknown, Unknown] & ~AlwaysFalsy]`"
  crates\ty_python_semantic\resources\mdtest\bidirectional.md:122 unexpected error: [invalid-return-type] "Return type does not match returned value: expected `list[dict[Unknown, Unknown]]`, found `list[dict[Unknown, Unknown] & ~AlwaysFalsy]`"
  crates\ty_python_semantic\resources\mdtest\bidirectional.md:140 unexpected error: [invalid-return-type] "Return type does not match returned value: expected `list[int | str]`, found `list[Literal[1]]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Propagating return t… (58105b30016e9e2e)'
MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Propagating return t… (58105b30016e9e2e)' cargo test -p ty_python_semantic --test mdtest -- mdtest__bidirectional

bidirectional.md - Bidirectional type i… - Type context sources (778c9046e867adb)

  crates\ty_python_semantic\resources\mdtest\bidirectional.md:200 unexpected error: [invalid-return-type] "Return type does not match returned value: expected `list[Literal[1]]`, found `list[Unknown | int]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Type context sources (778c9046e867adb)'
MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Type context sources (778c9046e867adb)' cargo test -p ty_python_semantic --test mdtest -- mdtest__bidirectional

--------------------------------------------------

test mdtest__bidirectional ... FAILED

failures:

failures:
    mdtest__bidirectional

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 275 filtered out; finished in 4.04s


--- STDERR:              ty_python_semantic::mdtest mdtest__bidirectional ---

thread 'mdtest__bidirectional' panicked at crates\ty_test\src\lib.rs:138:5:
Some tests failed.
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
   1: core::panicking::panic_fmt
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
   2: ty_test::run
             at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:138
   3: mdtest::mdtest
             at .\tests\mdtest.rs:29
   4: mdtest::mdtest__bidirectional
             at .\tests\mdtest.rs:7
   5: mdtest::mdtest__bidirectional::closure$0
             at .\tests\mdtest.rs:10
   6: core::ops::function::FnOnce::call_once<mdtest::mdtest__bidirectional::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
   7: core::ops::function::FnOnce::call_once
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

        PASS [   1.000s] ty_python_semantic::mdtest mdtest__comparison_unions
        PASS [   0.626s] ty_python_semantic::mdtest mdtest__comprehensions_invalid_syntax
        PASS [   0.662s] ty_python_semantic::mdtest mdtest__conditional_if_statement
        PASS [   0.959s] ty_python_semantic::mdtest mdtest__comprehensions_basic
        PASS [   1.762s] ty_python_semantic::mdtest mdtest__comparison_instances_rich_comparison
        PASS [   7.159s] ty_python_semantic::corpus typeshed_no_panic::_a_e_expects
        PASS [   1.248s] ty_python_semantic::mdtest mdtest__comparison_unsupported
        PASS [   0.919s] ty_python_semantic::mdtest mdtest__conditional_if_expression
        PASS [   0.741s] ty_python_semantic::mdtest mdtest__cycle
        PASS [   1.529s] ty_python_semantic::mdtest mdtest__comparison_tuples
        PASS [   0.642s] ty_python_semantic::mdtest mdtest__declaration_error
        PASS [   2.963s] ty_python_semantic::mdtest mdtest__call_union
        PASS [   1.264s] ty_python_semantic::mdtest mdtest__conditional_match
        PASS [   0.886s] ty_python_semantic::mdtest mdtest__decorators
        PASS [   0.784s] ty_python_semantic::mdtest mdtest__del
        PASS [   6.083s] ty_python_semantic::mdtest mdtest__assignment_annotations
        PASS [   1.082s] ty_python_semantic::mdtest mdtest__diagnostics_attribute_assignment
        PASS [   0.932s] ty_python_semantic::mdtest mdtest__diagnostics_legacy_typevars
        PASS [   1.648s] ty_python_semantic::mdtest mdtest__dataclasses_dataclass_transform
        PASS [   0.804s] ty_python_semantic::mdtest mdtest__diagnostics_missing_argument
        PASS [   1.251s] ty_python_semantic::mdtest mdtest__diagnostics_invalid_argument_type
        PASS [   1.256s] ty_python_semantic::mdtest mdtest__diagnostics_invalid_await
        PASS [   3.731s] ty_python_semantic::mdtest mdtest__class_super
        PASS [   1.771s] ty_python_semantic::mdtest mdtest__dataclasses_fields
        PASS [   0.817s] ty_python_semantic::mdtest mdtest__diagnostics_shadowing
        PASS [   0.988s] ty_python_semantic::mdtest mdtest__diagnostics_same_names
        PASS [   1.597s] ty_python_semantic::mdtest mdtest__descriptor_protocol
        PASS [   1.058s] ty_python_semantic::mdtest mdtest__diagnostics_no_matching_overload
        PASS [   0.046s] ty_python_semantic::mdtest mdtest__doc_README
        PASS [   0.891s] ty_python_semantic::mdtest mdtest__diagnostics_single_matching_overload
        PASS [   0.776s] ty_python_semantic::mdtest mdtest__diagnostics_unknown_argument
        PASS [   0.861s] ty_python_semantic::mdtest mdtest__diagnostics_unpacking
        PASS [   1.019s] ty_python_semantic::mdtest mdtest__diagnostics_too_many_positionals
        PASS [   5.024s] ty_python_semantic::mdtest mdtest__call_methods
        PASS [   0.736s] ty_python_semantic::mdtest mdtest__diagnostics_unresolved_reference
        PASS [   2.193s] ty_python_semantic::mdtest mdtest__deprecated
        PASS [   0.883s] ty_python_semantic::mdtest mdtest__diagnostics_unresolved_import
        PASS [   9.349s] ty_python_semantic::corpus typeshed_no_panic::_q_z_expects
        PASS [   1.128s] ty_python_semantic::mdtest mdtest__diagnostics_union_call
        PASS [   0.872s] ty_python_semantic::mdtest mdtest__diagnostics_unsupported_bool_conversion
        PASS [   0.821s] ty_python_semantic::mdtest mdtest__directives_assert_never
        PASS [   5.404s] ty_python_semantic::mdtest mdtest__call_function
        PASS [   0.848s] ty_python_semantic::mdtest mdtest__directives_cast
        PASS [   0.848s] ty_python_semantic::mdtest mdtest__doc_public_type_undeclared_symbols
        PASS [   1.031s] ty_python_semantic::mdtest mdtest__directives_assert_type
        PASS [   0.593s] ty_python_semantic::mdtest mdtest__exception_invalid_syntax
        PASS [   1.251s] ty_python_semantic::mdtest mdtest__diagnostics_version_related_syntax_errors
        PASS [   0.595s] ty_python_semantic::mdtest mdtest__expression_assert
        PASS [   0.729s] ty_python_semantic::mdtest mdtest__exception_except_star
        PASS [   0.587s] ty_python_semantic::mdtest mdtest__expression_if
        PASS [   0.125s] ty_python_semantic::mdtest mdtest__generics_builtins
        PASS [   0.670s] ty_python_semantic::mdtest mdtest__expression_attribute
        PASS [   0.823s] ty_python_semantic::mdtest mdtest__exception_control_flow
        PASS [   5.571s] ty_python_semantic::mdtest mdtest__call_overloads
        PASS [   0.508s] ty_python_semantic::mdtest mdtest__final
        PASS [   0.760s] ty_python_semantic::mdtest mdtest__expression_lambda
        PASS [   0.806s] ty_python_semantic::mdtest mdtest__expression_yield_and_yield_from
        PASS [   0.897s] ty_python_semantic::mdtest mdtest__expression_len
        FAIL [   8.240s] ty_python_semantic::mdtest mdtest__attributes

--- STDOUT:              ty_python_semantic::mdtest mdtest__attributes ---

running 1 test

attributes.md - Attributes - Instance attribute e… - Cyclic implicit attr… (f2becdccbab6c9d9)

  crates\ty_python_semantic\resources\mdtest\attributes.md:2432 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2432 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"
  crates\ty_python_semantic\resources\mdtest\attributes.md:2433 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2433 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"
  crates\ty_python_semantic\resources\mdtest\attributes.md:2434 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2434 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"
  crates\ty_python_semantic\resources\mdtest\attributes.md:2435 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2435 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"
  crates\ty_python_semantic\resources\mdtest\attributes.md:2436 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2436 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"
  crates\ty_python_semantic\resources\mdtest\attributes.md:2437 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2437 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"
  crates\ty_python_semantic\resources\mdtest\attributes.md:2438 unmatched assertion: revealed: Unknown | int
  crates\ty_python_semantic\resources\mdtest\attributes.md:2438 unexpected error: 21 [revealed-type] "Revealed type: `Unknown | Divergent | Divergent | int`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='attributes.md - Attributes - Instance attribute e… - Cyclic implicit attr… (f2becdccbab6c9d9)'
MDTEST_TEST_FILTER='attributes.md - Attributes - Instance attribute e… - Cyclic implicit attr… (f2becdccbab6c9d9)' cargo test -p ty_python_semantic --test mdtest -- mdtest__attributes

attributes.md - Attributes - Divergent inferred i… (e43b190970d91af3)

  crates\ty_python_semantic\resources\mdtest\attributes.md:2666 unmatched assertion: revealed: Unknown | tuple[Divergent, ...]
  crates\ty_python_semantic\resources\mdtest\attributes.md:2666 unexpected error: 13 [revealed-type] "Revealed type: `Unknown | tuple[Divergent | Unknown | tuple[Divergent, ...] | tuple[Divergent, Divergent], ...]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='attributes.md - Attributes - Divergent inferred i… (e43b190970d91af3)'
MDTEST_TEST_FILTER='attributes.md - Attributes - Divergent inferred i… (e43b190970d91af3)' cargo test -p ty_python_semantic --test mdtest -- mdtest__attributes

--------------------------------------------------

test mdtest__attributes ... FAILED

failures:

failures:
    mdtest__attributes

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 275 filtered out; finished in 8.11s


--- STDERR:              ty_python_semantic::mdtest mdtest__attributes ---

thread 'mdtest__attributes' panicked at crates\ty_test\src\lib.rs:138:5:
Some tests failed.
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
   1: core::panicking::panic_fmt
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
   2: ty_test::run
             at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:138
   3: mdtest::mdtest
             at .\tests\mdtest.rs:29
   4: mdtest::mdtest__attributes
             at .\tests\mdtest.rs:7
   5: mdtest::mdtest__attributes::closure$0
             at .\tests\mdtest.rs:10
   6: core::ops::function::FnOnce::call_once<mdtest::mdtest__attributes::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
   7: core::ops::function::FnOnce::call_once
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

        PASS [   0.688s] ty_python_semantic::mdtest mdtest__generics_legacy_variance
        PASS [   0.778s] ty_python_semantic::mdtest mdtest__generics_pep695_aliases
        PASS [   1.276s] ty_python_semantic::mdtest mdtest__function_parameters
        PASS [   1.729s] ty_python_semantic::mdtest mdtest__exhaustiveness_checking
        PASS [   1.941s] ty_python_semantic::mdtest mdtest__exception_basic
        PASS [   0.654s] ty_python_semantic::mdtest mdtest__import_builtins
        PASS [   1.331s] ty_python_semantic::mdtest mdtest__generics_pep695_variables
        PASS [   1.210s] ty_python_semantic::mdtest mdtest__generics_scoping
        PASS [   2.131s] ty_python_semantic::mdtest mdtest__expression_boolean
        PASS [  11.596s] ty_python_semantic::corpus linter_no_panic::_q_z_expects
        PASS [   1.503s] ty_python_semantic::mdtest mdtest__generics_pep695_functions
        PASS [   0.535s] ty_python_semantic::mdtest mdtest__import_conflicts
        PASS [   1.142s] ty_python_semantic::mdtest mdtest__implicit_type_aliases
        PASS [   1.566s] ty_python_semantic::mdtest mdtest__generics_pep695_classes
        PASS [   0.737s] ty_python_semantic::mdtest mdtest__import_conditional
        PASS [   0.580s] ty_python_semantic::mdtest mdtest__import_cyclic
        PASS [   0.511s] ty_python_semantic::mdtest mdtest__import_invalid_syntax
        PASS [   0.652s] ty_python_semantic::mdtest mdtest__import_errors
        PASS [   0.650s] ty_python_semantic::mdtest mdtest__import_module_getattr
        PASS [   2.361s] ty_python_semantic::mdtest mdtest__generics_legacy_functions
        PASS [   0.741s] ty_python_semantic::mdtest mdtest__import_namespace
        PASS [   1.727s] ty_python_semantic::mdtest mdtest__import_basic
        PASS [   0.867s] ty_python_semantic::mdtest mdtest__import_partial_stub_packages
        PASS [   0.829s] ty_python_semantic::mdtest mdtest__import_site_packages_discovery
        PASS [   0.996s] ty_python_semantic::mdtest mdtest__import_legacy_namespace
        PASS [   2.616s] ty_python_semantic::mdtest mdtest__generics_legacy_variables
        PASS [   0.612s] ty_python_semantic::mdtest mdtest__import_stubs
        PASS [   0.610s] ty_python_semantic::mdtest mdtest__import_tracking
        PASS [   1.805s] ty_python_semantic::mdtest mdtest__import_conventions
        PASS [   2.134s] ty_python_semantic::mdtest mdtest__import_case_sensitive
        PASS [   1.341s] ty_python_semantic::mdtest mdtest__import_relative
        PASS [   0.739s] ty_python_semantic::mdtest mdtest__invalid_syntax
        PASS [   0.543s] ty_python_semantic::mdtest mdtest__literal_bytes
        PASS [   0.653s] ty_python_semantic::mdtest mdtest__known_constants
        PASS [   0.669s] ty_python_semantic::mdtest mdtest__literal_boolean
        PASS [   1.163s] ty_python_semantic::mdtest mdtest__import_stub_packages
        FAIL [   3.366s] ty_python_semantic::mdtest mdtest__generics_legacy_classes

--- STDOUT:              ty_python_semantic::mdtest mdtest__generics_legacy_classes ---

running 1 test

classes.md - Generic classes: Leg… - Cyclic class definit… - Cyclic inheritance a… (eb0c5f5a01f786d2)

  crates\ty_python_semantic\resources\mdtest\generics\legacy\classes.md:737 unexpected error: [unsupported-base] "Unsupported class base with type `<class 'list[Derived[T@Derived]]'> | <class 'list[@Todo(specialized non-generic class)]'>`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='classes.md - Generic classes: Leg… - Cyclic class definit… - Cyclic inheritance a… (eb0c5f5a01f786d2)'
MDTEST_TEST_FILTER='classes.md - Generic classes: Leg… - Cyclic class definit… - Cyclic inheritance a… (eb0c5f5a01f786d2)' cargo test -p ty_python_semantic --test mdtest -- mdtest__generics_legacy_classes

--------------------------------------------------

test mdtest__generics_legacy_classes ... FAILED

failures:

failures:
    mdtest__generics_legacy_classes

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 275 filtered out; finished in 3.29s


--- STDERR:              ty_python_semantic::mdtest mdtest__generics_legacy_classes ---

thread 'mdtest__generics_legacy_classes' panicked at crates\ty_test\src\lib.rs:138:5:
Some tests failed.
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
   1: core::panicking::panic_fmt
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
   2: ty_test::run
             at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:138
   3: mdtest::mdtest
             at .\tests\mdtest.rs:29
   4: mdtest::mdtest__generics_legacy_classes
             at .\tests\mdtest.rs:7
   5: mdtest::mdtest__generics_legacy_classes::closure$0
             at .\tests\mdtest.rs:10
   6: core::ops::function::FnOnce::call_once<mdtest::mdtest__generics_legacy_classes::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
   7: core::ops::function::FnOnce::call_once
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

        PASS [   3.194s] ty_python_semantic::mdtest mdtest__generics_pep695_variance
        PASS [   0.720s] ty_python_semantic::mdtest mdtest__literal_collections_dictionary
        PASS [   0.610s] ty_python_semantic::mdtest mdtest__literal_collections_generator_expressions
        PASS [   1.106s] ty_python_semantic::mdtest mdtest__instance_layout_conflict
        PASS [   0.560s] ty_python_semantic::mdtest mdtest__literal_collections_tuple
        PASS [   6.815s] ty_python_semantic::mdtest mdtest__dataclasses_dataclasses
        PASS [   0.719s] ty_python_semantic::mdtest mdtest__literal_collections_list
        PASS [   0.102s] ty_python_semantic::mdtest mdtest__mdtest_custom_typeshed
        PASS [   5.734s] ty_python_semantic::mdtest mdtest__diagnostics_semantic_syntax_errors
        PASS [   0.669s] ty_python_semantic::mdtest mdtest__literal_complex
        PASS [   0.770s] ty_python_semantic::mdtest mdtest__literal_collections_set
        PASS [   0.617s] ty_python_semantic::mdtest mdtest__literal_float
        PASS [   0.634s] ty_python_semantic::mdtest mdtest__literal_f_string
        PASS [   0.548s] ty_python_semantic::mdtest mdtest__literal_string
        PASS [   0.683s] ty_python_semantic::mdtest mdtest__literal_integer
        PASS [   0.922s] ty_python_semantic::mdtest mdtest__literal_ellipsis
        PASS [   0.664s] ty_python_semantic::mdtest mdtest__loops_iterators
        PASS [   0.826s] ty_python_semantic::mdtest mdtest__loops_while_loop
        PASS [   0.561s] ty_python_semantic::mdtest mdtest__narrow_bool_call
        PASS [   1.055s] ty_python_semantic::mdtest mdtest__loops_async_for
        PASS [   0.748s] ty_python_semantic::mdtest mdtest__narrow_assert
        PASS [   2.205s] ty_python_semantic::mdtest mdtest__intersection_types
        PASS [   0.819s] ty_python_semantic::mdtest mdtest__narrow_boolean
        PASS [   0.686s] ty_python_semantic::mdtest mdtest__narrow_conditionals_elif_else
        PASS [   5.661s] ty_python_semantic::mdtest mdtest__enums
        PASS [   3.219s] ty_python_semantic::mdtest mdtest__import_dunder_all
        PASS [   1.032s] ty_python_semantic::mdtest mdtest__narrow_assignment
        PASS [   0.799s] ty_python_semantic::mdtest mdtest__narrow_conditionals_in
        PASS [   1.036s] ty_python_semantic::mdtest mdtest__narrow_conditionals_boolean
        PASS [   0.852s] ty_python_semantic::mdtest mdtest__narrow_conditionals_eq
        PASS [   0.673s] ty_python_semantic::mdtest mdtest__narrow_conditionals_is_not
        PASS [   1.665s] ty_python_semantic::mdtest mdtest__loops_for
        PASS [   0.684s] ty_python_semantic::mdtest mdtest__narrow_conditionals_not
        PASS [   4.723s] ty_python_semantic::mdtest mdtest__ide_support_all_members
        PASS [   0.644s] ty_python_semantic::mdtest mdtest__narrow_hasattr
        PASS [   0.642s] ty_python_semantic::mdtest mdtest__narrow_post_if_statement
        PASS [   1.851s] ty_python_semantic::mdtest mdtest__mro
        PASS [   1.675s] ty_python_semantic::mdtest mdtest__narrow_complex_target
        PASS [   0.848s] ty_python_semantic::mdtest mdtest__narrow_match
        PASS [   0.635s] ty_python_semantic::mdtest mdtest__narrow_while
        PASS [   0.609s] ty_python_semantic::mdtest mdtest__pep613_type_aliases
        PASS [   1.372s] ty_python_semantic::mdtest mdtest__narrow_conditionals_nested
        PASS [   1.065s] ty_python_semantic::mdtest mdtest__narrow_truthiness
        PASS [   1.586s] ty_python_semantic::mdtest mdtest__narrow_conditionals_is
        PASS [   2.193s] ty_python_semantic::mdtest mdtest__named_tuple
        PASS [   2.404s] ty_python_semantic::mdtest mdtest__mdtest_config
        PASS [   0.604s] ty_python_semantic::mdtest mdtest__regression_14334_diagnostics_in_wrong_file
        PASS [   1.477s] ty_python_semantic::mdtest mdtest__narrow_isinstance
        PASS [   1.466s] ty_python_semantic::mdtest mdtest__narrow_issubclass
        PASS [   0.577s] ty_python_semantic::mdtest mdtest__scopes_builtin
        PASS [   2.577s] ty_python_semantic::mdtest mdtest__metaclass
        PASS [   0.834s] ty_python_semantic::mdtest mdtest__public_types
        PASS [   1.012s] ty_python_semantic::mdtest mdtest__properties
        PASS [   0.630s] ty_python_semantic::mdtest mdtest__scopes_global_constants
        PASS [   1.046s] ty_python_semantic::mdtest mdtest__regression_1377_iteration_count_mismatch
        PASS [   0.683s] ty_python_semantic::mdtest mdtest__scopes_unbound
        PASS [   0.524s] ty_python_semantic::mdtest mdtest__shadowing_variable_declaration
        PASS [   0.878s] ty_python_semantic::mdtest mdtest__scopes_global
        PASS [   1.164s] ty_python_semantic::mdtest mdtest__scopes_annotate_global
        PASS [   0.588s] ty_python_semantic::mdtest mdtest__shadowing_class
        PASS [   0.847s] ty_python_semantic::mdtest mdtest__scopes_moduletype_attrs
        PASS [   0.603s] ty_python_semantic::mdtest mdtest__shadowing_function
        PASS [   0.535s] ty_python_semantic::mdtest mdtest__stubs_locals
        PASS [   0.960s] ty_python_semantic::mdtest mdtest__scopes_nonlocal
        PASS [   0.653s] ty_python_semantic::mdtest mdtest__stubs_ellipsis
        PASS [   1.984s] ty_python_semantic::mdtest mdtest__narrow_type_guards
        PASS [  17.021s] ty_python_semantic::corpus linter_no_panic::_p_expects
        PASS [   0.677s] ty_python_semantic::mdtest mdtest__subscript_bytes
        PASS [   0.574s] ty_python_semantic::mdtest mdtest__subscript_stepsize_zero
        PASS [   0.728s] ty_python_semantic::mdtest mdtest__subscript_class
        PASS [   0.737s] ty_python_semantic::mdtest mdtest__subscript_instance
        PASS [   0.062s] ty_python_semantic::mdtest mdtest__type_compendium_README
        PASS [   1.134s] ty_python_semantic::mdtest mdtest__stubs_class
        PASS [   0.661s] ty_python_semantic::mdtest mdtest__subscript_string
        PASS [   1.715s] ty_python_semantic::mdtest mdtest__scopes_eager
        PASS [   0.761s] ty_python_semantic::mdtest mdtest__subscript_lists
        PASS [   2.542s] ty_python_semantic::mdtest mdtest__narrow_type
        PASS [   0.578s] ty_python_semantic::mdtest mdtest__t_strings
        PASS [   0.659s] ty_python_semantic::mdtest mdtest__sys_platform
        PASS [   0.807s] ty_python_semantic::mdtest mdtest__suppressions_no_type_check
        PASS [   2.457s] ty_python_semantic::mdtest mdtest__pep695_type_aliases
        PASS [   0.866s] ty_python_semantic::mdtest mdtest__suppressions_ty_ignore
        PASS [   0.880s] ty_python_semantic::mdtest mdtest__suppressions_type_ignore
        PASS [   0.806s] ty_python_semantic::mdtest mdtest__sys_version_info
        PASS [   0.682s] ty_python_semantic::mdtest mdtest__type_compendium_always_truthy_falsy
        PASS [   0.653s] ty_python_semantic::mdtest mdtest__type_compendium_any
        PASS [   0.639s] ty_python_semantic::mdtest mdtest__type_compendium_none
        PASS [   0.629s] ty_python_semantic::mdtest mdtest__type_compendium_object
        PASS [   0.559s] ty_python_semantic::mdtest mdtest__type_of_typing_dot_Type
        PASS [   1.050s] ty_python_semantic::mdtest mdtest__terminal_statements
        PASS [   0.721s] ty_python_semantic::mdtest mdtest__type_compendium_not_t
        PASS [   6.100s] ty_python_semantic::mdtest mdtest__import_star
        PASS [   0.802s] ty_python_semantic::mdtest mdtest__type_compendium_never
        PASS [   0.716s] ty_python_semantic::mdtest mdtest__type_of_dynamic
        PASS [   0.977s] ty_python_semantic::mdtest mdtest__type_properties_constraints
        PASS [   0.655s] ty_python_semantic::mdtest mdtest__type_properties_is_single_valued
        PASS [   0.628s] ty_python_semantic::mdtest mdtest__type_properties_tuples_containing_never
        PASS [   0.668s] ty_python_semantic::mdtest mdtest__type_properties_str_repr
        PASS [   0.653s] ty_python_semantic::mdtest mdtest__type_qualifiers_initvar
        PASS [   3.991s] ty_python_semantic::mdtest mdtest__overloads
        PASS [   1.606s] ty_python_semantic::mdtest mdtest__type_of_basic
        PASS [   1.973s] ty_python_semantic::mdtest mdtest__ty_extensions
        PASS [   2.989s] ty_python_semantic::mdtest mdtest__statically_known_branches
        PASS [   1.095s] ty_python_semantic::mdtest mdtest__type_qualifiers_classvar
        PASS [   0.515s] ty_python_semantic::mdtest mdtest__unary_integers
        PASS [   0.536s] ty_python_semantic::mdtest mdtest__unary_invert_add_usub
        PASS [   0.689s] ty_python_semantic::mdtest mdtest__unary_custom
        PASS [   2.258s] ty_python_semantic::mdtest mdtest__type_compendium_integer_literals
        PASS [   0.895s] ty_python_semantic::mdtest mdtest__unary_not
        PASS [   1.665s] ty_python_semantic::mdtest mdtest__type_properties_truthiness
        PASS [   0.886s] ty_python_semantic::mdtest mdtest__with_sync
        PASS [   1.966s] ty_python_semantic::mdtest mdtest__type_qualifiers_final
        PASS [  20.278s] ty_python_semantic::corpus linter_no_panic::_f_expects
        PASS [   2.803s] ty_python_semantic::mdtest mdtest__type_compendium_tuple
        PASS [   2.673s] ty_python_semantic::mdtest mdtest__type_properties_is_equivalent_to
        PASS [   2.841s] ty_python_semantic::mdtest mdtest__type_properties_is_subtype_of
        PASS [   4.144s] ty_python_semantic::mdtest mdtest__subscript_tuple
        PASS [   1.755s] ty_python_semantic::mdtest mdtest__unreachable
        PASS [   1.972s] ty_python_semantic::mdtest mdtest__union_types
        PASS [   2.976s] ty_python_semantic::mdtest mdtest__type_properties_is_singleton
        PASS [   3.105s] ty_python_semantic::mdtest mdtest__type_properties_materialization
        PASS [   2.684s] ty_python_semantic::mdtest mdtest__typed_dict
        PASS [   3.730s] ty_python_semantic::mdtest mdtest__type_properties_is_disjoint_from
        PASS [   4.003s] ty_python_semantic::mdtest mdtest__type_properties_is_assignable_to
        FAIL [  11.899s] ty_python_semantic::mdtest mdtest__function_return_type

--- STDOUT:              ty_python_semantic::mdtest mdtest__function_return_type ---

running 1 test

return_type.md - Function return type - Inferred return type - Free function (eef71d295c8194aa)

  crates\ty_python_semantic\resources\mdtest\function\return_type.md:393 unmatched assertion: revealed: list[Divergent] | list[int]
  crates\ty_python_semantic\resources\mdtest\function\return_type.md:393 unexpected error: 13 [revealed-type] "Revealed type: `list[Divergent] | list[Divergent] | list[int]`"
  crates\ty_python_semantic\resources\mdtest\function\return_type.md:418 unmatched assertion: revealed: Never
  crates\ty_python_semantic\resources\mdtest\function\return_type.md:418 unexpected error: 13 [revealed-type] "Revealed type: `Divergent`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='return_type.md - Function return type - Inferred return type - Free function (eef71d295c8194aa)'
MDTEST_TEST_FILTER='return_type.md - Function return type - Inferred return type - Free function (eef71d295c8194aa)' cargo test -p ty_python_semantic --test mdtest -- mdtest__function_return_type

--------------------------------------------------

test mdtest__function_return_type ... FAILED

failures:

failures:
    mdtest__function_return_type

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 275 filtered out; finished in 11.83s


--- STDERR:              ty_python_semantic::mdtest mdtest__function_return_type ---

thread 'mdtest__function_return_type' panicked at crates\ty_test\src\lib.rs:138:5:
Some tests failed.
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
   1: core::panicking::panic_fmt
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
   2: ty_test::run
             at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:138
   3: mdtest::mdtest
             at .\tests\mdtest.rs:29
   4: mdtest::mdtest__function_return_type
             at .\tests\mdtest.rs:7
   5: mdtest::mdtest__function_return_type::closure$0
             at .\tests\mdtest.rs:10
   6: core::ops::function::FnOnce::call_once<mdtest::mdtest__function_return_type::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
   7: core::ops::function::FnOnce::call_once
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

        PASS [   2.822s] ty_python_semantic::mdtest mdtest__unpacking
        PASS [   2.963s] ty_python_semantic::mdtest mdtest__with_async
        FAIL [   8.557s] ty_python_semantic::mdtest mdtest__protocols

--- STDOUT:              ty_python_semantic::mdtest mdtest__protocols ---

running 1 test

protocols.md - Protocols - Recursive protocols - Recursive generic pr… (e02160e5ca187a06)

  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 Attempting to run mdtest caused a panic at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\zalsa_local.rs:731:9
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 assertion failed: extra.iteration.load() <= iteration_count
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    0: std::backtrace_rs::backtrace::win64::trace
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\..\..\backtrace\src\backtrace\win64.rs:85
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    1: std::backtrace_rs::backtrace::trace_unsynchronized
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\..\..\backtrace\src\backtrace\mod.rs:66
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    2: std::backtrace::Backtrace::create
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\backtrace.rs:331
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    3: std::backtrace::Backtrace::capture
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\backtrace.rs:296
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    4: ruff_db::panic::install_hook::closure$0::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ruff_db\src\panic.rs:114
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    5: std::panicking::rust_panic_with_hook
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:841
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    6: std::panicking::begin_panic_handler::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:699
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    7: std::sys::backtrace::__rust_end_short_backtrace<std::panicking::begin_panic_handler::closure_env$0,never$>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\sys\backtrace.rs:174
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    8: std::panicking::begin_panic_handler
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    9: core::panicking::panic_fmt
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   10: core::panicking::panic
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:145
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   11: salsa::zalsa_local::QueryRevisions::set_iteration_count
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\zalsa_local.rs:731
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   12: salsa::function::impl$1::set_cycle_iteration_count<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function.rs:394
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   13: salsa::function::IngredientImpl<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_>::execute_maybe_iterate<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_w
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:438
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   14: salsa::function::IngredientImpl<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_>::execute<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configura
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:107
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   15: salsa::function::IngredientImpl<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_>::fetch_cold<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Config
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:179
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   16: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   17: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::impl$158:
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1647
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   18: salsa::function::IngredientImpl<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_>::refresh_memo
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   19: salsa::function::IngredientImpl<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configuration_>::fetch<ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::is_redundant_with__Configurati
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:31
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   20: ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:476
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   21: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,bool,ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:79
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   22: salsa::attach::attach::closure$0<bool,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:135
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   23: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<bool,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::closure_env$0>,bo
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   24: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<bool,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::closure_env$0>,bool>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   25: salsa::attach::attach<bool,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:133
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   26: ty_python_semantic::types::impl$158::is_redundant_with::is_redundant_with_
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:470
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   27: enum2$<ty_python_semantic::types::Type>::is_redundant_with
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types.rs:928
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   28: ty_python_semantic::types::builder::UnionBuilder::push_type
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\builder.rs:507
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   29: ty_python_semantic::types::builder::UnionBuilder::add_in_place_impl
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\builder.rs:453
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   30: ty_python_semantic::types::builder::UnionBuilder::add_in_place
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\builder.rs:256
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   31: ty_python_semantic::types::builder::UnionBuilder::add
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\builder.rs:250
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   32: ty_python_semantic::types::impl$65::from_elements_leave_aliases::closure$0<array$<enum2$<ty_python_semantic::types::Type>,2>,enum2$<ty_python_semantic::types::Type> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types.rs:11612
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   33: core::ops::try_trait::impl$0::wrap_mut_2::closure$0<ty_python_semantic::types::builder::UnionBuilder,ty_python_semantic::types::builder::UnionBuilder,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::impl$65::from_elements_leave_aliases::c
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\try_trait.rs:401
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   34: core::array::iter::iter_inner::impl$8::try_fold::closure$0<enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::builder::UnionBuilder,core::ops::try_trait::impl$0::wrap_mut_2::closure_env$0<ty_python_semantic::types::builder::UnionBuilder,ty_
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\array\iter\iter_inner.rs:220
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   35: core::ops::index_range::impl$1::try_fold<ty_python_semantic::types::builder::UnionBuilder,core::array::iter::iter_inner::impl$8::try_fold::closure_env$0<enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::builder::UnionBuilder,core::ops::try
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\index_range.rs:169
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   36: core::array::iter::impl$4::fold<enum2$<ty_python_semantic::types::Type>,2,ty_python_semantic::types::builder::UnionBuilder,ty_python_semantic::types::impl$65::from_elements_leave_aliases::closure_env$0<array$<enum2$<ty_python_semantic::types::Type>,2>,enum
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\array\iter.rs:253
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   37: ty_python_semantic::types::UnionType::from_elements_leave_aliases<array$<enum2$<ty_python_semantic::types::Type>,2>,enum2$<ty_python_semantic::types::Type> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types.rs:11610
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   38: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_type_expression_no_store
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder\type_expression.rs:128
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   39: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_annotation_expression_impl
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder\annotation_expression.rs:292
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   40: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_annotation_expression
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder\annotation_expression.rs:22
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   41: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_annotated_assignment_definition
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:4579
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   42: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_definition
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:1221
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   43: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:488
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   44: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_definition
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:10009
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   45: ty_python_semantic::types::infer::infer_definition_types::impl$2::execute::inner_
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer.rs:161
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   46: ty_python_semantic::types::infer::infer_definition_types::impl$2::execute
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:302
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   47: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>::execute_query<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:507
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   48: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:210
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   49: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>::execute<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:107
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   50: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:179
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   51: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   52: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_definition
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1647
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   53: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>::refresh_memo
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   54: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>::fetch<ty_python_semantic::types::infer::infer_definition_types::infer_definition_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:31
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   55: ty_python_semantic::types::infer::infer_definition_types::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:480
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   56: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::DefinitionInference>,ty_python_semantic::types::infer::infer_definition_types::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:79
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   57: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::DefinitionInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_definition_types::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:135
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   58: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::DefinitionInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infe
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   59: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::DefinitionInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_de
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   60: salsa::attach::attach<ref$<ty_python_semantic::types::infer::DefinitionInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_definition_types::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:133
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   61: ty_python_semantic::types::infer::infer_definition_types
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:470
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   62: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_definition<ref$<ruff_python_ast::generated::StmtAnnAssign> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:2135
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   63: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_annotated_assignment_statement
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:4520
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   64: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_statement
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:2109
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   65: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_body
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:2087
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   66: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_module
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:1803
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   67: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_scope
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:500
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   68: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:487
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   69: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_scope
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer\builder.rs:10077
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   70: ty_python_semantic::types::infer::infer_scope_types::impl$2::execute::inner_
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types\infer.rs:82
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   71: ty_python_semantic::types::infer::infer_scope_types::impl$2::execute
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:302
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   72: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute_query<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:507
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   73: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:210
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   74: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\execute.rs:107
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   75: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:179
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   76: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   77: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_scope_types::infer_s
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1647
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   78: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::refresh_memo
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:65
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   79: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::fetch<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\function\fetch.rs:31
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   80: ty_python_semantic::types::infer::infer_scope_types::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:480
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   81: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::ScopeInference>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:79
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   82: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:135
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   83: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_sco
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   84: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_t
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   85: salsa::attach::attach<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\attach.rs:133
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   86: ty_python_semantic::types::infer::infer_scope_types
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\components\salsa-macro-rules\src\setup_tracked_fn.rs:470
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   87: ty_python_semantic::types::check_types
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\src\types.rs:149
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   88: core::ops::function::FnOnce::call_once<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db> >,ruff_db::files::File),tuple$<ref$<dyn$<ty_python_semantic::db::Db> >,ruff_db::files::File> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   89: ty_test::attempt_test::closure$0<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global>,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db> >,ruff_db::files::File)>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:646
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   90: std::panicking::catch_unwind::do_call<ty_test::attempt_test::closure_env$0<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global>,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   91: std::panic::catch_unwind<ty_test::attempt_test::closure_env$0<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global>,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db> >,ruff_db::f
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   92: std::panicking::catch_unwind
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   93: std::panic::catch_unwind<ty_test::attempt_test::closure_env$0<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global>,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db> >,ruff_db::f
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   94: ruff_db::panic::catch_unwind<ty_test::attempt_test::closure_env$0<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global>,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db> >,ruff_d
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ruff_db\src\panic.rs:145
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   95: ty_test::attempt_test<alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global>,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> (*)(ref$<dyn$<ty_python_semantic::db::Db> >,ruff_db::files::File)>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:646
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   96: ty_test::run_test::closure$2
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:342
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   97: core::ops::function::impls::impl$3::call_mut<tuple$<ref$<ty_test::TestFile> >,ty_test::run_test::closure_env$2>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:301
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   98: core::slice::iter::impl$166::find_map<ty_test::TestFile,ty_test::FileFailures,ref_mut$<ty_test::run_test::closure_env$2> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\slice\iter\macros.rs:342
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990   99: core::iter::adapters::filter_map::impl$2::next<ty_test::FileFailures,core::slice::iter::Iter<ty_test::TestFile>,ty_test::run_test::closure_env$2>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\filter_map.rs:64
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  100: alloc::vec::spec_from_iter_nested::impl$0::from_iter<ty_test::FileFailures,core::iter::adapters::filter_map::FilterMap<core::slice::iter::Iter<ty_test::TestFile>,ty_test::run_test::closure_env$2> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\spec_from_iter_nested.rs:25
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  101: alloc::vec::spec_from_iter::impl$0::from_iter<ty_test::FileFailures,core::iter::adapters::filter_map::FilterMap<core::slice::iter::Iter<ty_test::TestFile>,ty_test::run_test::closure_env$2> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\spec_from_iter.rs:34
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  102: alloc::vec::impl$15::from_iter<ty_test::FileFailures,core::iter::adapters::filter_map::FilterMap<core::slice::iter::Iter<ty_test::TestFile>,ty_test::run_test::closure_env$2> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\mod.rs:3633
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  103: core::iter::traits::iterator::Iterator::collect<core::iter::adapters::filter_map::FilterMap<core::slice::iter::Iter<ty_test::TestFile>,ty_test::run_test::closure_env$2>,alloc::vec::Vec<ty_test::FileFailures,alloc::alloc::Global> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:2027
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  104: ty_test::run_test
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:398
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  105: ty_test::run
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:72
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  106: mdtest::mdtest
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\tests\mdtest.rs:29
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  107: mdtest::mdtest__protocols
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\tests\mdtest.rs:7
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  108: mdtest::mdtest__protocols::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at .\tests\mdtest.rs:10
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  109: core::ops::function::FnOnce::call_once<mdtest::mdtest__protocols::closure_env$0,tuple$<> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  110: core::ops::function::FnOnce::call_once
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  111: test::__rust_begin_short_backtrace<enum2$<core::result::Result<tuple$<>,alloc::string::String> >,enum2$<core::result::Result<tuple$<>,alloc::string::String> > (*)()>
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\test\src\lib.rs:648
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  112: test::run_test_in_process
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\test\src\lib.rs:671
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  113: test::run_test::closure$0
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\test\src\lib.rs:592
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  114: test::run_test::closure$1
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\test\src\lib.rs:622
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  115: std::sys::backtrace::__rust_begin_short_backtrace<test::run_test::closure_env$1,tuple$<> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\sys\backtrace.rs:158
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  116: core::ops::function::FnOnce::call_once<std::thread::impl$0::spawn_unchecked_::closure_env$1<test::run_test::closure_env$1,tuple$<> >,tuple$<> >
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  117: alloc::boxed::impl$28::call_once
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\alloc\src\boxed.rs:1971
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  118: alloc::boxed::impl$28::call_once
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\alloc\src\boxed.rs:1971
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  119: std::sys::pal::windows::thread::impl$0::new::thread_start
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\sys\pal\windows\thread.rs:60
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  120: BaseThreadInitThunk
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990  121: RtlUserThreadStart
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 query stacktrace:
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    0: infer_definition_types(Id(92a6g3)) -> (R314, Durability::LOW)
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at crates\ty_python_semantic\src\types\infer.rs:144
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990    1: infer_scope_types(Id(c00)) -> (R314, Durability::LOW)
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990              at crates\ty_python_semantic\src\types\infer.rs:71
  crates\ty_python_semantic\resources\mdtest\protocols.md:2990 

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='protocols.md - Protocols - Recursive protocols - Recursive generic pr… (e02160e5ca187a06)'
MDTEST_TEST_FILTER='protocols.md - Protocols - Recursive protocols - Recursive generic pr… (e02160e5ca187a06)' cargo test -p ty_python_semantic --test mdtest -- mdtest__protocols

--------------------------------------------------

test mdtest__protocols ... FAILED

failures:

failures:
    mdtest__protocols

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 275 filtered out; finished in 8.44s


--- STDERR:              ty_python_semantic::mdtest mdtest__protocols ---

thread 'mdtest__protocols' panicked at crates\ty_test\src\lib.rs:138:5:
Some tests failed.
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\std\src\panicking.rs:697
   1: core::panicking::panic_fmt
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\panicking.rs:75
   2: ty_test::run
             at C:\Users\sbym8\GitHub\ruff\crates\ty_test\src\lib.rs:138
   3: mdtest::mdtest
             at .\tests\mdtest.rs:29
   4: mdtest::mdtest__protocols
             at .\tests\mdtest.rs:7
   5: mdtest::mdtest__protocols::closure$0
             at .\tests\mdtest.rs:10
   6: core::ops::function::FnOnce::call_once<mdtest::mdtest__protocols::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.90-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:253
   7: core::ops::function::FnOnce::call_once
             at /rustc/1159e78c4747b02ef996e55082b704c09b970588/library\core\src\ops\function.rs:253
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

------------
     Summary [  25.902s] 548 tests run: 542 passed (5 leaky), 6 failed, 37 skipped
        FAIL [   1.877s] ty_python_semantic types::tests::type_alias_variance
        FAIL [   8.240s] ty_python_semantic::mdtest mdtest__attributes
        FAIL [   4.152s] ty_python_semantic::mdtest mdtest__bidirectional
        FAIL [  11.899s] ty_python_semantic::mdtest mdtest__function_return_type
        FAIL [   3.366s] ty_python_semantic::mdtest mdtest__generics_legacy_classes
        FAIL [   8.557s] ty_python_semantic::mdtest mdtest__protocols
error: test run failed
</details>

---

_Comment by @MichaReiser on 2025-10-27 09:04_

> thread 'types::tests::type_alias_variance' panicked at C:\Users\sbym8.cargo\git\checkouts\salsa-c5388728a3f80381\9ea5289\src\cycle.rs:355:17:
assertion left == right failed: Can't merge cycle heads infer_scope_types(Id(818)) with different iteration counts (IterationCount(0), IterationCount(1))
left: IterationCount(0)
right: IterationCount(1)

 Oh no. This could be a salsa bug. I'd have to take a look. I fixed a similar case last week in https://github.com/salsa-rs/salsa/pull/1014 

---

_Comment by @MichaReiser on 2025-10-27 17:29_

I'll take a look at those salsa issues but I can't say yet how easy/quick I'll be at fixing them. 

I suspect what's happening (or at least, what we see more of) is that entire nested cycles "disappear" in later iterations because we converge (e.g. a `infer_implicit_attribute` on `Unknown` won't create any cycles anymore where it might have in previous iterationo where the value was a class literal)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:98 on 2025-10-28 09:07_

It seems like it would be beneficial if salsa passed the owned current value instead of a reference. I'd have to think about how such an API exactly should look like because we don't want to call `::eq` twice when returning `Iterate` but we also need to ensure that the value hasn't been changed when using `::Iterate`. 

Unless we can find a way to defer cloning in ty unless we hit a divergent value. But i suspect that this is might be harder

---

_@MichaReiser reviewed on 2025-10-28 09:07_

---

_Comment by @MichaReiser on 2025-10-28 11:10_

Okay, I think I found the issue. There are now instances where the cycle recovery function for `infer_definition_types` introduces new cycles... I'd have to do some more debugging to understand why and if it is necessary. I worry that allowing this could be tricky (and I suspect it's often undesired too). 

```
Cycle recovery function for infer_definition_types(Id(a42f)) introduced a cycle, depending on is_equivalent_to_object_inner(Id(8007)). This is not allowed.
```

I'll put up a Salsa PR that catches with an assertion. Let me know if we need to change Salsa's behavior here (and what the use case is)

---

_Comment by @MichaReiser on 2025-10-29 19:49_

Talked to @carljm today. He mentioned that there's a good chance that the new cycle dependencies come from unioning the types in the "flip-flop-cases", because unioning tests if two types are equal, which compares types... 

It wasn't clear to us if it were possible to use a more simplified unioning, assuming that we only want to union certain types (e.g. literals)

The relevant Salsa PR with the fix https://github.com/salsa-rs/salsa/pull/1018

---

_Comment by @mtshiba on 2025-10-31 06:17_

> Talked to @carljm today. He mentioned that there's a good chance that the new cycle dependencies come from unioning the types in the "flip-flop-cases", because unioning tests if two types are equal, which compares types...
> 
> It wasn't clear to us if it were possible to use a more simplified unioning, assuming that we only want to union certain types (e.g. literals)
> 
> The relevant Salsa PR with the fix [salsa-rs/salsa#1018](https://github.com/salsa-rs/salsa/pull/1018)

It seems that there is no problem if we use a simplified union handling that does not call `is_redundant_with`, as long as it is done within `cycle_recovery` (https://github.com/astral-sh/ruff/pull/20566/commits/9115e40ddc854c240c283b9b3512e5e0917618c4).

---

_Comment by @mtshiba on 2025-10-31 07:01_

The remaining problem is that if query cycles are nested, the `Divergent` type will remain even after convergence.
Here is the example:

`mdtest/attributes.py` (simplified)

```py
class ManyCycles:
    def __init__(self: "ManyCycles"):
        self.x1 = 0
        self.x2 = 0
        self.x3 = 1

    def f1(self: "ManyCycles"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2

    def f2(self: "ManyCycles"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2

    def f3(self: "ManyCycles"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2

        reveal_type(self.x1)  # revealed: Unknown | int
        reveal_type(self.x2)  # revealed: Unknown | int
```

But the revealed types are:

```sh
info[revealed-type]: Revealed type
  --> test.py:22:21
   |
20 |         self.x3 = self.x1 + self.x2
21 |
22 |         reveal_type(self.x1)  # revealed: Unknown | int
   |                     ^^^^^^^ `Unknown | DivergentType(id=Id(4805)) | DivergentType(id=Id(1406)) | int`
23 |         reveal_type(self.x2)  # revealed: Unknown | int
   |

info[revealed-type]: Revealed type
  --> test.py:23:21
   |
22 |         reveal_type(self.x1)  # revealed: Unknown | int
23 |         reveal_type(self.x2)  # revealed: Unknown | int
   |                     ^^^^^^^ `Unknown | DivergentType(id=Id(4805)) | DivergentType(id=Id(1406)) | int`
   |
```

I found that these queries are in a nested cycle and are being delegated outwards.

```
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle ClassLiteral < 'db >::implicit_attribute_inner_(Id(8003))
...
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(4805)), iterate it as part of the outer cycle ClassLiteral < 'db >::implicit_attribute_inner_(Id(8003))
...
```

In the current implementation, `Divergent` in the union type is assumed to be removed in the `cycle_recovery` function so that it is not exposed in convergent inference, but this handling is skipped when the cycle head moves.

I think that a fix for this requires changes to salsa. Here are some solutions I came up with:
* Provide a way to notify the `cycle_recovery` function that the cycle head has moved (e.g., by passing the IDs of the old inner cycle heads)
* Specify a "finalize" function that will always be called after the cycle has converged

---

_Comment by @MichaReiser on 2025-10-31 12:33_

This is great work! 

> In the current implementation, Divergent in the union type is assumed to be removed in the cycle_recovery function so that it is not exposed in convergent inference, but this handling is skipped when the cycle head moves.

I somewhat suspect that this would be the case, and it shouldn't be hard to support this, other than coming up with a good API.

> Provide a way to notify the cycle_recovery function that the cycle head has moved (e.g., by passing the IDs of the old inner cycle heads)

This seems too low-level to me.


> Specify a "finalize" function that will always be called after the cycle has converged

I think a `finalize` method would be misleading because an inner cycle can't know if it has finalized. All it knows is that it has converged, but any other head participating in the same cycle might not have converged and forces another iteration.

I think I prefer to simply call the cycle function on every iteration, regardless if the query has converged or not. The only downside of this is that it may require comparing the value twice:

* Once in the cycle function to decide whether some fallback logic should apply (maybe not in our case but I could see other instances where the fallback logic is expensive). This also feels a bit like a footgun for users that aren't aware that the function is called on every iteration
* Once within `execute_maybe_iterate` to decide if the query has converged.

Either way, this seems very solvable. Would calling the cycle function on every iteration work for your case?

---

_Comment by @mtshiba on 2025-10-31 15:07_

> I think I prefer to simply call the cycle function on every iteration, regardless if the query has converged or not. The only downside of this is that it may require comparing the value twice:
> 
> * Once in the cycle function to decide whether some fallback logic should apply (maybe not in our case but I could see other instances where the fallback logic is expensive). This also feels a bit like a footgun for users that aren't aware that the function is called on every iteration
> * Once within `execute_maybe_iterate` to decide if the query has converged.
> 
> Either way, this seems very solvable. Would calling the cycle function on every iteration work for your case?

Yes, I think that would work.

---

_Comment by @MichaReiser on 2025-10-31 21:52_

This should do it https://github.com/salsa-rs/salsa/pull/1021

---

_Comment by @MichaReiser on 2025-11-01 11:43_

Is there a chance that finalizing a nested query before the outer query converges results in a diverging query? Because there's no guarantee that a query that converged in an earlier iteration won't change its value in a later iteration if another head participating in the same cycle changed (on which the query also depends on directly or transitively)

---

_Comment by @mtshiba on 2025-11-01 16:01_

> Is there a chance that finalizing a nested query before the outer query converges results in a diverging query? Because there's no guarantee that a query that converged in an earlier iteration won't change its value in a later iteration if another head participating in the same cycle changed (on which the query also depends on directly or transitively)

The case in which divergent type inference would actually lead to a too-many-cycles panic would be if `Divergent` were lost from the provisional value, making it impossible to track type divergence. However, my concern so far has been that `Divergent` is left behind (the cycle recovery function removes `Divergent` from the provisional value when it's not actually needed, so if execution of this function is skipped, `Divergent` will remain), and there are probably no scenarios where `Divergent` would accidentally disappear due to salsa (as long as the cycle initial function is always executed).
On the other hand, in oscillating type inference, monotonicity may be broken by skipping the execution of the cycle recovery function.

---

_Comment by @MichaReiser on 2025-11-01 23:02_

> The case in which divergent type inference would actually lead to a too-many-cycles panic would be if Divergent were lost from the provisional value, making it impossible to track type divergence

I guess that's exactly the part I'm worried about. If a nested cycle converges (the value and previous value are the same), then the recovery function would remove the divergent value **before** the outer cycle converges. 

---

_Comment by @mtshiba on 2025-11-02 04:03_

> > The case in which divergent type inference would actually lead to a too-many-cycles panic would be if Divergent were lost from the provisional value, making it impossible to track type divergence
> 
> I guess that's exactly the part I'm worried about. If a nested cycle converges (the value and previous value are the same), then the recovery function would remove the divergent value **before** the outer cycle converges.

The recovery function only removes `Divergent` types that originate from its own query, so it will not accidentally remove `Divergent` types that originate from different cycle heads.

Also, the recovery function implementation does not include any part that determines whether values ​​converge.
The recovery function simply performs the following steps each time it is called:

* Take the union of the types in the current and last provisional values
* If a union type like `Divergent | T` exists among the provisional values, simplify it to `T`
* If a nested type containing a `Divergent` type like `Unknown | tuple[Unknown | tuple[Divergent, Literal[1]], Literal[1]]` exists among the provisional values, simplify it to `Unknown | tuple[Divergent, Literal[1]]`

---

_Comment by @github-actions[bot] on 2025-11-03 15:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/__init__.pyi:136:11: error[invalid-type-form] Invalid subscript of object of type `Unknown | None` in type expression
+ src/attr/__init__.pyi:136:11: error[invalid-type-form] Invalid subscript of object of type `None` in type expression

parso (https://github.com/davidhalter/parso)
- parso/python/errors.py:415:18: warning[possibly-missing-attribute] Attribute `add_block` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:416:24: warning[possibly-missing-attribute] Attribute `blocks` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:431:28: warning[possibly-missing-attribute] Attribute `parent_context` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:432:13: warning[possibly-missing-attribute] Attribute `close_child_context` may be missing on object of type `Unknown | None`
- parso/python/errors.py:470:32: warning[possibly-missing-attribute] Attribute `add_context` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:489:9: warning[possibly-missing-attribute] Attribute `finalize` may be missing on object of type `Unknown | None | _Context`
- Found 203 diagnostics
+ Found 197 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/mirrors/mirror.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Literal[True] | Unknown | str`
+ lib/spack/spack/mirrors/mirror.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Literal[True] | Unknown | str | Divergent`
- lib/spack/spack/mirrors/mirror.py:120:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | str`
+ lib/spack/spack/mirrors/mirror.py:120:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | str | Divergent`
- lib/spack/spack/mirrors/mirror.py:263:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | str`
+ lib/spack/spack/mirrors/mirror.py:263:45: error[invalid-argument-type] Argument to bound method `_update_connection_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | str | Divergent`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:216:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:233:39: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:268:39: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:2124:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:2188:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:2229:42: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:2257:31: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/incremental_graph.py:82:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/incremental_graph.py:89:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/types.py:240:13: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:240:13: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:243:14: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:243:14: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:244:17: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:244:17: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:343:12: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:343:12: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:349:14: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:349:14: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:350:18: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:350:18: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:351:16: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:351:16: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:353:17: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:353:17: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:454:11: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:454:11: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:455:13: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:455:13: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:459:17: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:459:17: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:576:13: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:576:13: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:579:14: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:579:14: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:580:17: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:580:17: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:701:13: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:701:13: error[invalid-type-form] Type qualifier `typing.NotRequired` is not allowed in type expressions (only in annotation expressions)
- src/graphql/execution/types.py:706:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/types.py:763:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/types.py:778:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/types.py:910:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/graphql.py:41:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/graphql.py:111:42: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/graphql.py:139:31: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/graphql.py:177:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:43:39: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:48:50: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:53:38: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:58:50: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:63:34: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:68:40: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:79:33: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:84:51: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:89:44: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:96:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/predicates.py:101:43: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/language/source.py:79:31: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/pyutils/async_reduce.py:27:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/pyutils/is_awaitable.py:20:33: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/pyutils/is_iterable.py:28:34: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/pyutils/is_iterable.py:35:32: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:183:28: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:213:37: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:455:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:584:39: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:610:39: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:814:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:918:38: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1003:34: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1197:33: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1375:41: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1490:33: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1579:34: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1594:35: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1616:37: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1629:37: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1689:34: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1725:33: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1745:38: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/definition.py:1765:37: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/directives.py:142:37: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/scalars.py:321:58: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/type/schema.py:442:31: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/validation/rules/known_type_names.py:92:6: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str] & dict[str, Any]`
+ tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & dict[Unknown | str, Unknown | str]`
- tests/type/test_definition.py:1291:39: error[invalid-type-form] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- Found 528 diagnostics
+ Found 471 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 505 diagnostics
+ Found 507 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job.py:167:24: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None`
+ dragonchain/job_processor/contract_job.py:167:24: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
- dragonchain/job_processor/contract_job.py:198:60: error[not-iterable] Object of type `Unknown | None` may not be iterable
+ dragonchain/job_processor/contract_job.py:198:60: error[not-iterable] Object of type `Unknown | None | (Divergent & ~AlwaysFalsy)` may not be iterable
- dragonchain/job_processor/contract_job.py:250:29: error[invalid-argument-type] Argument to bound method `pull_image` is incorrect: Expected `str`, found `Unknown | None`
+ dragonchain/job_processor/contract_job.py:250:29: error[invalid-argument-type] Argument to bound method `pull_image` is incorrect: Expected `str`, found `Unknown | None | (Divergent & ~AlwaysFalsy)`
- dragonchain/job_processor/contract_job.py:327:23: error[not-iterable] Object of type `Unknown | None` may not be iterable
+ dragonchain/job_processor/contract_job.py:327:23: error[not-iterable] Object of type `Unknown | None | (Divergent & ~AlwaysFalsy)` may not be iterable
- dragonchain/job_processor/contract_job.py:419:13: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None`
+ dragonchain/job_processor/contract_job.py:419:13: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
- dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None`
+ dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/asyncio/base_events.pyi:66:42: error[invalid-type-form] Invalid subscript of object of type `<class 'Awaitable[typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/base_events.pyi:87:37: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/base_events.pyi:89:37: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/events.pyi:73:58: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/events.pyi:133:42: error[invalid-type-form] Invalid subscript of object of type `<class 'Awaitable[typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/events.pyi:167:25: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/events.pyi:171:37: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/taskgroups.pyi:25:33: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/tasks.pyi:417:27: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- mypy/typeshed/stdlib/asyncio/tasks.pyi:420:27: error[invalid-type-form] Invalid subscript of object of type `<class 'Coroutine[Any, Any, typing.TypeVar]'> | types.UnionType` in type expression
- Found 1780 diagnostics
+ Found 1770 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/serialization/json_schema.py:334:17: error[invalid-return-type] Return type does not match returned value: expected `dict[str, None | int | str | ... omitted 4 union elements]`, found `dict[str, object]`
+ koda_validate/serialization/json_schema.py:340:17: error[invalid-return-type] Return type does not match returned value: expected `dict[str, None | int | str | ... omitted 4 union elements]`, found `dict[str, object]`
+ koda_validate/serialization/json_schema.py:356:17: error[invalid-return-type] Return type does not match returned value: expected `dict[str, None | int | str | ... omitted 4 union elements]`, found `dict[str, object]`
+ koda_validate/serialization/json_schema.py:362:17: error[invalid-return-type] Return type does not match returned value: expected `dict[str, None | int | str | ... omitted 4 union elements]`, found `dict[str, object]`
- Found 84 diagnostics
+ Found 88 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/ast/interpreter.py:197:9: error[no-matching-overload] No overload of bound method `update` matches arguments
+ mesonbuild/ast/interpreter.py:299:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`, found `BaseNode & Top[list[Unknown]]`
+ mesonbuild/ast/interpreter.py:301:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`, found `BaseNode & Top[dict[Unknown, Unknown]]`
- mesonbuild/ast/interpreter.py:556:48: error[invalid-argument-type] Argument to bound method `node_to_runtime_value` is incorrect: Expected `MesonInterpreterObject | BaseNode | str | ... omitted 4 union elements`, found `object`
+ mesonbuild/ast/interpreter.py:556:48: error[invalid-argument-type] Argument to bound method `node_to_runtime_value` is incorrect: Expected `MesonInterpreterObject | BaseNode | Sequence[Divergent] | ... omitted 6 union elements`, found `object`
- mesonbuild/ast/interpreter.py:694:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Token[str]`, found `Token[str | int]`
+ mesonbuild/ast/interpreter.py:731:28: error[invalid-argument-type] Argument to function `src_to_abs` is incorrect: Expected `str | IntrospectionFile | UnknownValue`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/ast/introspection.py:70:9: error[no-matching-overload] No overload of bound method `update` matches arguments
+ mesonbuild/interpreter/interpreter.py:1465:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:2225:19: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `EnvironmentVariables | dict[str, Divergent] | list[Divergent] | str`
+ mesonbuild/interpreter/interpreter.py:2225:19: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `EnvironmentVariables | dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | str`
+ mesonbuild/interpreter/interpreter.py:2231:33: error[invalid-argument-type] Argument is incorrect: Expected `EnvironmentVariables | list[Unknown] | dict[Unknown, Unknown] | str | None`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:2253:37: error[invalid-argument-type] Argument to bound method `unpack_env_kwarg` is incorrect: Expected `EnvironmentVariables | dict[str, Divergent] | list[Divergent] | str`, found `BaseTest`
+ mesonbuild/interpreter/interpreter.py:2253:37: error[invalid-argument-type] Argument to bound method `unpack_env_kwarg` is incorrect: Expected `EnvironmentVariables | dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | str`, found `BaseTest`
- mesonbuild/interpreter/interpreter.py:2748:41: error[invalid-argument-type] Argument to bound method `run_command_impl` is incorrect: Expected `tuple[Executable | ExternalProgram | Compiler | File | str, list[Executable | ExternalProgram | Compiler | File | str]]`, found `tuple[str | ExternalProgram, list[str | ExternalProgram]]`
+ mesonbuild/interpreter/interpreter.py:2748:41: error[invalid-argument-type] Argument to bound method `run_command_impl` is incorrect: Expected `tuple[Executable | ExternalProgram | Compiler | File | str, list[Executable | ExternalProgram | Compiler | File | str]]`, found `tuple[str | ExternalProgram, list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]]`
+ mesonbuild/interpreter/interpreter.py:3045:46: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `EnvironmentVariables | list[str] | list[list[str]] | ... omitted 3 union elements`, found `str | list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3045:52: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `Literal["set", "prepend", "append"]`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreter/interpreter.py:3045:70: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `str`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:3597:36: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreter/interpreter.py:3597:36: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreter/interpreter.py:3610:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `InterpreterObject`
+ mesonbuild/interpreter/interpreter.py:3610:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
- mesonbuild/interpreter/interpreter.py:3613:24: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `InterpreterObject`
+ mesonbuild/interpreter/interpreter.py:3613:24: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
- mesonbuild/interpreter/interpreter.py:3613:40: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `~None`
+ mesonbuild/interpreter/interpreter.py:3613:40: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `~None`
+ mesonbuild/interpreter/primitives/array.py:57:44: error[invalid-argument-type] Argument to function `check_contains` is incorrect: Expected `list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`, found `(Sequence[Divergent] & Top[list[Unknown]]) | (Sequence[Divergent] & Top[list[Unknown]]) | (HoldableObject & Top[list[Unknown]]) | (MesonInterpreterObject & Top[list[Unknown]])`
- mesonbuild/interpreter/primitives/array.py:109:40: error[invalid-argument-type] Argument to function `flatten` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreter/primitives/array.py:109:40: error[invalid-argument-type] Argument to function `flatten` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/_unholder.py:18:16: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `HoldableObject | int | str | Top[list[Unknown]] | Top[dict[Unknown, Unknown]]`
+ mesonbuild/interpreterbase/_unholder.py:18:16: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `HoldableObject | int | str | Top[list[Unknown]] | Top[dict[Unknown, Unknown]]`
+ mesonbuild/interpreterbase/baseobjects.py:70:17: error[no-matching-overload] No overload of bound method `update` matches arguments
+ mesonbuild/interpreterbase/baseobjects.py:137:26: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `Self@operator_call`
+ mesonbuild/interpreterbase/baseobjects.py:137:32: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreterbase/baseobjects.py:139:45: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `Self@operator_call`
+ mesonbuild/interpreterbase/baseobjects.py:139:51: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements], dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements], SubProject]`, found `tuple[Any, None | Any, None | Any, Any]`
+ mesonbuild/interpreterbase/helpers.py:31:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `~Top[list[Unknown]] & ~StringNode`
- mesonbuild/interpreterbase/helpers.py:37:30: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:37:30: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:39:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `dict[object, Divergent]`
+ mesonbuild/interpreterbase/helpers.py:39:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `dict[object, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 6 union elements]`
- mesonbuild/interpreterbase/helpers.py:39:33: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:39:33: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:58:59: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:58:59: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:60:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:60:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:61:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:61:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/interpreterbase.py:56:63: error[invalid-argument-type] Argument to class `ObjectHolder` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `typing.TypeVar`
+ mesonbuild/interpreterbase/interpreterbase.py:56:63: error[invalid-argument-type] Argument to class `ObjectHolder` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `typing.TypeVar`
- mesonbuild/interpreterbase/interpreterbase.py:294:67: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:294:67: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:309:60: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:309:60: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:372:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:372:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:380:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:380:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:388:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:388:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:396:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:396:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:405:70: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:405:70: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:436:64: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:436:64: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:569:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `HoldableObject | int | str | ... omitted 5 union elements`
+ mesonbuild/interpreterbase/interpreterbase.py:569:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `HoldableObject | int | str | ... omitted 8 union elements`
- mesonbuild/interpreterbase/interpreterbase.py:573:32: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `HoldableObject | int | str | ... omitted 5 union elements`
+ mesonbuild/interpreterbase/interpreterbase.py:573:32: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `HoldableObject | int | str | ... omitted 8 union elements`
- mesonbuild/interpreterbase/interpreterbase.py:631:41: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/interpreterbase/interpreterbase.py:631:41: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/interpreterbase.py:648:37: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `(InterpreterObject & ~MutableInterpreterObject) | None | (InterpreterObject & MutableInterpreterObject)`
+ mesonbuild/interpreterbase/interpreterbase.py:648:37: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `(InterpreterObject & ~MutableInterpreterObject) | None | (InterpreterObject & MutableInterpreterObject)`
+ mesonbuild/modules/cmake.py:192:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- mesonbuild/modules/hotdoc.py:135:48: error[invalid-argument-type] Argument to bound method `check_extra_arg_type` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `object`
+ mesonbuild/modules/hotdoc.py:135:48: error[invalid-argument-type] Argument to bound method `check_extra_arg_type` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/modules/modtest.py:22:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- mesonbuild/optinterpreter.py:131:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 3 union elements`, found `int | float`
+ mesonbuild/optinterpreter.py:131:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `int | float`
+ mesonbuild/rewriter.py:774:83: error[unresolved-attribute] Object of type `(Sequence[Divergent] & ~UnknownValue & ~str) | (int & ~UnknownValue) | (dict[str, Divergent] & ~UnknownValue) | ... omitted 4 union elements` has no attribute `to_abs_path`
- Found 1900 diagnostics
+ Found 1920 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/channels/handlers/ws_handler.py:69:5: warning[unsupported-base] Unsupported class base with type `<class 'AsyncBaseHTTPView[GraphQLWSConsumer, GraphQLWSConsumer, GraphQLWSConsumer, GraphQLWSConsumer, GraphQLWSConsumer, Context@GraphQLWSConsumer, RootValue@GraphQLWSConsumer]'> | <class 'AsyncBaseHTTPView[GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], GraphQLWSConsumer[None, None], Context@GraphQLWSConsumer, RootValue@GraphQLWSConsumer]'>`
- Found 399 diagnostics
+ Found 400 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:1237:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:402:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:406:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:929:5: error[type-assertion-failure] Type `StdBuilds[Divergent]` does not match asserted type `StdBuilds[Unknown]`
+ tests/annotations/declarations.py:929:5: error[type-assertion-failure] Type `StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `StdBuilds[Unknown]`
- tests/annotations/declarations.py:934:5: error[type-assertion-failure] Type `StdBuilds[Divergent]` does not match asserted type `StdBuilds[Unknown]`
+ tests/annotations/declarations.py:934:5: error[type-assertion-failure] Type `StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `StdBuilds[Unknown]`
- tests/annotations/declarations.py:940:5: error[type-assertion-failure] Type `PBuilds[Divergent]` does not match asserted type `PBuilds[Unknown]`
+ tests/annotations/declarations.py:940:5: error[type-assertion-failure] Type `PBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `PBuilds[Unknown]`
- tests/annotations/declarations.py:944:5: error[type-assertion-failure] Type `PBuilds[Divergent]` does not match asserted type `PBuilds[Unknown]`
+ tests/annotations/declarations.py:944:5: error[type-assertion-failure] Type `PBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `PBuilds[Unknown]`
- tests/annotations/declarations.py:945:5: error[type-assertion-failure] Type `PBuilds[Divergent]` does not match asserted type `Unknown`
+ tests/annotations/declarations.py:945:5: error[type-assertion-failure] Type `PBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `Unknown`
- tests/annotations/declarations.py:951:5: error[type-assertion-failure] Type `FullBuilds[Divergent] | StdBuilds[Divergent]` does not match asserted type `FullBuilds[Unknown] | StdBuilds[Unknown]`
+ tests/annotations/declarations.py:951:5: error[type-assertion-failure] Type `FullBuilds[int | None | float | ... omitted 20 union elements] | StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `FullBuilds[Unknown] | StdBuilds[Unknown]`
- tests/annotations/declarations.py:956:5: error[type-assertion-failure] Type `PBuilds[Divergent] | StdBuilds[Divergent]` does not match asserted type `PBuilds[Unknown] | StdBuilds[Unknown]`
+ tests/annotations/declarations.py:956:5: error[type-assertion-failure] Type `PBuilds[int | None | float | ... omitted 20 union elements] | StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `PBuilds[Unknown] | StdBuilds[Unknown]`
- tests/annotations/declarations.py:961:5: error[type-assertion-failure] Type `PBuilds[Divergent] | StdBuilds[Divergent]` does not match asserted type `PBuilds[Unknown] | StdBuilds[Unknown]`
+ tests/annotations/declarations.py:961:5: error[type-assertion-failure] Type `PBuilds[int | None | float | ... omitted 20 union elements] | StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `PBuilds[Unknown] | StdBuilds[Unknown]`
- tests/annotations/declarations.py:969:5: error[type-assertion-failure] Type `FullBuilds[Divergent] | PBuilds[Divergent] | StdBuilds[Divergent]` does not match asserted type `FullBuilds[Unknown] | PBuilds[Unknown] | StdBuilds[Unknown]`
- tests/annotations/declarations.py:980:5: error[type-assertion-failure] Type `FullBuilds[Divergent] | PBuilds[Divergent] | StdBuilds[Divergent]` does not match asserted type `FullBuilds[Unknown] | PBuilds[Unknown] | StdBuilds[Unknown]`
+ tests/annotations/declarations.py:969:5: error[type-assertion-failure] Type `FullBuilds[int | None | float | ... omitted 20 union elements] | PBuilds[int | None | float | ... omitted 20 union elements] | StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `FullBuilds[Unknown] | PBuilds[Unknown] | StdBuilds[Unknown]`
+ tests/annotations/declarations.py:980:5: error[type-assertion-failure] Type `FullBuilds[int | None | float | ... omitted 20 union elements] | PBuilds[int | None | float | ... omitted 20 union elements] | StdBuilds[int | None | float | ... omitted 20 union elements]` does not match asserted type `FullBuilds[Unknown] | PBuilds[Unknown] | StdBuilds[Unknown]`
- tests/annotations/declarations.py:1430:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1456:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 550 diagnostics
+ Found 545 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/builder.py:239:55: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["name"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ cwltool/builder.py:241:58: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["name"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
+ cwltool/builder.py:246:21: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["type"]` and value of type `object` on object of type `MutableMapping[str, None | int | str | ... omitted 3 union elements]`
+ cwltool/builder.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
+ cwltool/builder.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ cwltool/builder.py:277:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
+ cwltool/builder.py:280:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
+ cwltool/builder.py:280:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ cwltool/builder.py:280:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `float`
+ cwltool/builder.py:283:21: error[invalid-argument-type] Argument to bound method `bind_input` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `(int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (MutableMapping[str, Divergent] & ~Top[MutableSequence[Unknown]])`
+ cwltool/builder.py:292:25: error[invalid-argument-type] Argument to bound method `bind_input` is incorrect: 

... (truncated 128 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo metadata = ~8MB
+     memo metadata = ~7MB


```

</details>




---

_Comment by @mtshiba on 2025-11-05 17:16_

Hmm, it looks like the problem is still not solved...

MRE:

```python
class ManyCycles:
    def __init__(self: "ManyCycles"):
        self.x1 = 0
        self.x2 = 0
        self.x3 = 1

    def f(self: "ManyCycles"):
        self.x1 = self.x2 + self.x3 + 1
        self.x2 = self.x1 + self.x3 + 2
        self.x3 = self.x1 + self.x2 + 3

m = ManyCycles()
reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
```

<details>
<summary>tracing output</summary>

```
INFO Defaulting to python-platform `win32`
INFO Python version: Python 3.13, platform: win32
INFO new_revision: R1 -> R2
INFO Indexed 1 file(s) in 0.007s
INFO check_file_impl(Id(c00)): executing query
INFO source_text(Id(c00)): executing query
INFO parsed_module(Id(c00)): executing query
INFO semantic_index(Id(c00)): executing query
INFO infer_scope_types(Id(1000)): executing query
INFO infer_definition_types(Id(140a)): executing query
INFO infer_definition_types(Id(140b)): executing query
INFO module_type_symbols(Id(2000)): executing query
INFO resolve_module_query(Id(2400)): executing query
INFO global_scope(Id(c01)): executing query
INFO semantic_index(Id(c01)): executing query
INFO parsed_module(Id(c01)): executing query
INFO source_text(Id(c01)): executing query
INFO place_table(Id(1004)): executing query
INFO place_by_id(Id(3000)): executing query
INFO use_def_map(Id(1004)): executing query
INFO infer_definition_types(Id(156f)): executing query
INFO infer_definition_types(Id(1429)): executing query
INFO resolve_module_query(Id(2401)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3800)): executing query
INFO imported_relative_submodules_of_stub_package(Id(2c01)): executing query
INFO global_scope(Id(c02)): executing query
INFO semantic_index(Id(c02)): executing query
INFO parsed_module(Id(c02)): executing query
INFO source_text(Id(c02)): executing query
INFO place_table(Id(10ee)): executing query
INFO place_by_id(Id(3001)): executing query
INFO use_def_map(Id(10ee)): executing query
INFO infer_definition_types(Id(1767)): executing query
INFO file_to_module(Id(c02)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4400)): executing query
INFO file_to_module(Id(c01)): executing query
INFO place_table(Id(104d)): executing query
INFO module_type_symbols(Id(2000)): execute: iterate again (IterationCount(1))...
INFO place_by_id(Id(3000)): executing query
INFO infer_definition_types(Id(156f)): executing query
INFO infer_definition_types(Id(1429)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c00)): executing query
INFO resolve_module_query(Id(2402)): executing query
INFO global_scope(Id(c0e)): executing query
INFO semantic_index(Id(c0e)): executing query
INFO parsed_module(Id(c0e)): executing query
INFO source_text(Id(c0e)): executing query
INFO place_table(Id(117e)): executing query
INFO place_by_id(Id(3002)): executing query
INFO use_def_map(Id(117e)): executing query
INFO infer_definition_types(Id(3df1)): executing query
INFO file_to_module(Id(c0e)): executing query
INFO infer_definition_types(Id(3dba)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c02)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c02)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c02)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c00)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c00)): executing query
INFO ClassLiteral < 'db >::try_mro_(Id(4c00)): executing query
INFO resolve_module_query(Id(2403)): executing query
INFO global_scope(Id(c0f)): executing query
INFO semantic_index(Id(c0f)): executing query
INFO parsed_module(Id(c0f)): executing query
INFO source_text(Id(c0f)): executing query
INFO place_table(Id(11ea)): executing query
INFO place_by_id(Id(3003)): executing query
INFO use_def_map(Id(11ea)): executing query
INFO infer_definition_types(Id(3f3c)): executing query
INFO infer_definition_types(Id(3eef)): executing query
INFO file_to_module(Id(c0f)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c04)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c04)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c04)): executing query
INFO lookup_dunder_new_inner(Id(6000)): executing query
INFO place_by_id(Id(3004)): executing query
INFO infer_definition_types(Id(3fb5)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c05)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c05)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c05)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c05)): executing query
INFO ClassLiteral < 'db >::try_metaclass_(Id(1c00)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c00)): executing query
INFO code_generator_of_class(Id(6400)): executing query
INFO place_table(Id(1001)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6800)): executing query
INFO use_def_map(Id(1001)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3801)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c00)): executing query
INFO ClassLiteral < 'db >::decorators_(Id(1c00)): executing query
INFO enum_metadata(Id(1c00)): executing query
INFO ClassLiteral < 'db >::decorators_(Id(1c02)): executing query
INFO enum_metadata(Id(1c02)): executing query
INFO place_by_id(Id(3005)): executing query
INFO static_expression_truthiness(Id(1898)): executing query
INFO infer_expression_types_impl(Id(1898)): executing query
INFO infer_definition_types(Id(3d36)): executing query
INFO resolve_module_query(Id(2404)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3802)): executing query
INFO imported_relative_submodules_of_stub_package(Id(2c04)): executing query
INFO semantic_index(Id(c10)): executing query
INFO parsed_module(Id(c10)): executing query
INFO source_text(Id(c10)): executing query
INFO file_to_module(Id(c10)): executing query
INFO resolve_module_query(Id(2405)): executing query
INFO imported_modules(Id(c0e)): executing query
INFO global_scope(Id(c10)): executing query
INFO place_table(Id(59be)): executing query
INFO place_by_id(Id(3006)): executing query
INFO use_def_map(Id(59be)): executing query
INFO infer_definition_types(Id(5f9c)): executing query
INFO infer_definition_types(Id(5f9b)): executing query
INFO infer_definition_types(Id(5ede)): executing query
INFO resolve_module_query(Id(2406)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3803)): executing query
INFO imported_relative_submodules_of_stub_package(Id(2c06)): executing query
INFO global_scope(Id(c24)): executing query
INFO semantic_index(Id(c24)): executing query
INFO parsed_module(Id(c24)): executing query
INFO source_text(Id(c24)): executing query
INFO place_table(Id(5a46)): executing query
INFO place_by_id(Id(3007)): executing query
INFO use_def_map(Id(5a46)): executing query
INFO infer_definition_types(Id(704b)): executing query
INFO file_to_module(Id(c24)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4401)): executing query
INFO infer_definition_types(Id(5edf)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3804)): executing query
INFO place_by_id(Id(3008)): executing query
INFO infer_definition_types(Id(7148)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4402)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c06)): executing query
INFO place_by_id(Id(3009)): executing query
INFO infer_definition_types(Id(5088)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c07)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c07)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c07)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c07)): executing query
INFO infer_definition_types(Id(3dbb)): executing query
INFO all_narrowing_constraints_for_expression(Id(1898)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c03)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c03)): executing query
INFO infer_deferred_types(Id(3dba)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c03)): executing query
INFO ClassLiteral < 'db >::decorators_(Id(1c03)): executing query
INFO enum_metadata(Id(1c03)): executing query
INFO ClassLiteral < 'db >::try_mro_(Id(4c01)): executing query
INFO place_by_id(Id(300a)): executing query
INFO infer_definition_types(Id(1404)): executing query
INFO global_scope(Id(c00)): executing query
INFO place_table(Id(1000)): executing query
INFO place_by_id(Id(300b)): executing query
INFO use_def_map(Id(1000)): executing query
INFO Type < 'db >::try_call_dunder_get_(Id(8800)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c01)): executing query
INFO place_by_id(Id(300c)): executing query
INFO infer_definition_types(Id(145e)): executing query
INFO infer_definition_types(Id(1422)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c08)): executing query
INFO resolve_module_query(Id(2407)): executing query
INFO global_scope(Id(c2f)): executing query
INFO semantic_index(Id(c2f)): executing query
INFO parsed_module(Id(c2f)): executing query
INFO source_text(Id(c2f)): executing query
INFO place_table(Id(5ba1)): executing query
INFO place_by_id(Id(300d)): executing query
INFO use_def_map(Id(5ba1)): executing query
INFO static_expression_truthiness(Id(1a3b)): executing query
INFO infer_expression_types_impl(Id(1a3b)): executing query
INFO infer_definition_types(Id(7437)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3805)): executing query
INFO imported_modules(Id(c2f)): executing query
INFO infer_definition_types(Id(751f)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3806)): executing query
INFO imported_relative_submodules_of_stub_package(Id(2c00)): executing query
INFO place_by_id(Id(300e)): executing query
INFO static_expression_truthiness(Id(184d)): executing query
INFO infer_expression_types_impl(Id(184d)): executing query
INFO infer_definition_types(Id(140c)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3807)): executing query
INFO imported_modules(Id(c01)): executing query
INFO infer_definition_types(Id(16f8)): executing query
INFO all_narrowing_constraints_for_expression(Id(184d)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c09)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c09)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c09)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c09)): executing query
INFO ClassLiteral < 'db >::pep695_generic_context_(Id(1c08)): executing query
INFO ClassLiteral < 'db >::explicit_bases_(Id(1c08)): executing query
INFO ClassLiteral < 'db >::inherited_legacy_generic_context_(Id(1c08)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c00)): executing query
INFO ClassLiteral < 'db >::try_mro_(Id(4c02)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c01)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4403)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c02)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c03)): executing query
INFO FunctionType < 'db >::signature_(Id(4803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3808)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c04)): executing query
INFO file_settings(Id(c00)): executing query
INFO suppressions(Id(c00)): executing query
INFO place_by_id(Id(300f)): executing query
INFO static_expression_truthiness(Id(1863)): executing query
INFO infer_expression_types_impl(Id(1863)): executing query
INFO infer_definition_types(Id(1712)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3809)): executing query
INFO imported_modules(Id(c02)): executing query
INFO infer_definition_types(Id(17d0)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380a)): executing query
INFO place_by_id(Id(3010)): executing query
INFO static_expression_truthiness(Id(1a01)): executing query
INFO infer_expression_types_impl(Id(1a01)): executing query
INFO infer_definition_types(Id(7022)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380b)): executing query
INFO imported_modules(Id(c24)): executing query
INFO infer_definition_types(Id(7381)): executing query
INFO FunctionType < 'db >::signature_(Id(4804)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4404)): executing query
INFO infer_deferred_types(Id(7381)): executing query
INFO infer_definition_types(Id(7138)): executing query
INFO infer_definition_types(Id(708e)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO place_by_id(Id(3011)): executing query
INFO infer_definition_types(Id(1409)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4405)): executing query
INFO infer_expression_type_impl(Id(1800)): executing query
INFO infer_expression_types_impl(Id(1800)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO infer_definition_types(Id(1405)): executing query
INFO infer_scope_types(Id(1001)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1801)): executing query
INFO infer_expression_types_impl(Id(1801)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a400)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1802)): executing query
INFO infer_expression_types_impl(Id(1802)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a401)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c05)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6804)): executing query
INFO code_generator_of_class(Id(6401)): executing query
INFO ClassLiteral < 'db >::try_metaclass_(Id(1c04)): executing query
INFO ClassLiteral < 'db >::try_mro_(Id(4c03)): executing query
INFO ClassLiteral < 'db >::is_typed_dict_(Id(1c04)): executing query
INFO place_table(Id(11eb)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6805)): executing query
INFO use_def_map(Id(11eb)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c02)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c03)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6806)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c06)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6807)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6808)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::class_member_with_policy_(Id(6c07)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6809)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(680a)): executing query
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::is_redundant_with_(Id(8c04)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c05)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(680b)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(680c)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): execute: iterate again (IterationCount(1))...
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a402)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a403)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c06)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c07)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): execute: iterate again (IterationCount(2))...
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a404)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a405)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): execute: iterate again (IterationCount(3))...
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a406)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a407)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): execute: iterate again (IterationCount(4))...
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a408)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a409)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::is_redundant_with_(Id(8c08)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c09)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c0a)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c0b)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c0c)): executing query
INFO Type < 'db >::is_redundant_with_(Id(8c0d)): executing query
INFO ClassLiteral < 'db >::decorators_(Id(1c07)): executing query
INFO enum_metadata(Id(1c07)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380f)): executing query
INFO code_generator_of_class(Id(6402)): executing query
INFO ClassLiteral < 'db >::try_metaclass_(Id(1c07)): executing query
INFO ClassLiteral < 'db >::try_mro_(Id(4c04)): executing query
INFO place_table(Id(122b)): executing query
INFO place_by_id(Id(3012)): executing query
INFO use_def_map(Id(122b)): executing query
INFO infer_definition_types(Id(5007)): executing query
INFO Type < 'db >::try_call_dunder_get_(Id(8801)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c08)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4406)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c09)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c0a)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c0b)): executing query
INFO code_generator_of_class(Id(6403)): executing query
INFO ClassLiteral < 'db >::try_metaclass_(Id(1c05)): executing query
INFO place_table(Id(1211)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(680d)): executing query
INFO use_def_map(Id(1211)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(680e)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3810)): executing query
INFO Type < 'db >::try_call_dunder_get_(Id(8802)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c0c)): executing query
INFO ClassLiteral < 'db >::decorators_(Id(1c05)): executing query
INFO enum_metadata(Id(1c05)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3811)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c0d)): executing query
INFO place_by_id(Id(3013)): executing query
INFO infer_definition_types(Id(3ff2)): executing query
INFO Type < 'db >::try_call_dunder_get_(Id(8803)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c0e)): executing query
INFO FunctionLiteral < 'db >::overloads_and_implementation_(Id(4407)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c0f)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c10)): executing query
INFO FunctionType < 'db >::signature_(Id(4807)): executing query
INFO infer_deferred_types(Id(3ff2)): executing query
INFO place_by_id(Id(3014)): executing query
INFO FunctionType < 'db >::signature_(Id(4807)): execute: iterate again (IterationCount(1))...
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): execute: iterate again (IterationCount(5))...
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a40a)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a40b)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Type < 'db >::member_lookup_with_policy_(Id(380c)): execute: iterate again (IterationCount(6))...
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6801)): executing query
INFO infer_expression_type_impl(Id(1803)): executing query
INFO infer_expression_types_impl(Id(1803)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380d)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6802)): executing query
INFO infer_expression_type_impl(Id(1804)): executing query
INFO infer_expression_types_impl(Id(1804)): executing query
INFO infer_definition_types(Id(1406)): executing query
INFO infer_expression_types_impl(Id(a40a)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(380e)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(6803)): executing query
INFO infer_expression_type_impl(Id(1805)): executing query
INFO infer_expression_types_impl(Id(1805)): executing query
INFO infer_definition_types(Id(1407)): executing query
INFO infer_expression_types_impl(Id(a40b)): executing query
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380e)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle infer_definition_types(Id(1406)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO Detected nested cycle Type < 'db >::member_lookup_with_policy_(Id(380d)), iterate it as part of the outer cycle Type < 'db >::member_lookup_with_policy_(Id(380c))
INFO inferable_typevars_inner(Id(a000)): executing query
INFO Type < 'db >::apply_specialization_(Id(ac00)): executing query
INFO Type < 'db >::apply_specialization_(Id(ac01)): executing query
INFO Type < 'db >::apply_specialization_(Id(ac02)): executing query
INFO Type < 'db >::apply_specialization_(Id(ac03)): executing query
INFO ClassLiteral < 'db >::inheritance_cycle_(Id(1c00)): executing query
INFO infer_scope_types(Id(1002)): executing query
INFO infer_definition_types(Id(1400)): executing query
INFO Type < 'db >::member_lookup_with_policy_(Id(3812)): executing query
INFO Type < 'db >::class_member_with_policy_(Id(6c11)): executing query
INFO ClassLiteral < 'db >::implicit_attribute_inner_(Id(680f)): executing query
INFO infer_expression_types_impl(Id(a40c)): executing query
INFO infer_expression_types_impl(Id(a40d)): executing query
INFO infer_expression_types_impl(Id(a40e)): executing query
INFO infer_scope_types(Id(1003)): executing query
INFO infer_expression_types_impl(Id(a40f)): executing query
INFO infer_expression_types_impl(Id(a410)): executing query
INFO Checking file `C:\Users\sbym8\Desktop\typepy\cycle.py` took more than 100ms (734.6105ms)
INFO line_index(Id(c00)): executing query
warning[undefined-reveal]: `reveal_type` used without importing it
  --> cycle.py:13:1
   |
12 | m = ManyCycles()
13 | reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   | ^^^^^^^^^^^
14 | reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
15 | reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   |
info: This is allowed for debugging convenience but will fail at runtime
info: rule `undefined-reveal` is enabled by default

info[revealed-type]: Revealed type
  --> cycle.py:13:13
   |
12 | m = ManyCycles()
13 | reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   |             ^^^^ `Unknown | int | Divergent(Id(380d)) | Divergent(Id(1406))`
14 | reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
15 | reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   |

warning[undefined-reveal]: `reveal_type` used without importing it
  --> cycle.py:14:1
   |
12 | m = ManyCycles()
13 | reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
14 | reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
   | ^^^^^^^^^^^
15 | reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   |
info: This is allowed for debugging convenience but will fail at runtime
info: rule `undefined-reveal` is enabled by default

info[revealed-type]: Revealed type
  --> cycle.py:14:13
   |
12 | m = ManyCycles()
13 | reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
14 | reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
   |             ^^^^ `Unknown | int | Divergent(Id(1406))`
15 | reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   |

warning[undefined-reveal]: `reveal_type` used without importing it
  --> cycle.py:15:1
   |
13 | reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
14 | reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
15 | reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   | ^^^^^^^^^^^
   |
info: This is allowed for debugging convenience but will fail at runtime
info: rule `undefined-reveal` is enabled by default

info[revealed-type]: Revealed type
  --> cycle.py:15:13
   |
13 | reveal_type(m.x1)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
14 | reveal_type(m.x2)  # should be: Unknown | int, but: Unknown | int | Divergent
15 | reveal_type(m.x3)  # should be: Unknown | int, but: Unknown | int | Divergent | Divergent
   |             ^^^^ `Unknown | int | Divergent(Id(380d)) | Divergent(Id(1406))`
   |

Found 6 diagnostics
```

</details>

---

_Comment by @MichaReiser on 2025-11-06 13:56_

> Hmm, it looks like the problem is still not solved...

Were you able to figure out what's happening? Is the issue that the `Divergent` types remain in the final output?

---

_Comment by @mtshiba on 2025-11-06 14:57_

This example might make it easier to see what's happening.

```python
class A: ...
class B: ...
class C: ...

class Foo:
    def __init__(self):
        self.a = A()
        self.b = B()
        self.c = C()

    def f1(self):
        self.a = self.b
    def f2(self, cond: bool):
        self.b = self.c if cond else self.a
    def f3(self):
        self.c = self.b

# outermost cycle head
#   0th: Divergent(Foo.a)
#   1st: Unknown | A | B | C | Divergent(Foo.a) ->(strip divergent) Unknown | A | B | C
#   2nd: Unknown | A | B | C | Divergent(Foo.a) -> Unknown | A | B | C
reveal_type(Foo().a)
# iterated as part of the outer cycle implicit_attribute_inner(Foo.a)
#   0th: Divergent(Foo.b)
#   1st: Unknown | B | C | Divergent(Foo.b) | Divergent(Foo.a) -> Unknown | B | C | Divergent(Foo.a)
#   2nd: Unknown | B | C | Divergent(Foo.a) | A -> Unknown | B | C | Divergent(Foo.a) | A
#   3rd: Unknown | B | C | Divergent(Foo.a) | A -> Unknown | B | C | Divergent(Foo.a) | A
reveal_type(Foo().b)
# not a cycle head
#   0th: Unknown | C | Divergent(Foo.b)
#   1st: Unknown | C | B | Divergent(Foo.a)
#   2nd: Unknown | C | B | Divergent(Foo.a) | A
#   3rd: Unknown | C | B | Divergent(Foo.a) | A
reveal_type(Foo().c)
```

```mermaid
graph LR
	Foo.a-->Foo.b
	Foo.b-->Foo.c
	Foo.b-->Foo.a
    Foo.c-->Foo.b
```

The cycle recovery function for `implicit_attribute_inner(Foo.a)` can remove `Divergent(Foo.a)`, but before that, `Divergent(Foo.a)` has infected `Foo.b` and `Foo.c`.

---

_Comment by @MichaReiser on 2025-11-06 15:49_

> The cycle recovery function for implicit_attribute_inner(Foo.a) can remove Divergent(Foo.a), but before that, Divergent(Foo.a) has infected Foo.b and Foo.c.

Can't we replace all `Divergent` types when finalizing?

---

_Comment by @mtshiba on 2025-11-06 16:21_

> Can't we replace all `Divergent` types when finalizing?

The cycle recovery function cannot remove `Divergent` types originating from other queries, as this would prevent it from detecting divergence. Also, when a query converges, if the result type contains a `Divergent` type originating from another query, it must not be removed either, as queries dependent on that query may not yet have converged.

---

_Comment by @mtshiba on 2025-11-06 16:42_

As a PoC, I modified salsa so that the cycle recovery function receives `CycleHeads`, and removes the `Divergent` types originating from all outer cycles https://github.com/astral-sh/ruff/pull/20566/commits/d0b68f9fa9c3401d2172c4e44cf7f5e6c2425125.

This is something we wanted to avoid, but it seems to work.

---

_Comment by @MichaReiser on 2025-11-06 16:49_

> As a PoC, I modified salsa so that the cycle recovery function receives CycleHeads, and removes the Divergent types originating from all outer cycles https://github.com/astral-sh/ruff/commit/d0b68f9fa9c3401d2172c4e44cf7f5e6c2425125.

It's not clear to me how this is different from removing all divergent types. As in, you can only see `Divergent` types for queries participating in a cycle, which is the same as the queries in `CycleHeads`.

---

_Comment by @github-actions[bot] on 2025-11-06 17:05_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @MichaReiser on 2025-11-08 16:08_

For my understanding. Are we now removing all instances of `Divergent` in `cycle_fn` (or of the cycle heads) regardless if this head has converged (`last_provisional == value`) or do we only remove the `Diverged` when the head has converged?

I'm asking because there are instances where a cycle head in iteration N becomes a normal cycle participant (not a head) in `N+1`. In which case `cycle_fn` is never called with two equal values.




---

_Comment by @carljm on 2025-11-11 00:46_

> It's not clear to me how this is different from removing all divergent types. As in, you can only see `Divergent` types for queries participating in a cycle, which is the same as the queries in `CycleHeads`.

It's true that all `Divergent` types must originate from cycles, but not necessarily from _this_ cycle. There are recursive-type cases where the final converged type includes a `Divergent` type, and that's the best we can do (the alternative would be an infinitely-nested type.) So we can easily have, for example, a `list[Divergent]` type as the final result of some recursive list type unrelated to the current cycle, and then a query in the current cycle could index into that list and obtain the unrelated `Divergent` type, which the current cycle should not treat specially or eliminate. The `Divergent` types that should be treated specially are only those originating from the current cycle.

It makes sense to me (unfortunately) that we need the information from `CycleHeads`. I agree we don't want to expose too much Salsa internals, but I note that most of `CycleHeads` is only `pub(crate)`, so isn't exposed. The only thing exposed is [the new `ids` method](https://github.com/salsa-rs/salsa/compare/master...mtshiba:salsa:expose-cycle-heads#diff-ab78fe8e8a3b757307bea73776713d03698bdeafc96cd9ab65d58ea97d24c24aR241), which really is the minimum information we need to expose, I think.

> For my understanding. Are we now removing all instances of `Divergent` in `cycle_fn` (or of the cycle heads) regardless if this head has converged (`last_provisional == value`) or do we only remove the `Diverged` when the head has converged?

It has to be removed on any iteration, this is how we get to the right converged value. You can see this happening in https://github.com/astral-sh/ruff/pull/20566#issuecomment-3497656147

---

_Comment by @mtshiba on 2025-11-11 02:33_

> It's not clear to me how this is different from removing all divergent types. As in, you can only see `Divergent` types for queries participating in a cycle, which is the same as the queries in `CycleHeads`.

In salsa's execution model since salsa-rs/salsa#999, queries that are inner cycle heads never see the converged values ​​of outer cycle heads. Therefore, to remove `Divergent` from the output of all queries, we need to perform convergence steps on the values ​​from outer cycle heads as well.

> For my understanding. Are we now removing all instances of Divergent in cycle_fn (or of the cycle heads) regardless if this head has converged (last_provisional == value) or do we only remove the Diverged when the head has converged?

`cycle_fn` sees the provisional value, and if it looks like `T | Divergent(ID associated with this cycle)`, it means the value has not converged (note that this "convergence" is a subtly different meaning from the convergence determined by salsa; what it means here is that it is possible to get closer to a more stable/simplified value from the type system's perspective. If it "converges" in this sense, it will also converge in the salsa sense), so we set it to `T`.
This is an operation that always guides the calculation toward convergence, and does not change the handling depending on whether or not the provisional value has converged.

> I'm asking because there are instances where a cycle head in iteration N becomes a normal cycle participant (not a head) in N+1. In which case cycle_fn is never called with two equal values.

I don't think this is a problem. Roughly speaking, cycle_fn performs cycle value normalization that is stronger than the convergence criteria required by salsa, so once salsa determines that the query has converged, cycle_fn does not need to be run again in the next iteration. This means that the change in salsa-rs/salsa#1021 (performing `recover_from_cycle` regardless of whether the values ​​have converged) was not actually necessary in this case...

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:277 on 2025-11-11 08:33_

I think we can track `level` in this visitor (we also don't need level, a single bool if nested shouold be enough)
```suggestion
    fn visit(&self, item: Type<'db>, func: impl FnOnce() -> Type<'db>) -> Type<'db> {
        self.transformer.visit(item, || {
            self.nested = true;
            let res = func();
            self.nested = false;
            res
        })
    }

    fn visit_no_shift(&self, item: Type<'db>, func: impl FnOnce() -> Type<'db>) -> Type<'db> {
        self.transformer.visit_no_shift(item, || {
            self.nested = true;
            let res = func();
            self.nested = false;
            res
        })
    }

```

We could probably even get away by simply always setting `nested` to `true` (outside the callback)

---

_@MichaReiser reviewed on 2025-11-11 08:33_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1591 on 2025-11-11 08:39_

Can we limit this to `level == 1` to avoid recursively nested types or should `has_divergent_type` be limited to only traverse one level deep?

---

_@MichaReiser reviewed on 2025-11-11 08:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:447 on 2025-11-11 08:42_

This should remove the need to suppress `needless_pass_by_value` at all call sites

```suggestion
    place: Place<'db>,`
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:444 on 2025-11-11 08:46_

Nit: Should we make this a method on `Place` instead (and move it to `place.rs` to not add more to `types.rs`) and maybe even add it to `PlaceAndQualifiers` to reduce some of the boilerplate?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:317 on 2025-11-11 09:52_

Nit: Consider making this a method on `ExpressionInference`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1590 on 2025-11-11 09:56_

I don't think I understand this comment, specifically how it is related to union handling, given that `level == 0`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1593 on 2025-11-11 10:03_

I think this works well for the above mentioned case but what if we have `list[list[Divergent]]` after the first iteration and `list[list[list[list[Divergent]]]]` after the second. Wouldn't it be better to return the `previous_type` in that case? 

---

_Comment by @MichaReiser on 2025-11-11 10:44_

> In salsa's execution model since https://github.com/salsa-rs/salsa/pull/999, queries that are inner cycle heads never see the converged values ​​of outer cycle heads. Therefore, to remove Divergent from the output of all queries, we need to perform convergence steps on the values ​​from outer cycle heads as well.

Inner cycles should see the converged value from the outer-most cycle in the very last iteration. 


I'm not able to come up with an example from the top of my head but I wonder if it's possible to end up with `Divergent | Divergent` types similar to https://github.com/astral-sh/ruff/pull/20566#issuecomment-3492424559, for example, when a variable depends on the output of two separate queries that both are divergent (but aren't part of the same cycle). I guess that's more an UX issue than a convergence issue because the result converges, it just looks silly to have two `Divergent` in the output.

Which is related to 

> The Divergent types that should be treated specially are only those originating from the current cycle.

My assumption was that we eliminate all `Divergent` types when the cycle converges but I think that's simply not possible because Salsa has no hook to post-process all memos after a cycle converged. 

> It makes sense to me (unfortunately) that we need the information from CycleHeads. I agree we don't want to expose too much Salsa internals, but I note that most of CycleHeads is only pub(crate), so isn't exposed. The only thing exposed is [the new ids method](https://github.com/salsa-rs/salsa/compare/master...mtshiba:salsa:expose-cycle-heads#diff-ab78fe8e8a3b757307bea73776713d03698bdeafc96cd9ab65d58ea97d24c24aR241), which really is the minimum information we need to expose, I think.

I can see how this is useful but we should maybe reconsider the `cycle_fn` API overall. Its ergonomics is very poor with the amount of arguments it has and I still don't think we should expose the `CycleHeads` type. I don't mind exposing the `ids` but I rather not expose `CycleHeads` directly.




---

_@MichaReiser reviewed on 2025-11-11 10:45_

---

_Comment by @mtshiba on 2025-11-13 19:28_

> I can see how this is useful but we should maybe reconsider the `cycle_fn` API overall. Its ergonomics is very poor with the amount of arguments it has and I still don't think we should expose the `CycleHeads` type. I don't mind exposing the `ids` but I rather not expose `CycleHeads` directly.

We can store all arguments in a single struct using the following way.
First, define the following trait and struct in salsa:

```rust
pub trait TrackedFn {
    type DbView: ?Sized + crate::Database;
    type Input<'db>: Send + Sync;
    type Output<'db>: Send + Sync;
}

pub struct CycleRecoveryContext<'db, 'c, 'l, F: TrackedFn> {
    pub db: &'db F::DbView,
    pub cycle_head_ids: CycleHeadIdsIterator<'c>, // Iterator that returns only `Id`s from `CycleHeads`
    pub last_provisional_value: &'l F::Output<'db>,
    pub value: F::Output<'db>,
    pub iteration: u32,
    pub input: F::Input<'db>,
}

pub trait Configuration: Any {
    type TrackedFn: TrackedFn;
    ...
}
```

When setting up the tracked function, generate the following code to implement `TrackedFn`:

```rust
$vis struct $fn_name {
    _priv: std::convert::Infallible,
}
impl salsa::plumbing::function::TrackedFn for $fn_name {
    type DbView = dyn $Db;
    type Input<'db> = ($($interned_input_ty),*);
    type Output<'db> = $output_ty;
}
```

Then, pass `CycleRecoveryContext` to `C::recover_from_cycle`. cycle_fn receives the value through the parameter of type `CycleRecoveryContext<'db, '_, '_, fn_name>` (this works fine, even though we need to define a cycle recovery function before using the `tracked` macro).

The problem with this approach is that it simply doesn't work for tracked methods. So, for example, we can define a submodule like the following to provide pseudo-access to the method, but it seems a bit hacky.

```rust
mod __salsa { // some fresh name
    pub mod Type {
        pub struct method {}
    }
}

CycleRecoveryContext<'db, '_, '_, __salsa::Type::method>
```

---

_Comment by @mtshiba on 2025-11-13 19:30_

I ran the fuzzer with seeds 0-100000 and found that only 4 seeds (25199 35473 44447 97225) caused panics, all of which were stack overflows (which will be fixed in #21434), and that the too-many-cycle-iteration panic no longer occurs.

---

_Comment by @mtshiba on 2025-11-13 19:38_

> Inner cycles should see the converged value from the outer-most cycle in the very last iteration.

I think that by performing cycle normalization on all cycle heads at the inner cycle heads, the inner cycles indirectly see the normalized values ​​of the outer cycles.

---

_Comment by @MichaReiser on 2025-11-14 13:25_

>  First, define the following trait and struct in salsa:

I considered a `Context` too but I think the lifetimes and many generics don't make for a very ergonomic API. For me, the main goal is to avoid that exposing more information to the cycle function isn't a breaking API change in the future. That's why I'm leaning towards something like this:


```rust
fn cycle_fn(db: &dyn Db, value: T, cycle: &CycleInformation<T>, input: Type) -> Type {
	
}
```

Where `CycleInformation` exposes:

* `previous_value`
* `query_id` (or just id?)
* `heads` (or `head_ids`): The ids of all cycles including `id`
* `iteration`

Or the following if we want to avoid any generics

```
fn cycle_fn(db: &dyn Db, value: T, previous_value: &T, cycle: &CycleInformation, input: Type) -> Type {
	
}
```

---

_Comment by @MichaReiser on 2025-11-14 16:26_

> I'm not able to come up with an example from the top of my head but I wonder if it's possible to end up with Divergent | Divergent types similar to https://github.com/astral-sh/ruff/pull/20566#issuecomment-3492424559, for example, when a variable depends on the output of two separate queries that both are divergent (but aren't part of the same cycle). I guess that's more an UX issue than a convergence issue because the result converges, it just looks silly to have two Divergent in the output.

A more concrete example here would be:

* `a -> b -> a`, infers `list[Divergent(a)]`
* `c -> d -> c`, infers `list[Divergent(c)]`
* `e -> a`, `e -> c`, infers `a | c`

Is the result of `e` `list[Divergent] | list[Divergent]` or do they get simplified somehow?

---

_Comment by @carljm on 2025-11-18 02:46_

Can we converge on an agreed Salsa API so we can implement it and move this forward? @mtshiba how does @MichaReiser's proposed `CycleInformation` above look to you? (IMO we could just name it `Cycle` -- the old Salsa cycle handling had an exposed type like this.)

---

_Comment by @carljm on 2025-11-18 02:46_

Are there unsolved issues here besides the shape of the Salsa API?

---

_Comment by @mtshiba on 2025-11-20 20:09_

> Can we converge on an agreed Salsa API so we can implement it and move this forward? @mtshiba how does @MichaReiser's proposed `CycleInformation` above look to you? (IMO we could just name it `Cycle` -- the old Salsa cycle handling had an exposed type like this.)

Yes, I've opened a PR to add the new salsa API.

https://github.com/salsa-rs/salsa/pull/1028

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-11-21 09:47_

---

_Review comment by @carljm on `Cargo.toml`:149 on 2025-11-22 02:20_

Since the Salsa PR is merged, we can now use an upstream commit again?

I made https://github.com/astral-sh/ruff/pull/21575 to upgrade Salsa with only minimal changes to make the existing code work with the new Salsa API, to isolate the Salsa upgrade from this PR. If that's not useful and you'd rather just roll those changes into this PR, let me know and I can close it.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:708 on 2025-11-22 02:26_

Do we also need to ensure monotonicity between `Undefined` and `Defined`? We have seen oscillation examples before (for instance attributes) that oscillate between `Undefined` and `Defined`. For example:

```py
from typing import Literal

class C:
    def __init__(self, other: "C"):
        if not other.x:
            self.x: Literal[True] = True

def _(c: C):
    reveal_type(c.x)
```

I think we worked around this by turning off reachability analysis for finding implicit instance attributes? Maybe that's a fine solution and we can keep it, but I wonder if this can also occur in other cases.

(This is not a blocker for this PR, I'm just curious; we can just make it a TODO comment to consider later.)

---

_@carljm reviewed on 2025-11-22 02:50_

Haven't finished reading through this, but have to stop for tonight -- just a couple comments so far.

---

_Comment by @mtshiba on 2025-11-22 06:15_

The CI results, for easier review:

conformance test: https://github.com/astral-sh/ruff/pull/20566#issuecomment-3449548944
codspeed: https://github.com/astral-sh/ruff/pull/20566#issuecomment-3449594968
mypy_primer: https://github.com/astral-sh/ruff/pull/20566#issuecomment-3481242151
ruff-ecosystem: https://github.com/astral-sh/ruff/pull/20566#issuecomment-3498395810

---

_@mtshiba reviewed on 2025-11-22 15:31_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/place.rs`:708 on 2025-11-22 15:31_

I was reconsidering the problem while looking at https://github.com/astral-sh/ruff/pull/19579, and I think we might be able to use the mechanism introduced in this PR to improve our reachability analysis.

As you pointed out, we currently set the truthiness of places that aren't definitely-bound to `Ambiguous`, which guarantees convergence for the definedness of places. However, there is a slight downside to this approach. For example:

```python
def _(flag: bool):
    if flag:
        ALWAYS_TRUE_IF_BOUND = True

    # error: [possibly-unresolved-reference] "Name `ALWAYS_TRUE_IF_BOUND` used when possibly not defined"
    if True and ALWAYS_TRUE_IF_BOUND:
        x = 1
    else:
        x = 2

    reveal_type(x) # should be: Literal[1]
```

Currently, we infer `x: Literal[1, 2]`, but logically `Literal[1]` is better. This is because if `ALWAYS_TRUE_IF_BOUND` were not defined, an error would occur and the `x = 2` branch would never be executed anyway.
The new cycle recovery mechanism can merge the definedness of the current cycle's `Place` with the previous cycle's `Place`, allowing us to unambiguously determine the reachability of a definition while maintaining convergence.
In fact, in #20566 I confirmed that `static_expression_truthiness` no longer causes a panic even without the use of `all_places_definitely_bound`.

However, this presents another issue.

```python
from typing import Literal

class Toggle:
    def __init__(self: "Toggle"):
        if not self.x:
            self.x: Literal[True] = True

reveal_type(Toggle().x) # should be: Literal[True]
```

`Toggle().x` should be `Literal[True]`, but with the above change, it was inferred to be `Literal[True] | Unknown`.
After investigating why this was happening, I found that it was because `Literal` was inferred to be `Never`. The branch with `self.x: Literal[True] = True` was considered to be unreachable in a cycle, so the type annotation in that branch became `Never`. If we make a change like the one above, we should also make changes so that annotations in the unreachable code are not inferred to be `Never`.

In any case, this seems like something to consider after the merge, rather than something that should be done in this PR.


---

_Comment by @mtshiba on 2025-11-22 18:08_

> > I'm not able to come up with an example from the top of my head but I wonder if it's possible to end up with Divergent | Divergent types similar to [#20566 (comment)](https://github.com/astral-sh/ruff/pull/20566#issuecomment-3492424559), for example, when a variable depends on the output of two separate queries that both are divergent (but aren't part of the same cycle). I guess that's more an UX issue than a convergence issue because the result converges, it just looks silly to have two Divergent in the output.
> 
> A more concrete example here would be:
> 
> * `a -> b -> a`, infers `list[Divergent(a)]`
> * `c -> d -> c`, infers `list[Divergent(c)]`
> * `e -> a`, `e -> c`, infers `a | c`
> 
> Is the result of `e` `list[Divergent] | list[Divergent]` or do they get simplified somehow?

This can actually happen.

```python
class ManyCycles:
    def __init__(self: "ManyCycles"):
        self.x1 = [0]
        self.x2 = [1]
        self.x3 = [1]

    def f1(self: "ManyCycles"):
        reveal_type(self.x3)  # revealed: Unknown | list[Unknown | int] | list[Divergent] | list[Divergent]

        self.x1 = [self.x2] + [self.x3]
        self.x2 = [self.x1] + [self.x3]
        self.x3 = [self.x1] + [self.x2]

    def f2(self: "ManyCycles"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2

    def f3(self: "ManyCycles"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2
```

This may seem like a strange type to users, but `list[Divergent(a)] | list[Divergent(b)]` should behave equivalently to `list[Unknown]` as a dynamic type, so as you say, this is just a UX issue rather than a bug. For reference, the result of pyright is also ugly (`list[int] | list[list[list[list[int] | list[list[list[list[int] | Unknown]] | Unknown]] | Unknown`).
To fix this, we could combine `Divergent`s of different origins into a single `Divergent` with multiple `Id`s (`list[Divergent(a)] | list[Divergent(b)] => list[Divergent(a, b)]`).
I personally think this is not a high priority and should be a TODO task.

---

_Comment by @mtshiba on 2025-11-22 18:37_

I think most of the issues have been resolved. The remaining issue is this:

`mdtest/generic/legacy/classes.md`:

```pyi
# pyi

from typing_extensions import Generic, TypeVar

T = TypeVar("T")

# TODO: no error "Unsupported class base with type `<class 'list[Derived[T@Derived]]'> | <class 'list[@Todo(specialized non-generic class)]'>`"
# error: [unsupported-base]
class Derived(list[Derived[T]], Generic[T]): ...
```

~~The reason `@Todo(specialized non-generic class)` appears is because the generic context of `Derived` may become `None` in a cycle.
`(inherited_)legacy_generic_context` extracts `Generic[T]` or type aliases from `explicit_bases`, but if there is a self-referential class in the base classes, all base classes will become `Divergent` and the type variables cannot be extracted.~~

The problem is probably unioning with values ​​from the previous cycle.
In the first cycle, `explicit_bases` calculates that `<class 'list[@Todo(specialized non-generic class)]'>` is one of the bases of `Derived`. This is the same behavior as main. In the next cycle, this base can be correctly inferred to `<class 'list[Derived[T]]'>`, but in #20566, cycle normalization takes a union with the type from the previous cycle, so the base becomes `<class 'list[Derived[T]]'> | <class 'list[@Todo]'>`.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:1938 on 2025-11-24 14:30_

This addresses the bug identified at https://github.com/astral-sh/ruff/pull/21434.

---

_Renamed from "[ty] WIP: handle recursive type inference properly" to "[ty] handle recursive type inference properly" by @mtshiba on 2025-11-24 14:31_

---

_Marked ready for review by @mtshiba on 2025-11-24 14:32_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-24 14:32_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-24 14:32_

---

_Review requested from @dcreager by @mtshiba on 2025-11-24 14:32_

---

_Comment by @mtshiba on 2025-11-24 14:32_

OK, I've already had some early reviews, but I'm declaring this PR ready for everyone's review now.

---

_@mtshiba reviewed on 2025-11-24 14:32_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:966 on 2025-11-24 15:07_

I implemented inference for union subscript type expressions because I noticed that it could suppress the following error:

```python
from typing import TypeVar, TypeAlias, Union

K = TypeVar("K")
V = TypeVar("V")
# TODO: no error
# error: [invalid-type-form] "Invalid subscript of object of type `<class 'dict[typing.TypeVar, typing.TypeVar | Unknown]'> | <class 'dict[typing.TypeVar, typing.TypeVar | @Todo(specialized generic alias in type expression)]'> | <class 'dict[typing.TypeVar, Divergent]'>` in type expression"
NestedDict: TypeAlias = dict[K, Union[V, "NestedDict[K, V]"]]
```

However, this may not be an essential solution to the error. There's also the question of whether union subscript type expressions should be allowed in the first place.

```python
type ListOrSet[T] = list[T] | set[T]
type Tuple1[T] = tuple[T]

def _(cond: bool):
    Generic = ListOrSet if cond else Tuple1

    def _(x: Generic[int]):
        reveal_type(x)  # revealed: list[int] | set[int] | tuple[int]
```

Pyrefly allows it, while Pyright rejects it.

---

_@mtshiba reviewed on 2025-11-24 15:08_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-25 01:55_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-25 01:55_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-25 02:10_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-25 02:10_

---

_Comment by @astral-sh-bot[bot] on 2025-11-25 02:19_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-form` | 0 | 67 | 16 |
| `invalid-argument-type` | 24 | 1 | 49 |
| `invalid-assignment` | 22 | 4 | 3 |
| `invalid-return-type` | 6 | 0 | 10 |
| `possibly-missing-attribute` | 2 | 6 | 7 |
| `no-matching-overload` | 11 | 0 | 0 |
| `type-assertion-failure` | 0 | 0 | 10 |
| `unsupported-operator` | 0 | 0 | 5 |
| `unused-ignore-comment` | 0 | 5 | 0 |
| `unsupported-base` | 4 | 0 | 0 |
| `not-iterable` | 0 | 0 | 2 |
| `unresolved-attribute` | 1 | 0 | 0 |
| **Total** | **70** | **83** | **102** |




---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:741 on 2025-11-25 02:24_

I see this occurring in ecosystem projects, so we will want to follow up on this soon; users may experience it as a regression.

> cycle normalization takes a union with the type from the previous cycle

It seems like maybe we could address this with a targeted fix in the cycle recovery for `explicit_bases` query? For class literal types we could skip unioning, or union the type parameters instead if the outer type is the same and the outer type is covariant in its type parameters? Not sure on details, would need to experiment.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:2471 on 2025-11-25 02:31_

My understanding of this TODO is that the aim would be to merge the two `list[Divergent]` -- not sure I understand why the `Unknown` would go away, or we'd merge an `int` in there too. Should the TODO be this, or am I missing something?
```suggestion
        # TODO: should be Unknown | list[Unknown | int] | list[Divergent]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:966 on 2025-11-25 02:40_

I think according to the type spec this should not be allowed, and we may want to forbid it in future.

I feel like this issue and the "unsupported-class-base" issue discussed elsewhere are both symptoms of the same problem, which is that creating a union type in fixpoint iteration where a union would otherwise be unexpected can cause undesirable side effects. I wonder if we can address this as follow-up with more intelligent unioning -- like perhaps when the outer generic type is the same we should union the type arguments instead?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:2021 on 2025-11-26 01:19_

I'm wondering if you have a better sense of why we need this. There is one test in `overloads.md` that fails without it, but in that that test `overload` resolves to `Never`, and I don't see where there would be a cycle at all. I'm wondering if you already debugged this and understand why we see a `Divergent` type here in that test?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:632 on 2025-11-26 01:37_

I think it would be better to just eliminate this method entirely, and turn any callsites into no-ops? That helps us "bubble up" and eliminate unnecessary no-op type transformations.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:1111 on 2025-11-26 01:42_

```suggestion
        Some(Self::new(
            db,
            self.generic_context(db),
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:590 on 2025-11-26 02:18_

I see many other `recursive_type_normalized_impl` methods that just propagate downwards whatever `nested` value they receive, and always propagate `None` upwards. But here we always propagate `nested=true` downwards, and only propagate `None` upwards if we were already nested. It's not clear to me why properties are different from some other types (like bound super, or tuples, or ...) in this regard. Can we add some documentation explaining more clearly how we should determine what behavior a `recursive_type_normalized_impl` method should have?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1663 on 2025-11-26 02:25_

Here again, it's not immediately clear to me why `TypeIs` is considered a "nesting boundary" (by which I mean, sets `nested` to `true` and and propagates `div` instead of `None` if called non-nested), when many other types are not.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:1938 on 2025-11-26 02:29_

It seems like we don't use the fully-expanded type here at all, unless it is `Divergent` (i.e. we found the type alias to be cyclic). Do we need a type-mapping (type transformation) and diverging Salsa fixpoint iteration in order to achieve this detection? Could we instead use a a simple type-visit (with cycle detection) which expands aliases, and track in the visitor whether we ever encountered a cycle?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8479 on 2025-11-26 02:33_

These types send `nested=true` downwards instead of simply `nested` -- but in this case they propagate `None` upwards regardless of the outer `nested` value. So that's now three different behaviors I've seen:

1. Send `nested` downward as-is, always propagate `None` upward.
2. Send `nested=true` downward, always propagate `None` upward.
3. Send `nested=true` downward, send `None` upward if called nested, otherwise replace `None` with `div`.

I think we need a clearer description of why these different behaviors exist, and how we know which one to use in any given case.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9296 on 2025-11-26 02:34_

I think we should also eliminate this method and turn its callsite(s) into no-ops, to make it clearer when the calling method can be simplified.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:12452 on 2025-11-26 02:37_

Same here; eliminate this method.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:13559 on 2025-11-26 02:40_

```suggestion
        let recursive = UnionType::from_elements(
```

---

_@carljm approved on 2025-11-26 02:44_

Thank you @mtshiba, this is really impressive work! You've solved a difficult problem, relatively cleanly, and improved performance in the process.

I would like to get this merged tomorrow if possible. I've left some comments and would love your responses to them, and any updates to this PR that you would like to make / have time to make in order to address those comments. If there are comments that you agree with but don't have time to address, feel free to just comment and I will try to address them tomorrow before merge. Some can certainly also be follow-ups in a separate PR after we land this one.

Thank you!

---

_@mtshiba reviewed on 2025-11-26 06:42_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:966 on 2025-11-26 06:42_

Here's a more realistic example.

```python
try:
    class Foo[T]:
        x: T
        def foo(self) -> T:
            return self.x
        ...
    ...
except Exception:
    class Foo[T]:
        x: T
        def foo(self) -> T:
            return self.x
        ...

def f(x: Foo[int]):
    reveal_type(x.foo())  # revealed: int?
```

Currently, ty infers `x.foo()` as `Unknown` (and claims that `Foo[int]` is an invalid type expression), while mypy and pyright can infer it as `int`.
Perhaps some kind of warning should be reported for such hacky code, but since the user's intent is clear and we have the ability to infer it, we can consider that we should do so.

---

_@mtshiba reviewed on 2025-11-26 06:52_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:966 on 2025-11-26 06:52_

> like perhaps when the outer generic type is the same we should union the type arguments instead?

Yeah, this is discussed at https://github.com/astral-sh/ty/issues/1308.

---

_@mtshiba reviewed on 2025-11-26 07:13_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:1938 on 2025-11-26 07:13_

I also think this method is a bit over-engineered if we just want to check the divergence of type aliases.

One future direction I consider is that `expand_eagerly` would allow us to infer recursive type aliases a bit more intelligently, like the following code (although this type alias itself should probably be reported as invalid):

```python
# TODO: this should probably be a cyclic-type-alias-definition error
type Foo[T] = list[T] | Bar[T]
type Bar[T] = int | Foo[T]

def _(x: Bar[int]):
    # currently: int | list[int] | Any
    reveal_type(x) # revealed: int | list[int]
```

This means, when expanding a type alias, first expand recursively, and if it diverges, call `value_type` as a fallback.

---

_@mtshiba reviewed on 2025-11-26 14:20_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:8479 on 2025-11-26 14:20_

Nominal types such as `list[T]` and `tuple[T]` should send `nested=true` for `T`. This is absolutely necessary for normalization.
Structural types such as union and intersection do not need to send `nested=true`; that is, types that are "flat" from the perspective of recursive types. `T | U` should send `nested` for `T`, `U`.

For other types, the decision depends on whether they are interpreted as nominal or structural.
For example, `KnownInstanceType::UnionType` should simply send `nested`. You can confirm that mdtest fails when you send `nested=true` for `value_expr_types`.
I haven't found any problematic cases for other types, but they can be interpreted as nominal, so `nested=true` should be sent.
At the very least, sending `nested=true` ensures that `Divergent` remains, so it's safe operation. With `nested=false`, `Divergent` may be eliminated.

---

_@mtshiba reviewed on 2025-11-26 14:56_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer/builder.rs`:2021 on 2025-11-26 14:56_

Here's the rough outline that leads to the cycle:

```
infer_definition_types
-> infer_function_definition
-> try_upcast_to_callable (is_input_function_like)
-> FunctionType::signature
-> definition_expression_type (Signature::from_function)
-> infer_definition_types
```

A basic principle of fixed-point iteration calculations is that errors should not be reported unnecessarily early in the cycle when sufficient information has not yet been gathered.
If `in_function_overload_or_abstractmethod` is `false`, additional checks will be performed and an error may be reported.
A natural implementation would therefore be to set `in_function_overload_or_abstractmethod` to `true` early in the cycle, and defer the decision to subsequent cycles until more detailed function information has been gathered.

---

_@mtshiba reviewed on 2025-11-26 15:10_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:741 on 2025-11-26 15:10_

I've already confirmed that unioning the type parameters eliminates this error: 058f8ed7ee46b65cff139022559a8fd38b2158c3

However, I've reverted the commit because I found the operation to be unsound (`Base2` is not covariant with `T`).
A sound solution would be to improve the type inference of legacy generic classes so that we somehow recognize that `Sub2` is a generic class from the beginning of the cycle.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-26 15:12_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8479 on 2025-11-26 15:48_

Ok, this makes sense, thanks for adding the comment.

It's also not entirely clear to me why some types that send `nested=True` downward will unconditionally propagate `None` upward (as in these cases here), but others will replace `None` with `div`. Is there a way to summarize the logic behind this difference?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:2021 on 2025-11-26 15:54_

Yes, I think the principle makes sense, I was just curious why we hit a cycle at all in this code. It looks like we may have a cycle for all overloaded function definitions, thanks to `is_input_function_like` checking. This doesn't seem ideal, but it's a separate issue unrelated to this PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1648 on 2025-11-26 15:55_

Now that this is a no-op, it doesn't seem like we need `visitor.try_visit` here.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-26 15:56_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-26 15:56_

---

_@carljm approved on 2025-11-26 16:01_

---

_Merged by @carljm on 2025-11-26 16:50_

---

_Closed by @carljm on 2025-11-26 16:50_

---

_Branch deleted on 2025-11-27 06:49_

---
