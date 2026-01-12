```yaml
number: 13749
title: "[red-knot] Support for not equal narrowing"
type: pull_request
state: merged
author: Lexxxzy
labels:
  - ty
assignees: []
merged: true
base: main
head: narrowing-noteq
created_at: 2024-10-14T12:01:53Z
updated_at: 2024-10-21T21:08:34Z
url: https://github.com/astral-sh/ruff/pull/13749
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Support for not equal narrowing

---

_@Lexxxzy_

### Summary

Planning to support type narrowing for `!=` expression as stated in #13694. 

###  Test Plan

Add tests in new md format.

---

_Review requested from @carljm by @Lexxxzy on 2024-10-14 12:01_

---

_Review requested from @MichaReiser by @Lexxxzy on 2024-10-14 12:01_

---

_Review requested from @AlexWaygood by @Lexxxzy on 2024-10-14 12:01_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-14 12:02_

---

_Comment by @Lexxxzy on 2024-10-14 12:09_

I have a question: Why isn’t there a case for (1 positive, 1 negative) in the match statement of InnerIntersectionBuilder::build? It seems like (1 & ~0) is essentially just 1. Asking because for this example:

```py
x = None if flag else 1
y = 0

if x != 1:
    y = x

    reveal_type(y) # revealed: None & ~Literal[1]

reveal_type(y) # revealed: Literal[0] | None & ~Literal[1]
```

 `y` in `if x != 1` scope should be just `None`. Also `y` should be `Literal[0] | None` outside of condition scope.

---

_Comment by @github-actions[bot] on 2024-10-14 12:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:14 on 2024-10-15 13:38_

With the new test framework, we can now write this in a shorter way, without using additional variables:
```suggestion
x = None if flag else 1

if x != None:
    reveal_type(x)  # revealed: Literal[1]

reveal_type(x)  # revealed: None | Literal[1]
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:158 on 2024-10-15 13:41_

This branch needs the same fix that I did for the `IsNot` branch here: https://github.com/astral-sh/ruff/pull/13758. If you re-apply this ` | ast::CmpOp::NotEq` to the current version on `main`, that should work automatically.

If we don't filter out non-singleton types, you would get a type of `list[int] & ~list[int] == Never` in a case like this:
```py
x = [1]
y = [1, 2]

if x != y:
    reveal_type(x)  # should still have type list[int]!
```

It might make sense to add an explicit test for this as well, even though we have one for `is not` already. Just in case we ever split the `IsNot`/`NotEq` branches for some reason.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:1 on 2024-10-15 13:41_

Can you please move this test to `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_equals.md` and adapt the structure to the what the other files in this folder have?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/conditionals.md`:5 on 2024-10-15 13:42_

```suggestion
### `x != None`
```

---

_@sharkdp requested changes on 2024-10-15 13:43_

Thank you very much for working on this. I added a few comments. Let me know if you need any help.

---

_Comment by @sharkdp on 2024-10-15 13:56_

> I have a question: Why isn’t there a case for (1 positive, 1 negative) in the match statement of InnerIntersectionBuilder::build?

There is a case for that. The `_` catch-all pattern. If there is a positive and a negative contribution, an intersection type needs to be built. 
```rs
match (self.positive.len(), self.negative.len()) {
    (0, 0) => Type::Never,
    (1, 0) => self.positive[0],
    _ => {
        self.positive.shrink_to_fit();
        self.negative.shrink_to_fit();
        Type::Intersection(IntersectionType::new(db, self.positive, self.negative))
    }
}
```

There's still the question on how to simplify those intersection types. For example, `A & ~A` could be simplified to `Never`. Maybe this is what you had in mind. I will try to look into this soon.

> It seems like (1 & ~0) is essentially just 1.

It is. This simplification is exactly what the `(1, 0)` branch does.



> `y` in `if x != 1` scope should be just `None`

If we ignore the problem that `Literal[1]` is not a singleton type (https://github.com/astral-sh/ruff/pull/13758), then yes. We would expect `None & ~Literal[1]` to be simplified to `None`. This is harder than the `A & ~A` case above. `A & ~B == A` is only true if `A` and `B` are disjoint.

---

_Comment by @sharkdp on 2024-10-21 14:04_

@Lexxxzy I hope that you don't mind that I followed up on this myself. There was an additional point I hadn't thought about in my initial review. There actually is a difference in the narrowing-logic, compared to the `not in` branch. When we see
```py
magic_number = 345
x = something()

if x is not magic_number:
    ...
```
we're not allowed to narrow the type by adding a `~Literal[345]` constraint (because there could be other Python objects that also have the type `Literal[345]`). However, if we have a `x != magic_number` constraint, we are allowed to exclude `Literal[345]`. Because all inhabitants of `Literal[345]` would result in the condition being false.

> `y` in `if x != 1` scope should be just `None`. Also `y` should be `Literal[0] | None` outside of condition scope.

With #13775, both of these simplifications now work just as you expected.

---

_@sharkdp reviewed on 2024-10-21 14:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:1 on 2024-10-21 14:21_

This test is not related to the new feature here, but it's something I wanted to add. Using `!=` narrowing, we can now construct "meaningful" large intersection types.

---

_@sharkdp reviewed on 2024-10-21 15:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:714 on 2024-10-21 15:00_

There are probably ways in which an intersection can be single-valued, but I did not invest time into it for now. I can add a TODO, if we think it's important.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:12 on 2024-10-21 18:42_

This display could be improved, but I don't think that's a priority unless we see it causing unreadable types in real-world code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:56 on 2024-10-21 18:48_

This test doesn't currently seem to make sense. The comparison here is against `Literal[0]`, which is a single-valued type, and we will narrow for `!= 0`, as shown just a couple tests above. This test is really just testing that `Literal[1]` and `Literal[0]` are disjoint, meaning the intersection `Literal[1] & ~Literal[0]` immediately simplifies to `Literal[1]`. So this test should either be removed, or re-titled to describe what it actually tests.

I do think we should add some tests here showing that we don't do wrong narrowings against non-single-valued types, particularly instance types. For example, `a = A(); b = B(); if a != b: ...` should not narrow to `A & ~B`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:714 on 2024-10-21 19:04_

I think an intersection would be single-valued if at least one type in its positive side is single-valued.

But I think this can only occur, given intersection simplification, if there is a single-valued type that is overlapping (not disjoint) with some other type, but neither is a subtype of the other. If they are disjoint, or if one is a subtype of the other, then they can't occur in the same intersection without one or both of them disappearing (unless they are disjoint and both on the negative side, which wouldn't make the intersection single-valued).

I have a hard time imagining that we'd ever introduce such a pair of types; I can't think of an example for any of our current single-valued types.

So I don't think this case is important :)

---

_@carljm approved on 2024-10-21 19:05_

Looks great, thank you! One comment on the tests that should probably be addressed before landing.

---

_@sharkdp reviewed on 2024-10-21 20:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:12 on 2024-10-21 20:41_

> This display could be improved

You mean `int & ~Literal[1, 2, 3]`? Or the general fact that we show intersection types to users? (which pyright and mypy don't seem to do?)

---

_@carljm reviewed on 2024-10-21 20:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:12 on 2024-10-21 20:43_

I meant condensed, yeah. I'm ok with showing intersection types to users. (Pyright and mypy will in some cases do so, but they format it like "subclass of A and B")

---

_@sharkdp reviewed on 2024-10-21 20:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:56 on 2024-10-21 20:45_

> This test doesn't currently seem to make sense

Absolutely. I just pushed the version which I had lying around locally already, but didn't commit+push because I got interrupted (sorry). I think it's more or less equivalent to what you proposed, but let me know if I should add something similar to `a = A(); b = B(); if a != b: ...`.

---

_@carljm reviewed on 2024-10-21 20:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:56 on 2024-10-21 20:49_

The test you pushed looks sufficient!

---

_Merged by @sharkdp on 2024-10-21 21:08_

---

_Closed by @sharkdp on 2024-10-21 21:08_

---
