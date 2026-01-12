```yaml
number: 13316
title: "[red-knot] Infer target types for unpacked tuple assignment"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpacked-tuple-assignment
created_at: 2024-09-10T18:45:46Z
updated_at: 2024-10-16T05:36:18Z
url: https://github.com/astral-sh/ruff/pull/13316
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Infer target types for unpacked tuple assignment

---

_@dhruvmanila_

## Summary

This PR adds support for unpacking tuple expression in an assignment statement where the target expression can be a tuple or a list (the allowed sequence targets).

The implementation introduces a new `infer_assignment_target` which can then be used for other targets like the ones in for loops as well. This delegates it to the `infer_definition`. The final implementation uses a recursive function that visits the target expression in source order and compares the variable node that corresponds to the definition. At the same time, it keeps track of where it is on the assignment value type.

The logic also accounts for the number of elements on both sides such that it matches even if there's a gap in between. For example, if there's a starred expression like `(a, *b, c) = (1, 2, 3)`, then the type of `a` will be `Literal[1]` and the type of `b` will be `Literal[2]`.

There are a couple of follow-ups that can be done:
* Use this logic for other target positions like `for` loop
* Add diagnostics for mis-match length between LHS and RHS

## Test Plan

Add various test cases using the new markdown test framework.
Validate that existing test cases pass.


---

_Label `red-knot` added by @dhruvmanila on 2024-09-10 18:45_

---

_Comment by @dhruvmanila on 2024-09-10 18:56_

There's some more work needs to be done, please don't look ;)

---

_Comment by @github-actions[bot] on 2024-09-10 22:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-09-11 19:01_

This is completely an incorrect solution, I quickly realize that this will require structural matching between the target expression and the value type. It would also be good to add caching here because we've definitions for each name node on the LHS and so we'd perform structural matching against the same target and value type for each name node that's present on the LHS.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:85 on 2024-10-14 14:33_

As mentioned, the implementation of `infer_starred_expression` adds this diagnostic. This should be fixed when implementing assignment to a starred expression.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:197 on 2024-10-14 14:39_

It's interesting that mypy doesn't support string unpacking (https://github.com/python/mypy/issues/13823) while Pyright can.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1251 on 2024-10-14 14:43_

This is basically doing something like combine `Literal[1], Literal[2]` into `int` and the assignment to starred expression should add the `list` to make it `list[int]`.

---

_@dhruvmanila reviewed on 2024-10-14 14:46_

---

_@dhruvmanila reviewed on 2024-10-14 14:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_workspace/tests/check.rs`:78 on 2024-10-14 14:48_

The reason this is required is because in a code like `(a, b) = (1, 2)`, the LHS expression is not available.

---

_Marked ready for review by @dhruvmanila on 2024-10-14 16:13_

---

_Review requested from @carljm by @dhruvmanila on 2024-10-14 16:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-10-14 16:13_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-10-14 16:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:65 on 2024-10-14 23:03_

I think it would be useful to have a TODO in this test that there should be a diagnostic here:
```suggestion
# TODO diagnostic
(a, b, c) = (1, 2)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:75 on 2024-10-14 23:08_

```suggestion
# TODO diagnostic
(a, b) = (1, 2, 3)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:85 on 2024-10-14 23:11_

But there actually should be a diagnostic here! Just not this one. (There should be one about not enough values to unpack. Can we make the TODO a bit clearer about that?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:80 on 2024-10-14 23:12_

In general I prefer to use explanatory paragraphs like this to offer context/explanation for the tested behavior in general, and put TODO comments inline right next to the line that isn't quite right yet.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:141 on 2024-10-14 23:14_

Fixing this will require creating a new Salsa tracked struct for unpacking assignments to ensure we just do the unpacking once, correct?

I don't think I realized in our previous conversations that this would not just be a performance issue, but also a matter of diagnostics correctness. I think this raises the priority on a follow-up PR to create an `Unpack` tracked struct so we can Salsa-cache the unpacking and do it just once. (There are other unpacking cases we'll need to handle -- for loops and comprehensions -- which is why this shouldn't have an assignment-specific name.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:197 on 2024-10-14 23:16_

Yeah, I don't like mypy's behavior here. This is IMO lint-rule territory, not something a type checker should complain about.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:295 on 2024-10-14 23:39_

I think `into_tuple_type` would match the other similar methods better? We have `into_class_type` etc.

I'm not sure which is better -- it seems like these methods are cheap and non-consuming, which suggests maybe `as_` is actually better? But either way I'd rather be consistent.

cc @MichaReiser @AlexWaygood 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1172 on 2024-10-14 23:50_

I think names and sequences might be the only kinds of assignment targets that we ever handle as Definitions. Attribute and subscript assignments are fundamentally different; we need to check their validity, and possibly narrow on them as well, but they don't define a symbol in this scope. So probably we can just get rid of these TODOs, and more explicitly label this case as "assignment that doesn't create a Definition." We just need to be sure our understanding of which assignments creates Definitions is the same both here and in semantic indexing.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:524 on 2024-10-14 23:53_

I think we can call this `Name` -- I think those are the only two kinds of assignment targets we'll ever create Definitions for.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:573 on 2024-10-14 23:55_

I think we can be more conservative here; if the target is a name, we can create an `AssignmentKind::Name`, if it's anything else, I think it's safe to not create a `current_assignment` at all; these must be either an attribute target, a subscript target, or malformed syntax, and I think our best option in any of those cases is to not create a Definition.

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:78 on 2024-10-15 00:05_

Since the RHS is a standalone expression whose type should be a cached Salsa query, I think we _could_ query the type of that expression one more time and set a type on the entire LHS expression. But I'm not sure there's any value to doing that; I'm happy with this change to our coverage goal.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1246 on 2024-10-15 00:28_

This doesn't look right to me; the `- 2` seems hardcoded to the case where `starred_index == 1`, and all the starred unpacking tests also have `starred_index == 1`. If I change one of the tests to have the starred element in a different position, inference of the element types from the end doesn't seem to work correctly.

If this variable is supposed to be the number of remaining elements in the unpacking target, it seems like that should be something like `elts.len() - (starred_index + 1)` instead?

And let's make sure we add a test for starred unpack where the starred element is not in second position. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1247 on 2024-10-15 00:31_

I don't think the name `end_index` is very clear here. Seems like it should be the index of the last element to be "gobbled" by the starred element; maybe `starred_end_index`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1248 on 2024-10-15 00:39_

If we calculate `remaining` correctly above, then I think `end_index` should be purely a function of `tuple_ty.len()` and `remaining`; in fact it seems like it should be `tuple_ty.len(builder.db) - remaining - 1`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1251 on 2024-10-15 00:41_

Somewhere this Todo gets swallowed and turned into Unknown. Not sure why, and it's not really a big deal, but if it's not hard to preserve it as a Todo that would be nice.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1261 on 2024-10-15 00:45_

This is handling the case where the starred element would be bound to nothing, i.e. match zero elements and turn into an empty list? Maybe add a comment with an example of what it is handling.

I wonder if this case could just be integrated into the above case; we should be able to detect that `starred_index..=end_index` is an empty range and just handle that as needed (or just handle an empty `starred_element_types`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1292 on 2024-10-15 00:51_

It seems like to assign Unknown correctly here (and in the future to correctly issue diagnostics for mismatched unpacking) we also need to account for starred element, if present?

Alternatively, we could just generalize the tuple RHS case above, so for a string RHS we get a slice of one-character StringLiteral types and otherwise all the logic is identical? That would avoid needing to implement the starred index handling twice.

I don't think it's necessary that we precisely infer literal string types from unpacking a literal string, only suggesting that in case it is just as easy as handling starred element explicitly here?

---

_@carljm requested changes on 2024-10-15 00:51_

This is great! A few issues, but seems quite close. Make sure for any bugs you fix in updating the PR, you also add a new test that would fail without that fix.

---

_@dhruvmanila reviewed on 2024-10-15 05:43_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:85 on 2024-10-15 05:43_

Yeah, I can make the TODO clearer here.

---

_@dhruvmanila reviewed on 2024-10-15 05:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_workspace/tests/check.rs`:78 on 2024-10-15 05:47_

Yeah, I agree. As you've mentioned, I'm also unsure of what the benefit could be of getting the type of the LHS expression which is either a tuple or list.

---

_@dhruvmanila reviewed on 2024-10-15 05:56_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:295 on 2024-10-15 05:56_

I think the reason those method names are using the `into_` prefix is because they consume `self` and return the relevant type while this method only returns a reference.

Also, this method isn't required now but was in an earlier version. Sorry for the churn, I'll remove this.

---

_@dhruvmanila reviewed on 2024-10-15 06:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1246 on 2024-10-15 06:04_

Yes, thanks for catching that, I totally did not intend that to happen.

---

_@dhruvmanila reviewed on 2024-10-15 06:16_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1261 on 2024-10-15 06:16_

> I wonder if this case could just be integrated into the above case; we should be able to detect that `starred_index..=end_index` is an empty range and just handle that as needed (or just handle an empty `starred_element_types`.)

Yeah, I think this is possible and should make it simpler. Thanks for the suggestion.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1251 on 2024-10-15 06:33_

I got it, it's because the `inner` function does not account for `Expr::Starred` and only checks for `Expr::Name`. So, it would return `None` which then defaults to `Unknown`.

So, the diff would become
```diff
                 ast::Expr::Name(name) if name == variable => {
                     return Some(value_ty);
                 }
+                ast::Expr::Starred(ast::ExprStarred { value, .. }) => {
+                    // TODO: Wrap the `value_ty` in a list type.
+                    return inner(builder, value, value_ty, variable);
+                }
                 ast::Expr::List(ast::ExprList { elts, .. })
                 | ast::Expr::Tuple(ast::ExprTuple { elts, .. }) => match value_ty {
                     Type::Tuple(tuple_ty) => {
                         let starred_index = elts.iter().position(ast::Expr::is_starred_expr);
```

---

_@dhruvmanila reviewed on 2024-10-15 06:33_

---

_@dhruvmanila reviewed on 2024-10-15 10:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1172 on 2024-10-15 10:26_

Yeah, I think that makes sense.

I'm just trying to understand for "assignment that doesn't create a Definition." do you mean that something like `a.b = 1` won't create a `Definition` or it won't create an `AssignmentDefinition`?

---

_@dhruvmanila reviewed on 2024-10-15 10:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:524 on 2024-10-15 10:26_

Yeah, that's correct. This helped me uncover https://github.com/astral-sh/ruff/issues/13759

---

_@dhruvmanila reviewed on 2024-10-15 11:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:141 on 2024-10-15 11:58_

Yeah, that's correct, we'd need a way to do unpacking once and cache it for future lookup. I can take this up as a follow-up at a high priority, will need to think about the required changes.

---

_@dhruvmanila reviewed on 2024-10-15 12:15_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1292 on 2024-10-15 12:15_

Yeah, I need to account for the starred element here as well but I think it might not be as complex as the previous logic if it's fine to infer the types as `LiteralString` instead of the precise `StringLiteral` with the correct character(s). So,

```py
(a, b, *c) = "a"
reveal_type(a)  # revealed: LiteralString
reveal_type(b)  # revealed: Unknown
reveal_type(c)  # revealed: Unknown

(a, b, *c) = "ab"
reveal_type(a)  # revealed: LiteralString
reveal_type(b)  # revealed: LiteralString
reveal_type(c)  # revealed: list[LiteralString]
```

---

_@AlexWaygood reviewed on 2024-10-15 12:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:295 on 2024-10-15 12:18_

Yes, the `into_*` naming convention is because it consumes `self` -- but because `Type` is `Copy`, consuming `self` and returning an owned version of the underlying data is actually more ergonomic (and probably no less performant, or at least not significantly so) for these methods than returning a reference to the underlying data

---

_@dhruvmanila reviewed on 2024-10-15 12:29_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1292 on 2024-10-15 12:29_

Hmm, it might be useful to generalize it. We can convert the `StringLiteral` type into a `TupleType` containing `n` number of either `LiteralString` or `StringLiteral` (with the corresponding character) where `n` is the length of the containing string.

---

_@dhruvmanila reviewed on 2024-10-15 12:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1292 on 2024-10-15 12:44_

I've addressed this feedback by splitting the `StringLiteral` type into a tuple type containing equal amount of `LiteralString` and delegating it back to the `inner` function for correct handling of starred expression.

---

_Comment by @dhruvmanila on 2024-10-15 12:48_

@carljm Thanks for the great review, I've addressed all of the feedback except for the duplicate diagnostic issue (https://github.com/astral-sh/ruff/pull/13316#discussion_r1800219731) which I'll fix as a follow-up.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:135 on 2024-10-15 12:51_

The new test case where the starred expression isn't at the same position as other test cases.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:218 on 2024-10-15 12:52_

These test cases are similar to that of the `Tuple` type above and it only differs such that we use strings and the correct revealed type.

---

_@dhruvmanila reviewed on 2024-10-15 12:52_

---

_@carljm reviewed on 2024-10-15 13:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1172 on 2024-10-15 13:59_

I mean that `a.b = 1` (and `a[b] = 1`) won't create a `Definition` at all, since it doesn't define any name in the local scope. Though ultimately it may create some other kind of struct that we track relative to control-flow, if we want to narrow types based on such assignments.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1172 on 2024-10-15 14:04_

It's not important that we do anything about this comment in this PR, the existing TODO is OK, we have more to figure out here regardless.

---

_@carljm reviewed on 2024-10-15 14:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1201 on 2024-10-15 14:11_

It looks like the only test exercising this case is the "Non-iterable unpacking" test case. But I think this case is also important if we unpack something that _is_ iterable, but is not a literal tuple or string. Can we add a test for that? We can define our own type that implements `__iter__` (the iteration tests have examples of that), since the stdlib types will be mostly generic, which we don't support yet.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1306 on 2024-10-15 14:17_

It occurs to me now that we probably need a case in here for handling an arbitrary unknown-length iterable type, where we just see what the type is from iterating it; similar to the case out in `infer_assignment_definition` that I commented on above. Because an arbitrary iterable type could occur inside a tuple we are unpacking.

We'd want to add a test for this, too, something like `a, (b, c), d = (1, MyIterableType(), 2)`.

And then if we have that case here, maybe we don't also need it out in `infer_assignment_definition`, instead we could just send all Sequence assignments into this function?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1231 on 2024-10-15 14:27_

I think we need to clarify what we expect `value_ty` to be in this case; right now I think it's inconsistent. If we are called from the full-unpack-with-starred case below, `value_ty` today will be `Todo` but in the future will already be a combined list type, so nothing further is needed here, we could just do this:

```suggestion
                    return inner(builder, value, value_ty, variable);
```

But the problem is that in the "too few elements to unpack, with starred" case below (line 1271), we are instead always passing in a single element type as `value_ty` here. I think we should make my suggested change here, and resolve that discrepancy below.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:189 on 2024-10-15 14:33_

So if we make some changes I suggest below, I think we would end up inferring this as `LiteralString`, because we should assume `*b` matches nothing when we have too few items to unpack.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1271 on 2024-10-15 14:34_

As mentioned above, we'll need to be consistent about what `value_ty` we pass in for a starred element; right now I think it's inconsistent between the above case and this case. (Today the inconsistency is `Todo` above vs "some single element type" here, in the future it will be "list type" above vs "some single element type" here.)

I think this case is the one that is wrong: if we have too few items to unpack, the most sensible "best effort" is to assume starred always matches nothing, and match up the rest of the available items accordingly. That will mean making this case slightly more complex, so we pass in `Todo` for the starred element (with TODO comment that in future it will be `list[Any]`) and match up the rest of the available elements, skipping the starred one.

We could delay this change until a future PR that actually adds the Starred support, but I feel the changes I'm suggesting will make the current behavior clearer and leave things in a less confusing state for the author of that future PR. What do you think?

---

_@carljm reviewed on 2024-10-15 14:35_

Changes look great! I noticed a few more things in reviewing just now, sorry I didn't catch these yesterday!

---

_@dhruvmanila reviewed on 2024-10-15 16:01_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1306 on 2024-10-15 16:01_

Yeah, that makes sense. I've updated the test cases to include this at both top level and nested place. This also removes the need to do it in `infer_assignment_definition`. Thanks!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:237 on 2024-10-15 16:05_

Creating `AstNodeRef`'s isn't expensive but it also isn't free because it requires bumping an `Arc` (and decrementing it later). 

It might be worth to keep storing the assignment only with an index for the right target. I'm not sure what `variable` is for.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1243 on 2024-10-15 16:07_

```suggestion
                                    &tuple_ty.elements(builder.db)[..starred_index],
```

Possibly, depending on what `elements` returns

---

_@MichaReiser reviewed on 2024-10-15 16:07_

---

_@dhruvmanila reviewed on 2024-10-15 16:17_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:237 on 2024-10-15 16:17_

> It might be worth to keep storing the assignment only with an index for the right target. I'm not sure what `variable` is for.

(lol, after writing a couple of sentences I realize which target you're referring to.)

Ah, yes, I think that can be done.

The `variable` is used to find the symbol the definition belongs to in case the LHS is a sequence. It is also used to add the "symbol -> type" mapping in `self.types.expressions` hashmap.

---

_@carljm reviewed on 2024-10-15 16:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:237 on 2024-10-15 16:19_

Using indices is quite tricky because of arbitrary nesting of unpacking; this isn't like imports where it's a flat list of items. It's possible but requires keeping a counter across the entire recursive traversal; I think the current code is a lot clearer.

I was unclear about `variable` at first also; I think it is needed for the unique per-Definition NodeKey. Perhaps `name` would be clearer than `variable` -- I think it is always a `Name` node.

---

_@dhruvmanila reviewed on 2024-10-15 16:29_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:237 on 2024-10-15 16:29_

> Using indices is quite tricky because of arbitrary nesting of unpacking; this isn't like imports where it's a flat list of items. It's possible but requires keeping a counter across the entire recursive traversal; I think the current code is a lot clearer.

I _think_ Micha is talking about the `targets` list in a multi-assignment statement like `x = y = 1` in which case we could just store the single `AstNodeRef<ast::StmtAssign>` and a `usize` index into the `targets` list. @MichaReiser can confirm though

> I was unclear about `variable` at first also; I think it is needed for the unique per-Definition NodeKey. Perhaps `name` would be clearer than `variable` -- I think it is always a `Name` node.

Yes, the `name` field already existed by the name of `target`, I mainly renamed it to variable to avoid any clash with the `targets` field in the node itself. I think `name` is more clear.

---

_@dhruvmanila reviewed on 2024-10-15 16:32_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1231 on 2024-10-15 16:32_

Yeah, I realized that while resolving your previous comment and playing around with some test cases. This makes sense, thanks.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:87 on 2024-10-15 16:47_

nit: would prefer it if we used modern syntax for type hints consistently

```suggestion
# TODO: Should be list[int] / list[Literal[2]] once support for assigning to starred expression is added
```

(the same applies to a bunch of other comments in this file ;)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:584 on 2024-10-15 16:52_

do we now need to set `self.current_assignment = None;` on completion of each iteration of this loop? It looks like the `current_assignment` could still be set to the assignment we dealt with in the previous iteration of the loop if the current iteration of the loop doesn't create an assignment

```suggestion
                    if let Some(kind) = kind {
                        self.current_assignment = Some(CurrentAssignment::Assign {
                            target,
                            value: &node.value,
                            kind,
                        });
                    }
                    self.visit_expr(target);
                    self.current_assignment = None;
                }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 17:38_

I think ideally, for something like this:

```py
a, (b,), c, d = "abcd"
```

We'd infer `a` as being `Literal["a"]`, `b` as `Literal["b"]`, `c` as `Literal["c"]` and `d` as `Literal["d"]`. (At the moment they're all `LiteralString`.)

---

_@AlexWaygood reviewed on 2024-10-15 17:42_

Nice -- this is complex stuff to get right!! A couple of small points on top of Micha's and Carl's comments:

---

_@dhruvmanila reviewed on 2024-10-15 17:51_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1271 on 2024-10-15 17:51_

We discussed this internally and agreed that this is simpler / easier compared to the other alternative where we'd need to convert the type corresponding to the starred element at two places.

---

_@dhruvmanila reviewed on 2024-10-15 18:03_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:584 on 2024-10-15 18:03_

Ah yes, good catch, thanks!

---

_@dhruvmanila reviewed on 2024-10-15 18:05_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 18:05_

Yeah, I think that depends on how precise we want the types. For this case, I think it might make sense to do so.

---

_@dhruvmanila reviewed on 2024-10-15 18:14_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 18:14_

I can do that as a follow-up.

---

_@AlexWaygood reviewed on 2024-10-15 18:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 18:16_

Sounds good!

---

_Review requested from @carljm by @dhruvmanila on 2024-10-15 18:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1197 on 2024-10-15 18:29_

```suggestion
        let target_ty = match kind {
            AssignmentKind::Sequence => self.infer_sequence_unpacking(target, value_ty, name),
            _ => value_ty,
        };
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 18:33_

I agree that we _can_ do this, I'm not sure we ever _need_ to do it (pyright doesn't do it, it will have some cost in complexity and perf), and I'm pretty sure there's no need to do it _now_. I would suggest we defer it until or unless we see real motivating use cases. Avoiding duplicate diagnostics, supporting unpacking in for loops, and handling starred elements correctly are all much more important follow-ups. (Though handling starred elements correctly is blocked on generic list type, I think.)

---

_@carljm approved on 2024-10-15 18:45_

---

_Comment by @carljm on 2024-10-15 18:47_

Given it's quite late now for @dhruvmanila , I'm going to rebase this, apply my one suggested change, and go ahead and merge, to reduce our number of outstanding PRs and thus potential conflicts!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 18:47_

Fine by me if we don't do it, that all makes sense! But in that case I think we should leave a comment saying that it's a deliberate choice to infer a less precise type -- it _looked_ like it could have been a mistake to me when I was reading through the diff :-)

---

_@AlexWaygood reviewed on 2024-10-15 18:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1288 on 2024-10-15 18:55_

Makes sense, I'll add such a comment!

---

_@carljm reviewed on 2024-10-15 18:55_

---

_Merged by @carljm on 2024-10-15 19:07_

---

_Closed by @carljm on 2024-10-15 19:07_

---

_Branch deleted on 2024-10-15 19:07_

---

_Comment by @dhruvmanila on 2024-10-16 05:36_

Thanks @carljm for the reviews and merging!

---
