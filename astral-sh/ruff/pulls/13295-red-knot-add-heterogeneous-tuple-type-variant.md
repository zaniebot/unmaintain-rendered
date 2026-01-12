```yaml
number: 13295
title: "[red-knot] Add heterogeneous tuple type variant"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/tuple-type
created_at: 2024-09-09T20:50:11Z
updated_at: 2024-09-10T18:03:29Z
url: https://github.com/astral-sh/ruff/pull/13295
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Add heterogeneous tuple type variant

---

_@dhruvmanila_

## Summary

This PR adds a new `Type` variant called `TupleType` which is used for heterogeneous elements.

### Display notes

* For an empty tuple, I'm using `tuple[()]` as described in the docs: https://docs.python.org/3/library/typing.html#annotating-tuples
* For nested elements, it'll use the literal type instead of builtin type unlike Pyright which does `tuple[Literal[1], tuple[int, int]]` instead of `tuple[Literal[1], tuple[Literal[2], Literal[3]]]`. Also, mypy would give `tuple[builtins.int, builtins.int]` instead of `tuple[Literal[1], Literal[2]]`

## Test Plan

Update test case to account for the display change and add cases for multiple elements and nested tuple elements.


---

_Label `red-knot` added by @dhruvmanila on 2024-09-09 20:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-09-09 21:03_

[`typing.Tuple`](https://github.com/python/typeshed/blob/65170f4a2ef8dc81d3678624ac2a4610d1c6045e/stdlib/typing.pyi#L210) won't tell us much here ;) all the interesting stuff is in `builtins.pyi`

```suggestion
                // TODO: defer to `builtins.tuple` methods from typeshed's stubs
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1561 on 2024-09-09 21:03_

Seems like you did this ;)

```suggestion
```

---

_@dhruvmanila reviewed on 2024-09-09 21:03_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:199 on 2024-09-09 21:03_

Maybe we should use `TupleLiteral` instead? This is because there could also be `tuple[int, str]` or `tuple[int, ...]`. Later, we could merge them under the same enum and have them as a single variant in `Type::Tuple`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:482 on 2024-09-09 21:04_

This isn't correct: `typeshed_symbol_ty` looks things up in the (fictional) `_typeshed` module in typeshed, but we want to look things up in the `builtins` module:

https://github.com/astral-sh/ruff/blob/62c7d8f6ba118c85e65f79be2f3c0f992ad32b09/crates/red_knot_python_semantic/src/stdlib.rs#L57-L63

```suggestion
            Type::Tuple(_) => builtins_symbol_ty(db, "tuple"),
```

---

_@AlexWaygood reviewed on 2024-09-09 21:05_

---

_@dhruvmanila reviewed on 2024-09-09 21:07_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1561 on 2024-09-09 21:07_

I think there's still types like `tuple[int, str]` or `tuple[int, ...]`? Maybe I should update the comment

---

_@dhruvmanila reviewed on 2024-09-09 21:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-09-09 21:09_

Oh, thanks, that makes sense.

---

_@AlexWaygood reviewed on 2024-09-09 21:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:199 on 2024-09-09 21:11_

`TupleLiteral` sounds reasonable to me!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1561 on 2024-09-09 21:11_

Ah, fair point. Yeah, a more specific comment might be better!

---

_@AlexWaygood reviewed on 2024-09-09 21:11_

---

_Marked ready for review by @dhruvmanila on 2024-09-09 21:17_

---

_Review requested from @carljm by @dhruvmanila on 2024-09-09 21:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-09 21:17_

---

_Comment by @github-actions[bot] on 2024-09-09 21:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:199 on 2024-09-10 00:28_

I saw there was some discussion of this already, but I'm not sure I see the rationale for calling this `TupleLiteral` vs just `Tuple`. The current representation in this PR can already handle all tuple types other than `tuple[T, ...]`, and it wouldn't take much to add support for that, too (though I'm not suggesting we do it in this PR, and I'm not sure yet whether we should support that via this variant or via the regular builtin tuple type from typeshed). This representation can definitely already handle more tuple types than just literals.

Unless I'm missing the rationale, I would just call this `Type::Tuple`, and the contained struct `TupleType`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:368 on 2024-09-10 00:32_

We won't actually want to do this for all or even most methods. For example, we will want to special-case `__getitem__` to return an overloaded callable type which knows exactly which type to return (or raises an error) for each index it might be called with. We'll similarly need to special case many other methods: `__len__`, `__contains__`, `__iter__`, `count`, `index`, possibly some others as well.
```suggestion
                // TODO: implement tuple methods
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:672 on 2024-09-10 00:34_

Since this should be immutable once constructed, and we will have a lot of these and they'll be long-lived, do you think it makes sense to use a boxed slice instead?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1562 on 2024-09-10 00:36_

I don't understand the reference to `tuple[int, str]` here -- it seems to me that this representation can already represent that type just fine. The only one it can't represent is `tuple[int, ...]`. (We don't yet ever infer `tuple[int, str]`, e.g. from an annotation, but that just requires adding a bit of handling to `infer_annotation_expression`, it doesn't require any change to the code added in this PR.)

I also don't think this TODO belongs at this spot in the code. This code (for inferring a tuple type from a literal) is already fully correct, there's nothing more to be done here. If the TODO belongs anywhere it's in `types.rs`, in the list of `Type` variants, as a TODO that we don't yet have a representation for variable-length homogenous tuples (`tuple[T, ...]`)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4027 on 2024-09-10 00:36_

We can remove this todo; this test is correct after this PR, there's nothing left to do here.

---

_@carljm reviewed on 2024-09-10 00:40_

Awesome, thank you!

---

_@dhruvmanila reviewed on 2024-09-10 17:20_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:672 on 2024-09-10 17:20_

Yeah, make sense, thanks

---

_@dhruvmanila reviewed on 2024-09-10 17:20_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1562 on 2024-09-10 17:20_

That makes sense, thanks. I'll update the TODO and it's location

---

_Merged by @dhruvmanila on 2024-09-10 17:54_

---

_Closed by @dhruvmanila on 2024-09-10 17:54_

---

_Branch deleted on 2024-09-10 17:54_

---

_@carljm approved on 2024-09-10 17:58_

Looks great!!

---
