```yaml
number: 15161
title: "Don't special-case class instances in binary expression inference"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/infer-binary
created_at: 2024-12-27T15:54:49Z
updated_at: 2025-01-06T18:50:21Z
url: https://github.com/astral-sh/ruff/pull/15161
synced_at: 2026-01-10T20:34:00Z
```

# Don't special-case class instances in binary expression inference

---

_Pull request opened by @dcreager on 2024-12-27 15:54_

Just like in #15045 for unary expressions: In binary expressions, we were only looking for dunder expressions for `Type::Instance` types.  We had some special cases for coercing the various `Literal` types into their corresponding `Instance` types before doing the lookup.  But we can side-step all of that by using the existing `Type::to_meta_type` and `Type::to_instance` methods.

Closes #14549 

---

_Review requested from @carljm by @dcreager on 2024-12-27 15:54_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-27 15:54_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-27 15:54_

---

_Review requested from @sharkdp by @dcreager on 2024-12-27 15:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3503 on 2024-12-27 15:55_

I reordered these so that all of the special cases for literals appeared together

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3402 on 2024-12-27 15:56_

Had to add clauses for our other gradual type forms

---

_Label `red-knot` added by @AlexWaygood on 2024-12-27 15:56_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3521 on 2024-12-27 15:57_

These are now all handled by the `to_meta_type` calls down below.  The new approach is more complete: before we handled when each literal type appeared in an expression with an `Instance`, but not with other (different) literal types.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:15 on 2024-12-27 16:00_

This test produced a `@Todo` type before, since our literal coercions weren't complete

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3524 on 2024-12-27 16:01_

I've expanded this out into a list of all of the coercable types, just like @AlexWaygood  requested in https://github.com/astral-sh/ruff/pull/15045#discussion_r1890556626

---

_@dcreager reviewed on 2024-12-27 16:01_

---

_Comment by @codspeed-hq[bot] on 2024-12-27 16:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finfer-binary)

### Merging #15161 will **degrade performances by 4.5%**

<sub>Comparing <code>dcreager/infer-binary</code> (7149f42) with <code>main</code> (d45c1ee)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `dcreager/infer-binary` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 83.7 ms | 87.7 ms | -4.5% |


---

_Review comment by @dcreager on `crates/red_knot_workspace/tests/check.rs`:276 on 2024-12-27 19:58_

The test failures were because we're now noticing the recursiveness of some additional class definitions (https://github.com/astral-sh/ruff/issues/14672)

```py
class Tree(list[Tree | Leaf]): ...
```

We now see into the `BitOr` of two `ClassLiterals`, and in most cases, can see via the typeshed that the result is a `UnionType`.  But in these two fixtures, there's a recursive reference inside that union, which we can't yet handle.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-27 19:59_

This comes from the typeshed.  We might consider special-casing the `BitOr` of two `ClassLiteral`s to return a more specific `Type::Union`

---

_@dcreager reviewed on 2024-12-27 19:59_

---

_@dcreager reviewed on 2024-12-27 20:02_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-27 20:02_

(Note that this is not a type annotation, so none of the logic in `Type::in_type_expression` applies)

---

_Comment by @github-actions[bot] on 2024-12-27 20:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dcreager reviewed on 2024-12-27 20:15_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-27 20:15_

I went ahead and added this special case, but retaining the behavior that it's only available in Python ≥3.10

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-29 00:29_

I don't think `Literal[A, B]` is the right type to assign to the value expression `A | B`.

In a type expression, `A | B` spells the type `Type::Union[Type::Instance(A), Type::Instance(B)]` -- that is, the type consisting of all instances of `A` (or any subclass) and all instances of `B` (or any subclass).

The value expression `A if flag else B` would correctly be assigned the type `Literal[A, B]`, since its runtime value must either be the class object `A` or the class object `B`.

But the runtime value of the value expression `A | B` is not either of those, so its type can't be `Literal[A, B]`. Its runtime value is an instance of `types.UnionType`, which can't be treated as if it were a class object at all. So the type we should assign to it is `Type::Instance(types.UnionType)`.
```suggestion
reveal_type(A | B)  # revealed: UnionType
```

If we did any special-casing here, the correct special casing would be to create a custom type representation for `types.UnionType` so we can record the fact that its `__args__` attribute (in this case) has the type `tuple[Literal[A], Literal[B]]`. But I don't think this is worth doing.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/custom.md`:134 on 2024-12-29 00:32_

It feels like we are missing the `Yes() <op> No()` case here; is there a reason you didn't include that?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/custom.md`:192 on 2024-12-29 00:34_

This feels very closely related to the `## Classes` section above -- maybe group them together instead of having the intervening function literals section?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3369 on 2024-12-29 00:36_

This is the clause that I think we don't want; I expect typeshed will natively give us the correct type of `types.UnionType`?

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:276 on 2024-12-29 00:38_

Maybe eliminating the special case and just returning `types.UnionType` instead, as commented above, will help these cases?

---

_@carljm reviewed on 2024-12-29 00:38_

Looks great, thank you! Just one substantive comment.

---

_@carljm reviewed on 2024-12-29 00:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-29 00:44_

Oh, I commented on this in my review; I think the added special case is not correct, the type from typeshed is :)

---

_@carljm reviewed on 2024-12-29 00:45_

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:276 on 2024-12-29 00:45_

Oh, perhaps not; I see now that this comment was made before you added that special case.

---

_Comment by @carljm on 2024-12-29 00:50_

It's odd that this PR would cause a regression. The profiles on CodSpeed suggest that we are fetching some ingredient more often. I suspect that the cause is that improving completeness here means we now return a more complex type in lots of cases that would previously have just returned `Unknown`. Since this is just a natural consequence of better completeness, I'll go ahead and acknowledge the regression.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-30 13:59_

:+1: Sounds good!  I wasn't sure about which way to go, so I added the special case in a separate commit that would be easier to revert

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/classes.md`:16 on 2024-12-30 14:00_

Reverted the special case, this is now returning `UnionType` again as before

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/custom.md`:134 on 2024-12-30 14:07_

I wasn't sure if that would be needed, since `No` is not a subclass of `Yes`. So it doesn't matter whether it implements `__radd__`, we're always going to use `Yes.__add__`, and so that would give the same result as the `Yes() <op> Yes()` case.

So I think what I want is another stanza, where `Sub` and `No` both implemented the reflected methods, to show that we correctly use the `Sub` reflections since it's a subclass, but don't pick up the `No` reflections since it's not.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/binary/custom.md`:192 on 2024-12-30 14:08_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3369 on 2024-12-30 14:08_

Removed by the revert mentioned above

---

_@dcreager reviewed on 2024-12-30 14:24_

---

_Review request for @MichaReiser removed by @MichaReiser on 2024-12-30 14:25_

---

_@carljm approved on 2025-01-06 18:34_

Looks great, thanks for the thorough tests!

---

_Merged by @dcreager on 2025-01-06 18:50_

---

_Closed by @dcreager on 2025-01-06 18:50_

---

_Branch deleted on 2025-01-06 18:50_

---
