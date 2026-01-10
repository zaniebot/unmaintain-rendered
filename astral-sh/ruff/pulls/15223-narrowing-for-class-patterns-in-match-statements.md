```yaml
number: 15223
title: Narrowing for class patterns in match statements
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/class-match
created_at: 2025-01-02T16:15:11Z
updated_at: 2025-01-07T20:58:14Z
url: https://github.com/astral-sh/ruff/pull/15223
synced_at: 2026-01-10T20:34:00Z
```

# Narrowing for class patterns in match statements

---

_Pull request opened by @dcreager on 2025-01-02 16:15_

We now support class patterns in a match statement, adding a narrowing constraint that within the body of that match arm, we can assume that the subject is an instance of that class.

#14740

---

_Label `red-knot` added by @AlexWaygood on 2025-01-03 00:02_

---

_@sharkdp reviewed on 2025-01-03 15:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/match.md`:24 on 2025-01-03 15:53_

Oh cool. I totally missed that we can do this now.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1128 on 2025-01-03 21:52_

h/t to @sharkdp for noticing that we were performing these steps out of order.  The correct order is:

1. visit the pattern first
2. add the pattern constraint
3. visit the guard expression and body

That is, each pattern's constraint applies to the guard and body of that match case, but not to the pattern itself.

Also note that this is different than what David first suggested, which was that the pattern constraint should not apply to the guard either.  I think that it _should_ apply to the guard, giving my reading of this part of [the spec](https://docs.python.org/3/reference/compound_stmts.html#overview):

> If the pattern succeeds, the corresponding guard (if present) is evaluated. In this case all name bindings are guaranteed to have happened.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1551 on 2025-01-03 21:54_

This isn't used anywhere else, so I updated the call site to only call it once and then inlined it.  That way I didn't need to add a field to pass the constraint ID back up to where we're iterating through all of the cases.

---

_Marked ready for review by @dcreager on 2025-01-03 21:54_

---

_Review requested from @carljm by @dcreager on 2025-01-03 21:54_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-03 21:54_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-03 21:54_

---

_@dcreager reviewed on 2025-01-03 21:54_

---

_@sharkdp reviewed on 2025-01-04 08:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1128 on 2025-01-04 08:42_

> Also note that this is different than what David first suggested, which was that the pattern constraint should not apply to the guard either. I think that it should apply to the guard

Ah, yes. Of course.

---

_Comment by @carljm on 2025-01-06 18:38_

Given the failing tests and the "WIP, not working yet" PR description, moving this to draft state for now? Let me know if you would like a review in the current state.

---

_Converted to draft by @carljm on 2025-01-06 18:38_

---

_Comment by @github-actions[bot] on 2025-01-06 19:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:1788 on 2025-01-06 19:40_

This separation between `infer_match_pattern` and `infer_match_pattern_impl` has to do with how we handle nested subpatterns in the semantic index builder.  (I've changed the name of the latter to `infer_nested_match_pattern` to better describe this.)

We currently only call `add_pattern_constraint` for top-level patterns, and do not recurse into subpatterns.  That means, in particular, that we don't create a standalone expression for the value part of a value pattern nor for the class identifier in a class pattern.  That means we need to know here whether we are looking at a top-level pattern (in which case we would call `infer_standalone_expression`) or a nested subpattern (in which case we would call `infer_expression`).

I've kept that separation, since it results in a smaller diff for this PR.  But it seems brittle to have standalone expressions only for some value/class patterns, but not others.  So I've added this TODO comment to consider updating the semantic index builder to _always_ create standalone expressions.  (And 252290e2ae60716772bc3cda49b137dbc8a34361 actually implements that, should we want to do that now.)

@sharkdp, do have strong opinions about the two choices?

---

_@dcreager reviewed on 2025-01-06 19:41_

---

_Marked ready for review by @dcreager on 2025-01-06 19:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/match.md`:37 on 2025-01-06 20:46_

I don't think we need to do it in this PR (I didn't mention it in the issue; in fact I ignored it in my specification of the test case), but since cases are checked in order and only one can match, we can also constrain each case using the negation of all prior constraints. (Note that this doesn't apply if there's also an `if` guard, since the `if` guard can mean that the case doesn't match even if the pattern would have.)

```suggestion
        # TODO could be `B & ~A`
        reveal_type(x)  # revealed: B
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1788 on 2025-01-06 21:09_

I think the standalone-expressions system is brittle in general, not only in this this case. It requires hard-coded agreement between the semantic index builder and type inference about which expressions (in which syntactic positions) are standalone and which are not. This is just a particular case of that general problem. (In fact the brittleness problem is even more general than that; we also require hardcoded agreement between semantic-index builder and inference about e.g. what AST nodes we will/won't create a Definition for.)

The reason it works this way is because we want to minimize our overall number of Salsa ingredients, because they are associated with memory and performance overhead. But if we need to query the type of an expression as its own Salsa query (because it is either part of a definition or part of a constraint on a definition, which are things we need to query independently from the rest of the surrounding scope), then we need a Salsa ingredient for it. So we record it as a standalone expression.

In https://github.com/astral-sh/ruff/commit/252290e2ae60716772bc3cda49b137dbc8a34361 we would actually create both an `Expression` ingredient and a `Constraint` ingredient for every sub-pattern. I agree that this is less brittle and simpler to handle in inference. But it's also wasteful; we don't need those Salsa ingredients, because we will never need to query them separately from the top-level pattern. The logical conclusion of this approach would be to create an `Expression` for every expression, period. Which would indeed be simpler and less brittle (and is basically what I wanted to do in our bespoke inference-caching before we decided to adopt Salsa), but also prohibitively expensive.

(Creating a `Constraint` for each nested pattern is semantically odd as well, because nested patterns cannot function as an independent constraint on the type of any symbol, they are just potentially additional information -- e.g. information about the possible types of attributes -- within the context of the constraint applied by the top-level pattern.)

It's certainly possible that we could accept some inefficiency here for the sake of simpler code; we make that trade-off all the time. But I'm not convinced that it's warranted here; the current code is not that complex, and I think we arguably create more confusion by creating unused nested `Constraint` ingredients that aren't semantically sensible as standalone constraints.

So I would suggest to replace this `TODO` comment with something that explains why the distinction exists.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:429 on 2025-01-06 21:18_

Hmm, interesting. So normally the idea would be that when inferring types for a standalone-expression inference scope, we would only infer types for sub-expressions of the standalone expression. This is to ensure that we have a clear "owning inference scope" for every expression, and don't end up inferring some expressions multiple times, as part of different inference scopes.

But of course the inference code doesn't really care about what is actually nested inside what in the AST, as long as we do know which expressions belong to which inference scope, and only infer them there. So I don't think there is a problem with making `pattern.cls` be the standalone expression, as long as we know that `pattern.arguments` is "part of" the same inference scope.

The natural thing to do would be to have `pattern` itself be the standalone expression, since it is the parent of both `cls` and `arguments`. But since `pattern` is not an `ast::Expr`, we can't do that; at least not without reworking the `Expression` ingredient to be able to wrap nodes that are not actually expressions, which would be both invasive and semantically odd.

So I think what you've done here is the best option. But it may be worth an explicit comment over on the type inference side, that we consider `pattern.arguments` to be part of the inference scope of the standalone expression `pattern.cls`, even though they aren't actually nested in the AST. (Or an alternate phrasing would be that we use `pattern.cls` as a stand-in for the overall `pattern`, since its an expression.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:333 on 2025-01-06 21:29_

We could sometimes infer a static truthiness here (and also for `Singleton` for that matter), but for now we can consider that a possible future feature, to be driven by actual demand. We don't need to add it as a TODO.

---

_@carljm approved on 2025-01-06 21:29_

This is great, thank you! Sorry for the novel-length comments; I hope they are useful in clarifying some things.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:1788 on 2025-01-06 22:08_

Thank you for the explanation!  That's very helpful — I was able to figure out the "what", but the "why" is useful context.

> Creating a `Constraint` for each nested pattern is semantically odd as well

Yes, definitely — I had also considered adding a different method that would have recursed into the subpatterns, creating the standalone expression but not the `Constraint`.  But that would have just moved the complexity from the inference side to the builder.

> So I would suggest to replace this `TODO` comment with something that explains why the distinction exists.

Done — please lmk if I got any of the details wrong



---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:429 on 2025-01-06 22:08_

I've taken this into account in the new comment over in `infer_match_pattern`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:418 on 2025-01-06 22:09_

Does your comment below about inference scopes suggest that the guard of a pattern doesn't need to be wrapped in its own standalone expression?  And instead that we can consider it part of the inference scope of the standalone expression that we create for the pattern?  (That might require being exhaustive in creating a standalone expression for all pattern variants, even the ones we don't yet support for inference/narrowing?)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/narrow/match.md`:37 on 2025-01-06 22:11_

Ah yes!  @sharkdp already did that with the visibility constraints, but that's a good call-out that we can do that for the inferred types as well

---

_@dcreager reviewed on 2025-01-06 22:11_

---

_@carljm reviewed on 2025-01-07 01:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:429 on 2025-01-07 01:44_

The comment you wrote is excellent, and accurately reflects what I said! Unfortunately, after stepping away from the computer and going for a run, I realized that my description of the situation was wrong :/

The reason the nesting structure of the AST _is_ important is that, given an AST node, we can only access its children; we have no way to walk up the AST, or find sibling nodes. And the whole point of a standalone expression is that we can enter type inference given only that node (when you call `infer_expression_types` in `evaluate_match_pattern_class`, in this case.)

In the current PR, when we are inferring types for the entire scope, we will hit the match-pattern node, infer its `cls` attribute as a standalone expression, and infer its `arguments` attribute as normal expressions. The arguments will not be inferred as part of the standalone expression inference region, they will be inferred as part of the outer scope inference region.

When we are inferring just the standalone expression region, we start with the expression that is the `cls` attribute of the pattern, and can only reach it and its sub-expressions.

For the purposes of the current PR, this works fine, because the only thing we care about for the type narrowing is the `cls`; we don't care about the arguments. But if you tried to pull out types for the argument expressions in `evaluate_match_pattern_class`, you would find they are not in the returned `TypeInference`.

I think, given that it's unclear exactly how much we will narrow based on arguments to a class patter, and how we will implement that, that we shouldn't go out out of our way for it now. So I still think the implementation you have here is good for now. We can cross the arguments bridge when we come to it. But we should update the comment(s) to accurately reflect that class pattern arguments are not currently inferred as part of the standalone expression inference region, but that's fine for now because at the moment we don't need them in our handling of the constraint.

Sorry for the confusion.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1107 on 2025-01-07 07:30_

Nit: Using `assert_eq` gives more useful assertion messages if `current_match_case` is `Some` for whatever reason
```suggestion
                debug_assert_eq!(self.current_match_case, None);
```

---

_@MichaReiser reviewed on 2025-01-07 07:32_

---

_@carljm reviewed on 2025-01-07 07:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:418 on 2025-01-07 07:58_

Currently the guard doesn't need to be its own standalone expression, because we don't actually do any narrowing based on it. But if/when we do, it will need to be its own standalone expression, because of what I described in my more recent comment; we can't actually include non-child nodes as part of the inference region of a standalone expression.

---

_@dcreager reviewed on 2025-01-07 15:37_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:418 on 2025-01-07 15:37_

Does this suggest that it's worth a follow-on PR to allow standalone expressions to wrap `ast::Pattern` in addition to `ast::Expr`?  (Or I guess `ast::MatchCase`, so that it can encompass both the pattern and the guard)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:429 on 2025-01-07 15:48_

> But we should update the comment(s) to accurately reflect that class pattern arguments are not currently inferred as part of the standalone expression inference region,

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1107 on 2025-01-07 15:50_

I had to also add `Debug` and `PartialEq` derivations to make this work

---

_@dcreager reviewed on 2025-01-07 15:51_

---

_@carljm reviewed on 2025-01-07 20:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:418 on 2025-01-07 20:48_

The other approach (which is already used for guards, although we don't use it on the inference side yet) is that the `PatternConstraintKind` can just store references to multiple expressions. (The code isn't super clear because it uses tuple variants instead of struct variants, but the optional second expression in both `PatternConstraintKind::Singleton` and `PatternConstraintKind::Value` is the guard expression, which is already recorded as a standalone expression in the semantic index builder.) So I don't think there's anything more to be done with guards, until we are ready to actually use them in narrowing.

The situation with class pattern arguments is more complex, since they aren't expressions either, but rather nested patterns. So we'll need to do something different there; one option is to allow an `ast::Pattern` itself to be an inference region (similar to a standalone expression). Another option might be to have `PatternConstraintKind::Class` recursively contain a vec of constraints for the arguments; this would be more similar to the recursive-standalone-expression approach you experimented with. It would result in more `Constraint` ingredients for large nested patterns, but in practice that might be fine, if such patterns aren't very common.

My suggestion was that we just punt on this until we actually want to narrow based on class-pattern arguments.

---

_@carljm approved on 2025-01-07 20:51_

Looks good, thank you!

---

_Merged by @dcreager on 2025-01-07 20:58_

---

_Closed by @dcreager on 2025-01-07 20:58_

---

_Branch deleted on 2025-01-07 20:58_

---
