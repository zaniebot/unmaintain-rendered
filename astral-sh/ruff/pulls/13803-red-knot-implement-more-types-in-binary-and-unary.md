```yaml
number: 13803
title: "[red-knot] Implement more types in binary and unary expressions"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: unary-op
created_at: 2024-10-17T22:30:05Z
updated_at: 2024-10-20T07:51:52Z
url: https://github.com/astral-sh/ruff/pull/13803
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Implement more types in binary and unary expressions

---

_@Glyphack_

Implemented some points from https://github.com/astral-sh/ty/issues/244

- Handle Unknown and Any in Unary operation
- Handle Boolean in binary operations
- Handle instances in unary operation
- Consider division by False to be division by zero

I thinks it's easier to review these multiple small things in one go. Let me know if it's not to split for next ones.

---

_Review requested from @carljm by @Glyphack on 2024-10-17 22:30_

---

_Review requested from @MichaReiser by @Glyphack on 2024-10-17 22:30_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-10-17 22:30_

---

_Comment by @github-actions[bot] on 2024-10-17 23:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:24 on 2024-10-18 01:16_

nit: this pattern of creating many variables is a legacy holdover from our previous testing infrastructure, which was more limited in the assertions it could make. For cases like this with small expressions and small types, it's more readable to just do e.g. `reveal_type(a + b)  # revealed: int` all in one line. (In fact, a PR changing existing tests to use this style would also be welcome)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:24 on 2024-10-18 01:19_

I think we can be more precise in all of these cases (except for division, since we don't have float literals). For example, we know that `True + True == 2`, so that can be `Literal[2]` instead of `int`. In fact I think the best implementation strategy for all these cases (except for `|`, where we can return a `BooleanLiteral`) is to implement the operator for `IntLiteral` and then just have binary operations on `BooleanLiteral` call back in to the same operation, but with `BooleanLiteral[True]` converted to `Literal[1]` and `BooleanLiteral[False]` converted to `Literal[0]`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:7 on 2024-10-18 01:22_

Especially if we go for a bit more precision, as I suggest below, it would be useful to also test with `True` and `False`, or `False` and `False`. We don't need the full matrix, but at least some different variants for different operators. And maybe all four possible combinations for `|`, since that one is special for bools.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/instance.md`:4 on 2024-10-18 01:24_

This test is great! Can we also add a test for a class that implements none of these dunders, and assert that we get an "unsupported operation" diagnostic from each operator? (Unless you decide not to implement that in this PR.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:40 on 2024-10-18 01:26_

This tests dividing by `False`, which is good, but you also added a line of code below which supports issuing division by zero diagnostic when dividing a bool by zero (i.e. the dividend is a `BooleanLiteral`). Can we add a test for that, too?

And actually also a test for where the dividend is just `bool()`, too!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:545 on 2024-10-18 01:27_

We can also add `KnownClass::Bool` here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2487 on 2024-10-18 01:31_

If we get a not-callable outcome here, we should emit an unsupported-operation diagnostic (in the draft PR https://github.com/astral-sh/ruff/pull/13800 we are currently proposing to use the code `operation` for that diagnostic, though I'm kinda thinking I might prefer the more explicit `unsupported-operation` -- @AlexWaygood ?).

If you'd rather not implement that in this PR, that's fine, but please add a TODO comment for it here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2532 on 2024-10-18 01:37_

I wouldn't include `BooleanLiteral` here explicitly. (If you did, it should also be included for left operand, as well as right operand, so as to handle e.g. `True / 2`.) But I'd instead allow this to be handled by the more generic recursive-call approach described below.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2617 on 2024-10-18 01:41_

`BitOr` of two `BooleanLiteral` needs special handling here; I'd give it its own case, and aim to return the correct `BooleanLiteral` for all four cases (should be easy to do as one line by just ORing the internal rust bools from each operand).

For everything else, I'd just have patterns for `(left, BooleanLiteral(b), op)` and for `(BooleanLiteral(b), right, op)` where all those cases do is construct the correct 1-or-0 `IntLiteral` for the `BooleanLiteral` (see above where that's already done in `infer_unary_expression`) and then re-run this match using that int literal in place of the boolean literal.

This will require extracting an `infer_binary_expression_type` function, which takes `left_ty: Type, right_ty: Type, op` arguments and just contains the match statement, so that you can recursively call it with the new types. You can see where we already did that in https://github.com/astral-sh/ruff/pull/13800, but it's fine to do it here too, one can easily rebase on the other.

---

_Review comment by @carljm on `crates/ruff_python_ast/src/nodes.rs`:3003 on 2024-10-18 01:48_

Hmm, the other three look good here, but this one can be backwards. Someone can write a class C with `def __bool__(self) -> Literal[True]: return True`, and now we'll think that `not C()` is `Literal[True]`, when it should be `Literal[False]`. (Let's add a test for this case!)

I think unfortunately this means that using a direct-translation `dunder` method here is probably not the right approach for unary ops, and instead this needs to be handled explicitly with a `match` over the op kinds in type inference, so that we can flip the sense for `not`.

---

_@carljm requested changes on 2024-10-18 01:49_

Thank you, this is great! A few comments:

---

_Label `red-knot` added by @MichaReiser on 2024-10-18 06:20_

---

_@sharkdp reviewed on 2024-10-18 08:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:24 on 2024-10-18 08:02_

> a PR changing existing tests to use this style would also be welcome

I had already started with this, so here it is: https://github.com/astral-sh/ruff/pull/13806

---

_@Glyphack reviewed on 2024-10-18 14:52_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/binary/booleans.md`:7 on 2024-10-18 14:52_

I added more tests covering those cases.

---

_@AlexWaygood reviewed on 2024-10-18 15:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2487 on 2024-10-18 15:47_

> in the draft PR https://github.com/astral-sh/ruff/pull/13800 we are currently proposing to use the code `operation` for that diagnostic, though I'm kinda thinking I might prefer the more explicit `unsupported-operation` -- @AlexWaygood ?

We actually use the `operator` code in #13800. I don't mind using the `unsupported-` prefix, but I would much prefer to use the operat-_or_ suffix rather than operat-_ion_. To some extent any type checker diagnostic comes about as a result of an "unsupported operation"; but this error code specifically deals with attempts to use an operat-_or_ in an unsupported way.

TL;DR: `unsupported-operator`?

---

_@carljm reviewed on 2024-10-18 16:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2487 on 2024-10-18 16:20_

Oops, my mistake; yeah `unsupported-operator` sounds good to me!

---

_@Glyphack reviewed on 2024-10-19 10:22_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2617 on 2024-10-19 10:22_

Thanks this definitely works better than what I did.

Just one note, I was thinking to also move the zero division check to the recursive call so if a non trivial zero division is covered in the recursions it's also reported but I found that cases like `True / False` will become `Cannot divide object of type `Literal[1]` by zero` so I left it in the parent function.

It makes sense because we can know anything that will become zero  error. But I guess another way to address your previous comment https://github.com/astral-sh/ruff/pull/13803#discussion_r1805699455 would be to do the recursive check but when we print the error message we get the type again from boolean expression.
I did this and found that there is a type map of previously calculated types. So I used the `types.expressions` here.
Let me know if there's a better way to do it here.

---

_@Glyphack reviewed on 2024-10-19 11:14_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/src/nodes.rs`:3003 on 2024-10-19 11:14_

Thanks. I modified the code to to match other unary ops for instance types. We can also apply your previous suggestion here (match op and instance type and after getting the call type call the function again with return type and op) but I decided to let the Unary::Not case to handle all the Not operators to make it simpler to read.

So not operator will not work on instances now because it's not fully implemented but I will implement that part in a separate pr:
https://github.com/Glyphack/ruff/blob/de190b820662478e5cdf53f16f4d48ade8721071/crates/red_knot_python_semantic/src/types.rs#L617

---

_@Glyphack reviewed on 2024-10-19 11:31_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2487 on 2024-10-19 11:31_

Thanks do you want to create functions to create the error with the right prefix in the future? I just put the string in the error message now.

---

_Review requested from @carljm by @Glyphack on 2024-10-19 11:32_

---

_@AlexWaygood reviewed on 2024-10-19 16:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2487 on 2024-10-19 16:25_

At some point we'll make our diagnostics less "stringly typed", and we'll probably do an audit of all our error codes at around the same time. For now I wouldn't much worry about :-)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2490 on 2024-10-19 17:19_

Minor nit: since `+` and `-` are both unary and binary operators, let's make sure this diagnostic is clear which one we're talking about:
```suggestion
                            "Unary operator `{op}` is unsupported for type `{}`",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2485 on 2024-10-19 17:25_

There is also the possibility of `CallOutcome::Union` where some elements of the union were not callable, in which case we should also emit this diagnostic. The way to handle this is to match on `call.return_ty_result(db, unary, self)`; if we get `Ok(return_ty)` we just return that and there is no diagnostic, if we get any `NotCallableError::Err` variant, we do emit the unsupported-operator diagnostic, and return the `return_ty` field of the `Err` variant.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2531 on 2024-10-19 17:28_

I would prefer to leave this check out in `infer_binary_expression` and not re-check it recursively, and then we don't have to bother with `original_type` and this long comment. The only cost is that we have to explicitly check for `BooleanLiteral(False)` in the divisor, but that seems totally fine. It doesn't seem worth the complexity of adding `original_type` and needing this long comment, and the efficiency cost of double-checking for division by zero, just so we can match on `IntLiteral(0)` instead of `IntLiteral(0) | BooleanLiteral(False)`.

(It would be different if there were lots of different equivalent-to-zero values we might be converting to in a recursive call, but there aren't -- there's exactly one.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2592 on 2024-10-19 17:34_

Let's add a few mdtests for `Pow` operator on int literals in `binary/integers.md`? (One for each possible case here: ok literal result, too large `m`, too large result.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2652 on 2024-10-19 17:35_

This can be simplified:
```suggestion
            ) => Type::BooleanLiteral(b1 | b2),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2656 on 2024-10-19 17:38_

Let's do it this way instead, like it's already done in `infer_unary_expression`:
```suggestion
            (Type::BooleanLiteral(bool_value), right, op) => {
                self.infer_binary_expression_type(expr, op, Type::IntLiteral(i64::from(bool_value)), right)
```
And same for the case below.

Bool and int literals aren't equivalent; the conversion between them should be explicit, it shouldn't be done implicitly by `expect_int_literal`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:364 on 2024-10-19 17:40_

We should not add this; boolean literals are not equivalent to int literals, the conversion should not be implicit. See below comment for how we can handle the conversion explicitly without doing this.

---

_Review comment by @carljm on `crates/ruff_python_ast/src/nodes.rs`:3003 on 2024-10-19 17:46_

Sounds good! I think the results look fine now for this PR (it's fine for handling boolean value of instances to be a separate PR), but I still think we need to remove this `dunder()` method entirely. It's simply not correct that the unary Not operator is equivalent to the `__bool__` dunder, so leaving this method in place with that equivalence in it is asking for future bugs. Instead we should handle matching the `op` and getting the right dunder method for each op out in `infer_unary_expression`.

---

_@carljm requested changes on 2024-10-19 17:47_

Thanks for the updates! This now needs to be rebased now that #13800 has landed; there are some conflicts (should be minor.) And some comments below.

---

_@carljm reviewed on 2024-10-19 18:07_

---

_Review comment by @carljm on `crates/ruff_python_ast/src/nodes.rs`:3003 on 2024-10-19 18:07_

Or if you want to leave this method in place, it would have to return an `Option` and return `None` for unary `Not` operator. But at that point I'm not sure the ergonomics justify having the method at all, especially since I don't expect there to be multiple call sites where we need this.

---

_@Glyphack reviewed on 2024-10-19 18:18_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/src/nodes.rs`:3003 on 2024-10-19 18:18_

No I'm removing it I agree with you.

---

_@AlexWaygood reviewed on 2024-10-19 19:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2483 on 2024-10-19 19:06_

I think this should be `unreachable!` rather than `debug_assert!`

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:2483 on 2024-10-19 19:13_

Oh I didn't know about this thanks.

---

_@Glyphack reviewed on 2024-10-19 19:13_

---

_@AlexWaygood reviewed on 2024-10-19 19:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2483 on 2024-10-19 19:16_

Micro-nit (sorry!!): "it's" is short for "it is"; this should be "its"

```suggestion
                        unreachable!("Not operator is handled in its own case");
```

---

_@AlexWaygood reviewed on 2024-10-19 19:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2483 on 2024-10-19 19:16_

No worries!

---

_Review requested from @carljm by @Glyphack on 2024-10-19 19:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:23 on 2024-10-20 01:49_

```suggestion
largest_u32 = 4_294_967_295
reveal_type(2**2)  # revealed: Literal[4]
reveal_type(1 ** (largest_u32 + 1))  # revealed: int
reveal_type(2**largest_u32)  # revealed: int
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:57 on 2024-10-20 01:49_

```suggestion
# error: [division-by-zero] "Cannot divide object of type `Literal[True]` by zero"
True / False
# error: [division-by-zero] "Cannot divide object of type `Literal[True]` by zero"
bool(1) / False
```

---

_@carljm approved on 2024-10-20 01:52_

Looks great, thank you!!

---

_Merged by @carljm on 2024-10-20 01:57_

---

_Closed by @carljm on 2024-10-20 01:57_

---

_Branch deleted on 2024-10-20 07:51_

---
