```yaml
number: 15516
title: "[red-knot] Ensure differently ordered unions and intersections are considered equivalent"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: alex/union-equivalence
created_at: 2025-01-15T22:21:05Z
updated_at: 2025-05-07T15:23:18Z
url: https://github.com/astral-sh/ruff/pull/15516
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Ensure differently ordered unions and intersections are considered equivalent

---

_@AlexWaygood_

## Summary

Fixes astral-sh/ruff#15380

Currently, we don't understand that `str | int` and `int | str` are equivalent types; nor do we understand that `P & T ≡ T & P`. This PR fixes that, by ensuring that we never construct a `str | int` union -- `str | int` is simplified to `int | str` during constructions via a sorting algorithm introduced by this PR. (For instance types from the same module, specifically, the algorithm puts the instance types in alphabetical order.) This ensures that the trivial case of `int | str ≡ str | int` can be handled, well, trivially, with our existing logic in `Type::is_equivalent_to()`. An added benefit of this approach should be reduced memory usage, since we'll only ever store `int | str` in the Salsa `db`, never `str | int`.

Fixing astral-sh/ruff#15380 means that we can now promote the `subtype_of_is_antisymmetric` property test from flaky to stable 🎉

## Why not just implement `Ord` on `Type`?

It would be fairly easy to slap `#[derive(PartialOrd, Ord)]` on `Type`. However, this would order types according to their Salsa ID. While this would mean that types would always be consistently ordered in any single run of red-knot, the order in which they would appear might vary between different runs of red-knot. Unless we implemented an entirely different order for display purposes, this would make it difficult to write mdtests, and would also be quite confusing for users.

Moreover, it doesn't really "make sense" for `Type` to implement `Ord` in terms of the semantics. There are many different ways in which you could plausibly sort a list of types; the algorithm implemented in this PR is only one (somewhat arbitrary, at times) possible ordering.

## Costs of this approach

This approach does have several costs! Firstly, it makes the construction of unions and intersections more expensive. I think this tradeoff is worth it considering the reduced memory usage, and considering that `Type::is_equivalent_to()` does not need to be `O(n^2)` for unions and intersections with this change (at least for now). It's also possible that we could mitigate the performance penalty by maintaining the union/intersection elements in sorted order at all times while the union/intersection is being constructed. I haven't looked too hard at this yet, partly because I wanted to keep the diff clean and easy to understand. I also wasn't sure how high a performance cost this might be -- I'm interested to see what codspeed says here!

Another cost to this approach is that it might be confusing for users to see unions displayed in a different order to the order the user gave them in annotations. I don't see this as a significant cost, however, as we already perform many simplifications and rearrangements of unions and intersections.

Lastly, this doesn't solve all problems with `Type::is_equivalent_to()` that we'll ever encounter. When we implement protocols, for example, we'll need to understand `P` and `Q` in the following example as being equivalent types, and we'll need to understand `P | None` as being equivalent to `Q | None`, etc.

```python
from typing import Protocol

class P(Protocol):
    x: int

class Q(Protocol):
    x: int
```

But I think even when we get to these more advanced and trickier problems, having union and intersection elements guaranteed to always be in a predictable order should make answering the question of equivalence easier to do with a complexity less than `O(n^2)`.

## Implementation details

The algorithm for sorting union/intersection elements is placed in a new submodule, `crates/red_knot_python_semantic/src/types/type_ordering.rs`. Other than this algorithm, the only significant changes made are to add a few `.sort_by()` calls to `UnionBuilder::build()` and `InnerIntersectionBuilder::build()`.

## Test Plan

- Many existing mdtests updated
- New mdtests added
- One property test promoted to stable
- Two new property tests added. Both need to be marked as flaky for now, due at least in part to:
  - https://github.com/astral-sh/ty/issues/216
  - https://github.com/astral-sh/ruff/issues/15508
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable` to verify that the stable property tests all pass. I also ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::flaky` and examined the failures for the two added flaky property tests to check whether they looked related to the sorting algorithm; I couldn't find any that were.

---

_Label `red-knot` added by @AlexWaygood on 2025-01-15 22:21_

---

_Comment by @github-actions[bot] on 2025-01-15 22:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-01-15 22:41_

[1% regression on codspeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-equivalence). I'll wait for review comments before trying to optimise this

---

_Marked ready for review by @AlexWaygood on 2025-01-15 22:42_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-15 22:42_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-15 22:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-15 22:42_

---

_@MichaReiser reviewed on 2025-01-16 06:43_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 06:43_

I still think there's value in preserving the source order. E.g I would find this confusing as a user. The same in the example above. It's not the end of the world, but it is irritating. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:42 on 2025-01-16 06:45_

This branch and other branches where `left == right` seem unnecessary? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:296 on 2025-01-16 06:47_

Nit: I'd name those method `sequence_ordering` because they don't actually sort, they only return an ordering.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:301 on 2025-01-16 06:48_

Can we use `is_sorted_by`?

---

_@MichaReiser reviewed on 2025-01-16 06:52_

This is a great write up with a lot of context. Thank you. 

> this would make it difficult to write mdtests, 

I don't think this is true. The order is predictable for as long as no multi-threading is involved. 

> astly, this doesn't solve all problems with Type::is_equivalent_to() that we'll ever encounter. 

I'd like to understand this more. What makes `Protocols` different from what we have today? How would sorting the types help?

I do remain concerned about reordering types, especially if that ordering is non-obvious and, therefore, appears to be arbitrary for users. I often write my TypeScript types with a specific order in mind, e.g. group semantically relevant types first and I'd find it rather annoying if the extension removes that ordering when hovering over the type. 

I think this is different from making other simplifications because it sort of teaches me how I could have written my type instead, although I have slight reservations there as well. 



---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 10:02_

I agree. I wouldn't want something like `int | str | None` to be reordered to `int | None | str` in the output of the type checker (just an example with case-ignoring alphabetical order, not sure if that's the actual order we would choose here).

The idea in [this comment](https://github.com/astral-sh/ruff/issues/15380#issuecomment-2581108350) was that we could potentially keep a second list inside `Union` and `Intersection` structs that would keep track of this order — if sensible to do so. As in: we could always opt-out if we need to perform complex operations on unions/intersections, but we would try to preserve it on a best-effort basis.

I'm imagining something like this: When a user specifies the type `int | str | None` in an annotation, we would create an internal union type that conceptually looks like this:

```rs
Union {
  elements: [int, None, str],
  user_order: Some([0, 2, 1])
}
```

The idea would be that we could usually ignore this `user_order` field, but then try to reconstruct the original order in the rare case that we need to display a type to the user.

It might be fine as a first solution to switch to `user_order: None` as soon as we perform any sort of operation on the union type. But I think we could even try to do better by creating new `user_order`s in a limited set of operations. For example, when joining two unions `(A | B) | (C | D)`, which would both have `user_order: Some([0, 1])`, we could probably join these by adding `max(lhs.user_order) + 1` to all elements on the right hand side:

```rs
Union {
  elements: [A, B, C, D],
  user_order: Some([0, 1, 2, 3])
```

---

_@sharkdp reviewed on 2025-01-16 10:02_

---

_@AlexWaygood reviewed on 2025-01-16 10:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:42 on 2025-01-16 10:59_

Nice catch!

---

_@AlexWaygood reviewed on 2025-01-16 11:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:301 on 2025-01-16 11:06_

`Iterator::is_sorted_by` was stabilised in 1.82, and our MSRV is currently 1.80. But I think that would not be the correct method anyway, since `Iterator::is_sorted_by()` returns a `bool`, and we want a method here that returns an `Ordering`. The method we want is [`Iterator::cmp_by`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.cmp_by), I think, but it's not yet stabilised. If we had that method, we could just do this:

```diff
--- a/crates/red_knot_python_semantic/src/types/type_ordering.rs
+++ b/crates/red_knot_python_semantic/src/types/type_ordering.rs
@@ -289,8 +289,5 @@ fn order_sequences<'db>(
     right: impl IntoIterator<Item = &'db Type<'db>>,
 ) -> Ordering {
     left.into_iter()
-        .zip(right)
-        .map(|(left, right)| order_union_elements(db, left, right))
-        .find(|ordering| !ordering.is_eq())
-        .unwrap_or(Ordering::Equal)
+        .cmp_by(right, |left, right| order_union_elements(db, left, right))
 }
```

---

_Comment by @AlexWaygood on 2025-01-16 11:24_

> > this would make it difficult to write mdtests,
> 
> I don't think this is true. The order is predictable for as long as no multi-threading is involved.

Oh, interesting; I didn't think that through. And we don't use multi-threading in red-knot as invoked by `red_knot_test`?

I still think it's useful generally for the display of types to be stable between red-knot runs. Among plausible benefits to this, I can think of:
- Less user confusion
- Should help with persistent caching?
- Probably makes it easier to write integration CLI tests that assert the output of red-knot on the command line?

> > Lastly, this doesn't solve all problems with Type::is_equivalent_to() that we'll ever encounter.
> 
> I'd like to understand this more. What makes `Protocols` different from what we have today? How would sorting the types help?

Well, it didn't help that I forgot to actually write up the example in my initial PR description 🙃 I've edited it in now.

The difference with `Protocol`s is that they use [structural subtyping rather than nominal subtyping](https://docs.python.org/3/library/typing.html#nominal-vs-structural-subtyping). I.e. in the following example, `Unrelated` is not a subtype of `Nominal`, because it does not explicitly inherit from `Nominal`, and the type `Nominal` represents "all possible instances of the `Nominal` class at runtime". `Unrelated` _is_ a subtype of `Structural`, however, because one class does not have to explicitly inherit from a structural type in order for there to be a subtyping relationship between the corresponding types. The type `Structural` represents "all possible objects that have a mutable instance attribute `x` with a type that is assignable to `int`".

```py
from typing import Protocol

class Nominal:
    x: int

class Structural(Protocol):
    x: int

class Unrelated:
    x: int
```

Going back to the example I've edited into my PR description: the types `P` and `Q` are equivalent, because they both represent "all possible objects that have a mutable instance attribute `x` with a type that is assignable to `int`". But we can't "simplify" one type into the other type, which has been our approach for many of these equivalence issues, because otherwise you'd get things like this, which would be a truly terrible user experience -- especially if `Q` is defined in user code but `P` comes from a third-party library:

```py
def _(obj: Q):
    reveal_type(obj)  # revealed: P
```

So when we get to protocols (and `TypedDict`s, which are similar), it will absolutely be necessary to implement some more complex equivalence logic that is more complex and costly than just "eagerly simplify one type into its equivalent type at type construction time, and then check for equivalence using a simple equality comparison". Nonetheless, the reason why I say that this PR might help make even equivalence between protocol types easier to implement is because with this approach we _only_ need to do the complicated equivalence check between two protcol types or between two typeddict types -- and with this PR, we know exactly "where" in the list of union elements protocol types and typedict types will lie, relative to all other types that might be included in the union.

> I think this is different from making other simplifications because it sort of teaches me how I could have written my type instead, although I have slight reservations there as well.

I know what you mean, but our existing simplifications already frequently perturb the order of unions and intersections because of our use of `swap_remove` and swap_remove_index` in `UnionBuilder` and `InnerIntersectionBuilder`

---

_@AlexWaygood reviewed on 2025-01-16 11:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 11:36_

> The idea in [this comment](https://github.com/astral-sh/ruff/issues/15380#issuecomment-2581108350) was that we could potentially keep a second list inside `Union` and `Intersection` structs that would keep track of this order — if sensible to do so. As in: we could always opt-out if we need to perform complex operations on unions/intersections, but we would try to preserve it on a best-effort basis.

One issue I see with this approach -- that also exists in our current approach on `main` -- is that it means that our incremental type-checking is very sensitive to the exact source order of union elements. E.g. if somebody made this change in their source code

```diff
- x: None | int
+ x: int | None
```

then that might trigger a lot of otherwise-unnecessary recomputation. The type before the change would not compare equal with the type after the change, even though it represents the exact same set of union elements, because the type inferred for `x` would contain a record of the source-order of elements. It will also result in higher memory usage for us.

One idea I've had in the past is that types could have an `Option<(File, TextRange)>` field -- `Type::display` could just go back to the original source code and get the exact original representation of the type. This would be elegant in some ways because it also solves tricky problems such as this:

```py
type T = int | str | memoryview | Literal["foo", "bar", "baz", "spam"]

def f(x: T):
    reveal_type(x)  # ideally we'd say "Revealed type is `T`"
                    # rather than saying "Revealed type is `int | str | memoryview | Literal["foo", "bar", "baz", "spam"]`"
```

But carrying a `TextRange` attribute around would make our incremental type-checking even _more_ sensitive to trivia such as the exact amount of whitespace prior to the type definition, etc.

---

_Comment by @sharkdp on 2025-01-16 11:45_

> > I think this is different from making other simplifications because it sort of teaches me how I could have written my type instead, although I have slight reservations there as well.
> 
> I know what you mean, but our existing simplifications already frequently perturb the order of unions and intersections because of our use of `swap_remove` and swap_remove_index`in`UnionBuilder`and`InnerIntersectionBuilder`

I think Micha was trying to say that reorderings are fine *if* any simplifications are performed. But the default case will be that users write union-type annotations that can *not* be simplified, and maybe we should try a bit harder to make the UX in this case better. It's great that we can simplify `Literal[False] | str | Literal[True]` to `bool | str`, or `int | object | str` to `object`. But nobody will write that in reasonable user code (and if they do, they will probably be okay with being presented with the simplified version of their type).

(don't get me wrong, I am very much excited about this PR :+1:)

---

_Comment by @AlexWaygood on 2025-01-16 11:48_

> But nobody will write that in reasonable user code

I don't know about that. I've seen a lot of `object | None` and `int | bool` annotations in my time. And those aren't always a "mistake" -- `int | bool` is often done quite deliberately, even though folks know it will be simplified by the type checker, because people don't think of `bool` as being an `int` subtype even though they know that it is at runtime.

---

_Comment by @AlexWaygood on 2025-01-16 11:54_

> (don't get me wrong, I am very much excited about this PR 👍)

Thank you! And don't get me wrong -- I'm not wedded to the approach here! There are some really tricky tradeoffs to think through.

Even if we don't go with this approach, I don't consider this PR to be time wasted, because of the number of bugs I discoverered along the way:
- https://github.com/astral-sh/ty/issues/215
- https://github.com/astral-sh/ty/issues/216
- https://github.com/astral-sh/ruff/issues/15508
- https://github.com/astral-sh/ruff/pull/15511
- https://github.com/astral-sh/ruff/pull/15496
- https://github.com/astral-sh/ruff/pull/15475

---

_@MichaReiser reviewed on 2025-01-16 11:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 11:55_

> then that might trigger a lot of otherwise-unnecessary recomputation. The type before the change would not compare equal with the type after the change, even though it represents the exact same set of union elements, because the type inferred for x would contain a record of the source-order of elements. 

I'm not too concerned about this. We need to do well at dealing with this because it's no different from when the user makes an actual meaning full change.

> But carrying a TextRange attribute around would make our incremental type-checking even more sensitive to trivia such as the exact amount of whitespace prior to the type definition, etc.

I consider this a problem because it means any change will invalidate any public type in that module, unless we hide it behind a salsa interned but that has other costs. It also has the downside that types with a range are displayed differently from types without a range (e.g. it doesn't join `Literal` values etc).

Have we looked into how other type checkers (TypeScript, flow, Pyright, mypy) handle and represent union types? 

---

_@MichaReiser reviewed on 2025-01-16 11:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 11:57_

It seems `TypeScript` carries the origin type along 

https://github.com/microsoft/TypeScript/blob/4a18b5cf8d41975c86cde25a0b2229f81e3b08d2/src/compiler/types.ts#L6682-L6695

---

_@MichaReiser reviewed on 2025-01-16 11:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:301 on 2025-01-16 11:58_

Ah i see. I think there's still a possible bug in case `left` and `right` have a different length. In which case this method returns `Equal` (because `zip` truncates) when it shouldn't

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 12:02_

It also seems that preserving the original notation has advantages other than just to display a union 

https://github.com/microsoft/TypeScript/blob/4a18b5cf8d41975c86cde25a0b2229f81e3b08d2/src/compiler/checker.ts#L22445-L22458

---

_@MichaReiser reviewed on 2025-01-16 12:02_

---

_@AlexWaygood reviewed on 2025-01-16 12:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 12:03_

> > then that might trigger a lot of otherwise-unnecessary recomputation. The type before the change would not compare equal with the type after the change, even though it represents the exact same set of union elements, because the type inferred for x would contain a record of the source-order of elements.
> 
> I'm not too concerned about this. We need to do well at dealing with this because it's no different from when the user makes an actual meaning full change.
> 
> > But carrying a TextRange attribute around would make our incremental type-checking even more sensitive to trivia such as the exact amount of whitespace prior to the type definition, etc.
> 
> I consider this a problem because it means any change will invalidate any public type in that module, unless we hide it behind a salsa interned but that has other costs. It also has the downside that types with a range are displayed differently from types without a range (e.g. it doesn't join `Literal` values etc).

Fair enough. It sounds like maintaining a separate boxed array recording the source order would be the way to go here, then, like @sharkdp suggests.

> It also has the downside that types with a range are displayed differently from types without a range (e.g. it doesn't join `Literal` values etc).

Hmm, but I thought that this was what you were arguing we wanted -- I thought you were arguing that we _should_ display the type as the user presented it, without any simplifications, if the type comes directly from a user annotation.

> Have we looked into how other type checkers (TypeScript, flow, Pyright, mypy) handle and represent union types?

Mypy does not eagerly simplify unions at all; it only lazily simplifies them. This means that it has to do a lot of recomputation that for us is unnecessary. Pyright does some simplification (it deduplicates elements) but not nearly as much as we do (it doesn't remove a subtype from a union if the supertype is present). Even when it removes duplicate elements, it maintains order among the other elements (which we don't do because we use `swap_remove()`): https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFCMAmhwUwAFAB4BcmK%2BAD5QAzjBBRhqIVABGYMBUlQIhCLjjEkhAO7LgFMGRnTlFJGIDaYkAF1lTJAGMY18QBp%2BMe8JgBXBApCSw4ASltQnnooGIIScmpaQm5QoA.

---

_@AlexWaygood reviewed on 2025-01-16 12:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:301 on 2025-01-16 12:09_

I think there is no bug in this PR right now because all current uses of `order_sequences` do something like

```rs
left
    .len()
    .cmp(&right.len())
    .then_with(|| order_sequences(db, left, right))
```

However, I agree with you that this length-comparison logic should be part of the logic in this function; this would make the function more robust against possible future bugs. I'll change this 👍

---

_@AlexWaygood reviewed on 2025-01-16 12:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 12:10_

> It also seems that preserving the original notation has advantages other than just to display a union
> 
> [microsoft/TypeScript@`4a18b5c`/src/compiler/checker.ts#L22445-L22458](https://github.com/microsoft/TypeScript/blob/4a18b5cf8d41975c86cde25a0b2229f81e3b08d2/src/compiler/checker.ts#L22445-L22458)

How did you obtain that link?? When I click on it, GitHub just shows me this 😆

![image](https://github.com/user-attachments/assets/dc2af2d7-57d2-4fb7-8ba6-21eb147d660c)


---

_@MichaReiser reviewed on 2025-01-16 12:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 12:16_

I open it in VS code / Rust Rover and then do Ctrl+P Copy link

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:301 on 2025-01-16 12:28_

That refactor made the code quite a bit more elegant as well :-) Though it comes at the cost of slightly less short-circuiting for comparing intersections. https://github.com/astral-sh/ruff/pull/15516/commits/c37f8d57606f61bfc3f5d2f05d0a3b4451c13211

---

_@AlexWaygood reviewed on 2025-01-16 12:28_

---

_@carljm reviewed on 2025-01-16 15:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:108 on 2025-01-16 15:26_

Yeah, I think we probably should continue to make an effort to not reorder user-authored types that we haven't simplified (and that it's ok to reorder in simplifying.)

As far as _how_ we do this, I spent some time thinking about whether there could be solution overlap with the more general case of showing users a nice short type alias name instead of the complex type it aliases, which I think we will also want to do, as in Alex's example above. The proposal of carrying around a `TextRange` doesn't work, as discussed, because it is too sensitive to unrelated source-code changes. I'm not really coming up with any good unified solution here, though; I don't think we want every union/intersection type carrying around a string, for example. So it may be best to go with separate solutions here; a `user_order` field for unions and intersections, and something else (maybe something like a lookup table mapping types to their known aliases in a file?) for the type alias case.

---

_Comment by @carljm on 2025-01-16 15:33_

Thanks for this!

I do think it's likely that even with more complex equivalence cases, this can make equivalence a lot cheaper to implement, as long as our equivalent-types reliably end up sorted near each other.

(As a side note, I'm not sure it will ever be necessary, or even an improvement in practice, that we consider two different Protocol types such as `P` and `Q` to be equivalent, although technically they are. We will need to consider either one assignable to the other, but for example I don't know that we should simplify the union `P | Q` to either `P` or `Q`.)

---

_Comment by @carljm on 2025-01-16 15:37_

I guess in terms of next steps, if we are in agreement on the above discussion of user order preservation, the question would be whether we land this as-is and introduce user-order preservation as a follow-up, or add user-order preservation to this PR. I think probably the latter is better, as otherwise it'll be a lot more churn and re-churn to the mdtest output?

---

_Comment by @AlexWaygood on 2025-01-16 17:42_

> (As a side note, I'm not sure it will ever be necessary, or even an improvement in practice, that we consider two different Protocol types such as `P` and `Q` to be equivalent, although technically they are. We will need to consider either one assignable to the other, but for example I don't know that we should simplify the union `P | Q` to either `P` or `Q`.)

Right, though that would mean we'd have to abandon this property test if we decided not to recognise the equivalence there -- we'd have two types which would be mutual subtypes but would not be equivalent: https://github.com/astral-sh/ruff/blob/e2da33a45c7a4c76166bc0c11bd9c3f451f47caf/crates/red_knot_python_semantic/src/types/property_tests.rs#L360-L365

(This PR promotes that property test to stable)

---

_Comment by @carljm on 2025-01-16 17:49_

Yeah, after more discussion in Discord I think we would want to simplify `P | Q` to a non-union structural type.

---

_Comment by @carljm on 2025-01-16 19:28_

@AlexWaygood and I discussed today in 1:1 that deterministically sorting union/intersection elements and also tracking a `user_order` (which already loses us the property that `str | int` and `int | str` result in the same interned Salsa `UnionType`) is semantically indistinguishable from keeping the user-provided order just as we do today, and lazily producing the deterministic sorting of elements only when we actually need to compare two unions or intersections for equivalence (and possibly Salsa-memoizing that sorting, if that's a win in practice). The choice between the two approaches is a matter of performance, not correctness. I have half a suspicion that lazy sorting may turn out better, given that it means we won't have to do it for all unions. And I think it's probably simpler to implement as well. So I'd be inclined to try that first.

---

_Comment by @sharkdp on 2025-01-16 19:48_

> and lazily producing the deterministic sorting of elements only when we actually need to compare two unions or intersections for equivalence

Just one additional thought: The other scenario where the always-ordered canonical representation (proposed in this PR) would help would be during *construction* of unions and intersections, i.e. in the `{Union,Intersection}Builder`s. The construction of unions and intersections — the way it's currently implemented — is also $\mathcal{O}(n^2)$, because we have simplification procedures for every insertion that are themselves $\mathcal{O}(n)$. Constructing these aggregate types in ordered fashion could potentially make those simplification steps more efficient. For example: a search whether a particular *other* type is already part of a union/intersection could be reduced to $\mathcal{O}(\text{log}(n))$, making the whole construction potentially $\mathcal{O}(n\ \text{log}(n))$. I really don't know how much of a problem large unions/intersections are, so this might be misguided.

---

_Comment by @AlexWaygood on 2025-01-16 19:50_

> I really don't know how much of a problem large unions/intersections are, so this might be misguided.

Often quite a large problem! See https://github.com/astral-sh/ty/issues/240 for a summary of some of the many problems mypy and pyright have had with large unions of the years

---

_Comment by @carljm on 2025-01-16 19:52_

No, that's a good point. I suspect that we should probably aim for "simplest implementation that gets the semantics right and isn't obviously inefficient" initially, and tackle deeper optimization as its own project, once we are faced with real-world projects with large-union problems (or at least some projects we can use as benchmarks that make heavier use of unions than tomllib.)

---

_Comment by @MichaReiser on 2025-01-16 20:18_

We could also do the sorting lazily and cache the result in a separate field that's not part of eq using internal mutability 

---

_@AlexWaygood reviewed on 2025-01-17 11:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1136 on 2025-01-17 11:28_

I noticed that this branch was redundant, since we now eagerly normalize `type[object]` to `type`. But I could separate this out into another PR, if we prefer that

---

_@AlexWaygood reviewed on 2025-01-17 11:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:59 on 2025-01-17 11:36_

now that the ordering is no longer user-facing, I'm not sure that Salsa db lookups like this in the ordering algorithm make sense anymore. I think it's still useful to make sure that all the `StringLiteral` elements are next to each other in the elements list, but I don't think it matters anymore whether `Literal["bar"]` comes before `Literal["foo"]`. So maybe I should just sort by the Salsa IDs here, which would be cheaper?

---

_@MichaReiser reviewed on 2025-01-17 11:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:59 on 2025-01-17 11:37_

That makes sense to me

---

_@AlexWaygood reviewed on 2025-01-17 12:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:59 on 2025-01-17 12:02_

I implemented this in https://github.com/astral-sh/ruff/pull/15516/commits/64351be6cb95214c4f0ee7d52f28f0b32f92bc1a

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 12:14_

Did you measure the performance impact of caching the sorted unions? 

I'm asking because it's unclear to me if caching it always is worth it. E.g. sorting a union with only a few elements is super cheap. So maybe it's only worth calling into the cached variant after a certain threshold? 

It might also be worth to test first if the union elements aren't sorted already

---

_@AlexWaygood reviewed on 2025-01-17 12:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:1 on 2025-01-17 12:14_

This now _is_ basically equivalent to what we'd get if we just slapped `#[derive(PartialOrd, Ord)]` on `Type`, `KnownInstanceType`, `SubclassOfType`, `ClassLiteralType`, etc. So maybe we should just do that at this point -- it would certainly be less code. But it still doesn't make _sense_ semantically (to me) for them to implement `Ord`. There are many different ways you could plausibly order types; this is only one possible (arbitrary) ordering

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:29 on 2025-01-17 12:15_

Same as I'd not call this `sort_` I'd also not call this `order` because it doesn't reorder elements. I'd instead just call it `xy_ordering`

```suggestion
pub(super) fn union_elements_ordering<'db>(left: &Type<'db>, right: &Type<'db>) -> Ordering {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:100 on 2025-01-17 12:24_

I think implementing `PartialOrd` for `KnownInstance` should be fine. I agree that `Type` itself should not implement `Ord`. It would drastically reduce the code needed here (and I'd probably to the same for other types, e.g. `ClassBase)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:24 on 2025-01-17 12:25_

The salsa concern seems to be outdated

---

_@MichaReiser reviewed on 2025-01-17 12:26_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:1 on 2025-01-17 12:26_

You could also define your own trait `UnionElementOrd` and implement your own derive macro. But that seems painful too :D

---

_@MichaReiser reviewed on 2025-01-17 12:26_

---

_@AlexWaygood reviewed on 2025-01-17 12:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 12:29_

I pushed a commit shortly before your review to see what happens to the benchmarks if we don't cache them. Though I'm still worried about what will happen if we encounter very large unions, which do occur regularly in popular Python libraries such as pydantic (see https://github.com/astral-sh/ruff/issues/13549), even if they don't happen much in `tomllib`.

---

_@AlexWaygood reviewed on 2025-01-17 12:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 12:31_

The benchmarks do seem to be much improved without the caching (codspeed is [now neutral](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-equivalence)). So I guess it makes sense to keep these uncached for now until we benchmark how red-knot performs on projects with catastrophically huge unions.

---

_@AlexWaygood reviewed on 2025-01-17 12:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:100 on 2025-01-17 12:32_

> I think implementing `PartialOrd` for `KnownInstance` should be fine.

Hmm, why do you think it's okay for `KnownInstanceType` but not for `Type`? The two enums seem pretty similar to me in terms of whether it makes sense for them to implement `Ord`

---

_Label `great writeup` added by @sharkdp on 2025-01-17 12:38_

---

_@MichaReiser reviewed on 2025-01-17 12:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:100 on 2025-01-17 12:40_

I consider `Type` as the main public interface. I agree that probably the same argument applies as for `Type`. 

---

_@AlexWaygood reviewed on 2025-01-17 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 12:42_

> It might also be worth to test first if the union elements aren't sorted already

it [would be easier](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.is_sorted_by) if we upgraded our MSRV to 1.82 :P

---

_Renamed from "[red-knot] Ensure union elements and intersection elements always appear in sorted order" to "[red-knot] Ensure differently ordered unions and intersections are considered equivalent" by @AlexWaygood on 2025-01-17 12:45_

---

_@AlexWaygood reviewed on 2025-01-17 12:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:100 on 2025-01-17 12:49_

I kinda feel inclined to either add it to `Type` and all its wrapped enums/structs or none of them. Writing it all out like this is a lot of boilerplate code, but it's not _too_ hard to review it for correctness (it's all quite simple). But maybe it _would_ be better to just write a macro here...

---

_@sharkdp reviewed on 2025-01-17 12:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:45 on 2025-01-17 12:50_

Should we maybe write this using `P` and `Q`? And potentially do it for unions with three elements as well?
```suggestion
static_assert(is_equivalent_to(P | Q, Q | P))

static_assert(is_equivalent_to(P | Q | R, P | R | Q))
static_assert(is_equivalent_to(P | Q | R, R | P | Q))
static_assert(is_equivalent_to(P | Q | R, R | Q | P))
static_assert(is_equivalent_to(P | Q | R, Q | P | R))
static_assert(is_equivalent_to(P | Q | R, Q | R | P))
```

---

_@sharkdp reviewed on 2025-01-17 12:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:67 on 2025-01-17 12:53_

If I recall correctly, one of the original concerns with the ordering approach was that we would not recognize if two equivalent-but-not-`Eq` types like `type` and `type[object]` would be part of the union.

Can we add a test for this? Like
```py
static_assert(is_equivalent_to(type[object] | P | type, type | P | type[object]))
```
Or do we already simplify unions with `type` and `type[object]`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 12:53_

It shouldn't be too hard to implement ;)

---

_@MichaReiser reviewed on 2025-01-17 12:53_

---

_@sharkdp reviewed on 2025-01-17 12:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:67 on 2025-01-17 12:58_

No wait, that's not the right test. More like:

```py
static_assert(is_equivalent_to(type[object] | P, P | type))
```

If we don't recognize that yet, that's also okay. Might still be worth adding the test.

I imagined that we would design the ordering in such a way that those equivalence classes (w.r.t. `is_equivalent_to`) would be grouped together. Which would then make it easier to implement the `Union(…)` branch in `is_equivalent_to`.

---

_@AlexWaygood reviewed on 2025-01-17 13:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:67 on 2025-01-17 13:20_

Yeah. So far we've managed to get by here by ensuring that equivalent types are recognised as such by ensuring that only one of the two can ever appear in our internal model. So `type[object]` is eagerly simplified to `type` (meaning no special logic is necessary to implement the equivalence between the two), `bool & AlwaysTruthy` is eagerly simplified to `Literal[True]`, etc.. There are some known places where we don't currently do this, but where I'm hopeful that we can, e.g. https://github.com/astral-sh/ruff/issues/15528 and https://github.com/astral-sh/ruff/issues/15513

But there _will_ be types we encounter in the future where we _cannot_ do the eager simplification from one type to the other: `Protocol`s, `TypedDict`s, type aliases, there may be others.

So this is a good reason to keep the manual ordering rather than deriving `#[PartialOrd, Ord]` (links to the conversation I was having with @MichaReiser at https://github.com/astral-sh/ruff/pull/15516#discussion_r1920118053), because we can see and control exactly how the types are ordered. But it makes it hard to test right now, because we haven't got that far yet in our implementation of the type system!

---

_@MichaReiser reviewed on 2025-01-17 13:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:1 on 2025-01-17 13:23_

I'd probably wo with what you have now and you can create a (help wanted?) issue that adds a `ArbitraryOrd` (or similar) trait with a derive macro. Seems like a fun exercise for someone interested in Rust.

---

_@AlexWaygood reviewed on 2025-01-17 14:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 14:23_

done in https://github.com/astral-sh/ruff/pull/15516/commits/1e4769339fb2e54eff016ab304f2361aaaa69528 :-)

---

_@MichaReiser reviewed on 2025-01-17 14:29_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:67 on 2025-01-17 14:29_

I don't think one necessarily precludes the other. We can automatically derive ordering for some types but manually implement ord where necessary (or our custom ordering trait)

---

_Comment by @AlexWaygood on 2025-01-17 19:15_

I _think_ this should be ready for another round of review now!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:411 on 2025-01-17 21:55_

The linked issue is closed. Was it closed prematurely, or is it not in fact the cause of this property test being flaky, or is this property test not in fact flaky?

Or did you mean #15528 here?

---

_@carljm approved on 2025-01-17 22:49_

This looks good to me! I'm really not too concerned about custom trait vs macro vs explicit match; these are surface code organization issues that are easy to change later if we decide we don't like what we have.

---

_@AlexWaygood reviewed on 2025-01-17 23:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-17 23:03_

The version where we first check to see whether it is already sorted before attempting to sort it does (I think) seem to be doing _slightly_ worse on the benchmarks? But it's all within the noise range; I might be looking for signal here in something that's actually random variation.

---

_@AlexWaygood reviewed on 2025-01-18 11:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:411 on 2025-01-18 11:50_

I think #15508 used to cause this property test to be flaky, and now #15513 causes this property test to be flaky. We need at least #15508 and #15513 and this PR (and possibly more fixes on top) for this property test to pass.

I realised that with the latest version of the PR, however, we can promote `gradual_equivalent_to_is_symmetric` to stable! I ran it for 2,000,000 iterations without any failures.

---

_@AlexWaygood reviewed on 2025-01-18 11:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:411 on 2025-01-18 11:52_

> and now #15513 causes this property test to be flaky

e.g. it's given me this as a failing example, which looks pretty clearly #15513-related

```
---- types::property_tests::flaky::intersection_equivalence_not_order_dependent stdout ----
thread 'types::property_tests::flaky::intersection_equivalence_not_order_dependent' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-6f17d22bba15001f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (KnownClassInstance(Bool), AlwaysFalsy, Intersection { pos: [], neg: [BooleanLiteral(false)] })
```

---

_@MichaReiser reviewed on 2025-01-18 14:03_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4215 on 2025-01-18 14:03_

I just noticed that checking `is_sorted` first is probably wasteful because rustc hash function aims to be `O(n)` when the array is already sorted. 

I also think that we should probably leave those optimizations for later and only apply them based on the size of the union (in which case we may also decide to cache). 

Sorry that I misguided you here. 

---

_@MichaReiser approved on 2025-01-18 14:04_

I'd revert the `is_sorted` check and go with the most simplistic version and it probably is slower (see my inline comment). I still think it might be nice to have a macro that generates the implementations for a custom trait. Maybe something we can create an issue for?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1171 on 2025-01-18 19:00_

Can we move the `len` checks before the `to_sorted_union` (same for intersections).  Or did you intentionally decide against this optimization because of the case mentioned in the TODO (although I'm not sure if we need to sort to detect that case?)

---

_@MichaReiser reviewed on 2025-01-18 19:00_

---

_Comment by @AlexWaygood on 2025-01-19 16:10_

[Final codspeed report](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-equivalence): a 1% _speedup_ on the incremental benchmark, I assume due to fewer Salsa lookups 🥳

---

_Merged by @AlexWaygood on 2025-01-19 16:10_

---

_Closed by @AlexWaygood on 2025-01-19 16:10_

---

_Branch deleted on 2025-01-19 16:10_

---
