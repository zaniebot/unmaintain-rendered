```yaml
number: 13781
title: "[red-knot] Inference for comparison of union types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/comparison-union-types
created_at: 2024-10-16T18:39:31Z
updated_at: 2024-10-17T14:43:03Z
url: https://github.com/astral-sh/ruff/pull/13781
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Inference for comparison of union types

---

_Pull request opened by @sharkdp on 2024-10-16 18:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add type inference for comparisons involving union types. For example:
```py
one_or_two = 1 if flag else 2

reveal_type(one_or_two <= 2)  # revealed: Literal[True]
reveal_type(one_or_two <= 1)  # revealed: bool
reveal_type(one_or_two <= 0)  # revealed: Literal[False]
```

closes #13779

## Test Plan

See `resources/mdtest/comparison/unions.md`


---

_Label `red-knot` added by @sharkdp on 2024-10-16 18:39_

---

_Review requested from @carljm by @sharkdp on 2024-10-16 18:39_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-16 18:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-16 18:39_

---

_@sharkdp reviewed on 2024-10-16 18:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2937 on 2024-10-16 18:41_

I can remove this again if we prefer to return `Type::Todo` for now, to remind us that we *could* do better in some cases (and return `Literal[True]` or `Literal[False]` instead). But compared to the other operators, it can't be overloaded, so `bool` should always be correct(?).

---

_@sharkdp reviewed on 2024-10-16 18:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2725 on 2024-10-16 18:45_

I'm not 100% sure it's correct to filter out `None`s here. But it seems wrong to return `None` if there are valid `Some(…)` cases in the overall union of result types. But maybe we should return `None` if the resulting union is empty, i.e. return `None` instead of `Some(Type::Never)`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/unions.md`:63 on 2024-10-16 18:46_

Weird, I would have expected that we did this already, via the `is_subtype_of` checks in `UnionBuilder::add`. There must be a bug/oversight in there somewhere?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2725 on 2024-10-16 18:52_

A `None` return from `infer_binary_type_comparison` means "op not supported for these operands." If `infer_compare_expression` gets `None` it will emit a diagnostic and fall back to `Unknown` (if there was a comparison error we can't really say what the result should be.)

Using `filter_map` here silently excludes the `None` results from consideration, meaning we won't emit a diagnostic when we should, and we will provide an "answer" when we should fall back to `Unknown`.

I think instead we should short-circuit and return `None` if we see any `None`.

Probably we also want to change this at some point to return a `Result` instead of `Option`, both simply because it's more explicit, and because then the `Err` variant can propagate details of precisely which two "inner" types we failed to compare, in the recursive cases, allowing us to offer a better diagnostic.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2937 on 2024-10-16 18:55_

I'm fine with this change; we don't need `Todo` if we are returning a correct and "reasonably precise" (*waves hands*) result -- something that a user isn't likely to consider outright broken. I think this certainly qualifies.

---

_@carljm requested changes on 2024-10-16 18:55_

Tests look good! Requesting changes because I don't think we should `filter_map` out unsupported comparisons, and we should add a test for an unsupported-comparison-with-unions case.

---

_@sharkdp reviewed on 2024-10-16 18:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/unions.md`:63 on 2024-10-16 18:59_

Thought the same thing. I looked at the code briefly. I believe it might only work for `bool | Literal[True]` (when the newly added type is a literal), but not the other way around (when the literal is already in the union). I can look into it!

---

_@carljm reviewed on 2024-10-16 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2725 on 2024-10-16 19:00_

Oh, I submitted my review without seeing this comment.

I still think we should short-circuit and return `None` if any of the comparisons failed. If the comparison may raise `TypeError`, we need to emit a diagnostic.

I'm less settled on whether we have to fall back to `Unknown` (or `bool`) in this case. I think we _could_ still try to give a more precise best-effort answer, along with the diagnostic. But I don't think it's critical that we do that, and I think it will take some effort; we'd need to return a `Result` here instead of `Option`, and have the `Err` variant carry not only the two types that failed to compare, but also our best-effort conclusion from the "rest" of the comparison. (And I think comparing unions (and intersections?) may be the only case where we even have a best-effort conclusion?)

---

_@AlexWaygood reviewed on 2024-10-16 19:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2725 on 2024-10-16 19:04_

I would vote for falling back to `bool` if one of the elements of the union would lead to the comparison being invalid

---

_Comment by @github-actions[bot] on 2024-10-16 19:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Closed by @AlexWaygood on 2024-10-17 00:04_

---

_Reopened by @AlexWaygood on 2024-10-17 00:04_

---

_@sharkdp reviewed on 2024-10-17 07:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2732 on 2024-10-17 07:15_

Would be nice to avoid the duplication here, but I couldn't think of a good way to do this without introducing much more code.

I also looked for ways to replace the `for` loop with an iterator expression, but that would require something like [`Iterator::try_collect`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.try_collect), which is nightly-only. And would require an additional allocation.

---

_@sharkdp reviewed on 2024-10-17 07:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/unions.md`:63 on 2024-10-17 07:24_

No, the reason was simply that `BooleanLiteral[…] <: bool` was not implemented yet.

---

_@MichaReiser reviewed on 2024-10-17 08:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2732 on 2024-10-17 08:08_

I don't mind the repetition (or use of a for loop, I'm a huge fan of for loops) but maybe `Itertools::fold_options` is what you want

https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.fold_options

---

_@MichaReiser approved on 2024-10-17 08:08_

---

_@AlexWaygood reviewed on 2024-10-17 08:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2732 on 2024-10-17 08:31_

It will require refactoring this method (so shouldn't be done in this PR), but in the long term something like #13787 seems like a good idea. I also think we'll want to emit a separate diagnostic on each member of the union that would be invalid for this operation (or one diagnostic that mentions all possibly erroring Union members). So I'd also just leave this as it is for now

---

_@AlexWaygood approved on 2024-10-17 08:32_

This is great!

---

_@sharkdp reviewed on 2024-10-17 09:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2732 on 2024-10-17 09:01_

> maybe `Itertools::fold_options` is what you want
> 
> https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.fold_options

Hm, not quite. `fold_options` short-circuits on `None`s in the input iterator. But we want to short-circuit if the output of the `fold`-function returns `None`. So we would need something like [`Itertools::fold_while`](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.fold_while). That *works*, but it's completely ridiculous:
```rs
union
    .elements(self.db)
    .iter()
    .fold_while(Some(UnionBuilder::new(self.db)), |builder, element| {
        if let Some(ty) = self.infer_binary_type_comparison(other, op, *element) {
            FoldWhile::Continue(builder.map(|b| b.add(ty)))
        } else {
            FoldWhile::Done(None)
        }
    })
    .into_inner()
    .map(UnionBuilder::build)
```

I'll go with the `for` loop :smile: 

---

_Merged by @sharkdp on 2024-10-17 09:03_

---

_Closed by @sharkdp on 2024-10-17 09:03_

---

_Branch deleted on 2024-10-17 09:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:418 on 2024-10-17 14:43_

Nice catch, I really thought we had this already 🤦 

There are unit tests at the bottom of this module for `is_subtype_of`, ideally we'd add one for this case. (Though the union-builder tests are great, too, and do transitively test this.)

---

_@carljm reviewed on 2024-10-17 14:43_

---
