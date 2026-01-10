```yaml
number: 15374
title: "[red-knot] Move `UnionBuilder` tests to Markdown"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/union-type-tests
created_at: 2025-01-09T13:34:51Z
updated_at: 2025-01-09T20:45:08Z
url: https://github.com/astral-sh/ruff/pull/15374
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Move `UnionBuilder` tests to Markdown

---

_Pull request opened by @sharkdp on 2025-01-09 13:34_

## Summary

This moves almost all of our existing `UnionBuilder` tests to a Markdown-based test suite.

I see how this could be a more controversial change, since these tests where written specifically for `UnionBuilder`, and by creating the union types using Python type expressions, we add an additional layer on top (parsing and inference of these expressions) that moves these tests away from clean unit tests more in the direction of integration tests. Also, there are probably a few implementation details of `UnionBuilder` hidden in the test assertions (e.g. order of union elements after simplifications).

That said, I think we would like to see all those properties that are being tested here from *any* implementation of union types. And the Markdown tests come with the usual advantages:

- More consice
- Better readability
- No re-compiliation when working on tests
- Easier to add additional explanations and structure to the test suite

This changeset adds a few additional tests, but keeps the logic of the existing tests except for a few minor modifications for consistency.

---

_Label `red-knot` added by @sharkdp on 2025-01-09 13:34_

---

_Review requested from @carljm by @sharkdp on 2025-01-09 13:34_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-09 13:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-09 13:34_

---

_Comment by @github-actions[bot] on 2025-01-09 13:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:23 on 2025-01-09 13:43_

```suggestion
## `Never` is removed

`Never` is an empty set, a type with no inhabitants. Its presence in a union is always redundant, and so we eagerly simplify it away:
```

---

_@sharkdp reviewed on 2025-01-09 13:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:68 on 2025-01-09 13:44_

These tests would be more readable if we had something like C++'s `std::declval<T>()` (https://github.com/astral-sh/ruff/issues/13789) which pretends to create a value of type `T`. Using `knot_extensions`, we could now easily implement a function like the following (using PEP 747 language):
```py
def value_of[T](typ: TypeForm[T]) -> T: ...
```
that would allow us to write these tests in a single line without having to introduce dummy parameters and without having the definitions a few lines away from the assertions:

```suggestion
reveal_type(value_of(str | LiteralString))  # revealed: str
reveal_type(value_of(LiteralString | str))  # revealed: str
reveal_type(value_of(Literal["a"] | str | LiteralString))  # revealed: str
reveal_type(value_of(str | bytes | LiteralString))  # revealed: str | bytes
```
A disadvantage of this approach would be that it makes more of these tests dependent on `knot_extensions`. @AlexWaygood is still "a little sceptical" about this idea, so I kept the usual strategy of using function parameters.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:26 on 2025-01-09 13:44_

is it worth also including an example using `typing.NoReturn`...?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:50 on 2025-01-09 13:45_

```suggestion
The type `S | T` can be simplified to `T` if `S` is a subtype of `T`:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:66 on 2025-01-09 13:46_

```suggestion
The union `Literal[True] | Literal[False]` is exactly equivalent to `bool`:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:76 on 2025-01-09 13:47_

```suggestion
    u1: Literal[True, False],
    u2: bool | Literal[True],
    u3: Literal[True] | bool,
    u4: Literal[True] | Literal[True, 17],
    u5: Literal[True, False, True, 17],
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:102 on 2025-01-09 13:48_

```suggestion
## Collapse multiple `Unknown`s

Since `Unknown` is a gradual type, it is not a subtype of anything,
but multiple `Unknown`s in a union are still redundant:
```

---

_@AlexWaygood approved on 2025-01-09 13:49_

Nice. I think this is definitely worthwhile

---

_Label `testing` added by @AlexWaygood on 2025-01-09 13:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:68 on 2025-01-09 16:42_

Interestingly, `typing.cast` (with a dummy second argument), is effectively the `value_of` function. I wouldn't be opposed to adding `value_of` as a variant that doesn't require the dummy second argument, at the same time we add `typing.cast`. But I don't feel strongly either way, I think the function arguments are reasonable too.

---

_@carljm approved on 2025-01-09 16:49_

I haven't reviewed in detail, happy to let Alex do that for efficiency's sake. Just clarifying that I'm on board with moving these tests to mdtest, I think the pros outweigh the cons. I think almost any test where we have to use the `Ty` enum and construct arbitrary types, is probably better as an mdtest.

---

_@T-256 reviewed on 2025-01-09 17:29_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:68 on 2025-01-09 17:29_

> A disadvantage of this approach would be that it makes more of these tests dependent on `knot_extensions`.

Also, could consider `reveal_type_of_value(...)` function which has same logic of `reveal_type` that doesn't need to explicitly import?

---

_@T-256 reviewed on 2025-01-09 17:32_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:92 on 2025-01-09 17:32_

```suggestion
    reveal_type(u1)  # revealed: Unknown | str
    reveal_type(u2)  # revealed: str | Unknown
```
consistent with other examples.

---

_@sharkdp reviewed on 2025-01-09 18:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:68 on 2025-01-09 18:46_

> Also, could consider `reveal_type_of_value(...)` function which has same logic of `reveal_type` that doesn't need to explicitly import?

More like `reveal_type_of_type_expression` or something like that, but yes — that's a good idea! We could take it one step further and implement the analog to `assert_type` on that level. Basically `assert_equal_type(type1, type2)`.

And now that I write it out, I realize that we sort-of already have this in `knot_extensions`. We can write
```py
static_assert(is_equivalent_to(str | LiteralString, str))
```
Now that only works for fully static types, but once we have `is_gradual_equivalent_to`, that would be a (slightly more verbose) alternative to `assert_equal_type`:
```py
static_assert(is_gradual_equivalent_to(Unknown | Unknown | str, Unknown | str))
```

---

_@sharkdp reviewed on 2025-01-09 18:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/union_types.md`:68 on 2025-01-09 18:55_

> Interestingly, `typing.cast` (with a dummy second argument), is effectively the `value_of` function.

*"It's like `typing.cast`, just without having a value in the first place!"* is exactly how I pitched it to Alex, and he *still* didn't want it :smile: 

---

_Merged by @sharkdp on 2025-01-09 20:45_

---

_Closed by @sharkdp on 2025-01-09 20:45_

---

_Branch deleted on 2025-01-09 20:45_

---
