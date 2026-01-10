```yaml
number: 17298
title: "[syntax-errors] `yield`, `yield from`, and `await` outside functions"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: main
head: brent/syn-yield-outside-function
created_at: 2025-04-08T18:03:06Z
updated_at: 2025-04-11T14:16:25Z
url: https://github.com/astral-sh/ruff/pull/17298
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] `yield`, `yield from`, and `await` outside functions

---

_Pull request opened by @ntBre on 2025-04-08 18:03_

Summary
--

This PR reimplements [yield-outside-function (F704)](https://docs.astral.sh/ruff/rules/yield-outside-function/) as a semantic syntax error. Despite the name, this rule covers `yield from` and `await` in addition to `yield`.

Test Plan
--

New linter tests, along with the existing F704 test.

---

_Label `rule` added by @ntBre on 2025-04-08 18:03_

---

_Comment by @github-actions[bot] on 2025-04-08 18:12_

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

_Marked ready for review by @ntBre on 2025-04-08 18:13_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-08 18:13_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-08 18:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/inline/ok/match_classify_as_keyword_1.py`:25 on 2025-04-08 20:07_

This and other changes like this seems a bit unfortunate but I do understand the reason behind it. A potential solution would be to introduce a new `test_semantic_err` (or something like that) that is used to make sure that (a) it's a valid AST i.e., no parse errors and (b) there are semantic errors. 

Curious to hear others thoughts on this.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-08 20:30_

The original implementation used inclusion instead of exclusion:
```rs
if scope.kind.is_module() || scope.kind.is_class() {
```

What happens when it's detected in other scopes like comprehension?

Should we expose a `scope` method on `SemanticSyntaxContext` and use that instead of maintaining the state ourselves?

---

_@dhruvmanila reviewed on 2025-04-08 20:31_

---

_@ntBre reviewed on 2025-04-08 20:54_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-08 20:54_

It's a good question, but based on the other available scope types, I think `Function` and `Lambda` are the only ones that actually apply, and it was only a few lines of code to track them here.

It seems the current rule is actually too permissive in allowing these in comprehension and type scopes ([playground](https://play.ruff.rs/11eb1dfe-9e9d-4300-9dad-f23c390d277a)):

```pycon
>>> [(yield x) for x in range(3)]
  File "<python-input-50>", line 1
    [(yield x) for x in range(3)]
      ^^^^^^^
SyntaxError: 'yield' inside list comprehension
```

Although we could also consider not flagging the type parameter cases since those are already handled by another syntax error. Based on our previous discussions, I think it's reasonable to flag them both, though, because even if it were valid in the type parameter/alias it would still be invalid outside of a function.

https://github.com/astral-sh/ruff/blob/9327c8f1c0f161867600389b8a372c37be4727c8/crates/ruff_python_semantic/src/scope.rs#L170-L178

---

_@ntBre reviewed on 2025-04-08 20:59_

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/ok/match_classify_as_keyword_1.py`:25 on 2025-04-08 20:59_

I think we discussed then when first expanding the scope of the inline tests, and it seemed preferable to test both kinds of errors at the same time. I'm happy either way, though. It would certainly focus the diffs in these PRs. 

We could also add more options to `JsonParseOptions` that don't necessarily have to correspond to the real `ParseOptions` type. Then we could simply set `skip_semantic_errors` or similar in the cases where it's needed. However, that would still shift all of the diff ranges and line numbers...

---

_@ntBre reviewed on 2025-04-08 21:22_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-08 21:22_

We may well need a more sophisticated way of tracking scopes, though. I'm trying to implement `await outside async function` and running into trouble for cases like this:

```python
async def foo():
    @await bar
    def baz(): ...
```

This should be valid, but the async context tracking from #17177 updates the `in_async_context` field on the _test visitor_ when visiting the `baz` function definition before visiting its decorators, leading to a false positive.

I either need to defer visiting the function itself, like in the real `ast::Checker`, or we could move to a `scope` method on the trait, or duplicate that kind of scope tracking here.

---

_@dhruvmanila reviewed on 2025-04-08 21:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/inline/ok/match_classify_as_keyword_1.py`:25 on 2025-04-08 21:25_

> I think we discussed then when first expanding the scope of the inline tests, and it seemed preferable to test both kinds of errors at the same time.

Ah right, I think I confused that discussion with the recent one where I suggested that we should include both parse and semantic errors.

> We could also add more options to `JsonParseOptions` that don't necessarily have to correspond to the real `ParseOptions` type.

Yeah, using `JsonParseOptions` would still produce large diff but using a separate identifier wouldn't. 

This doesn't need to block this PR as it's a separate discussion. I think it's fine for now to move ahead with what you have in this PR.

---

_@dhruvmanila reviewed on 2025-04-08 21:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-08 21:32_

I see, yeah that seems reasonable then.

I'd still prefer to re-use the existing scope information from the `Checker` than to add new state here.

> It seems the current rule is actually too permissive in allowing these in comprehension and type scopes

Oh, interesting. Do we need to gate that behind preview as it's an increase in the rule scope? I think it's fine as it's mainly a syntax error so I'd consider this a bug fix instead.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:623 on 2025-04-08 21:36_

I think we might need to guard this against the scope check as this should only return if the `await` is being used at the module scope level.

---

_@dhruvmanila reviewed on 2025-04-08 21:36_

---

_@dhruvmanila reviewed on 2025-04-08 21:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:623 on 2025-04-08 21:38_

For example, if we're in a class scope and not in a function scope, this would return. We should also update the test case for Jupyter Notebooks to consider this specific case which would've caught this.

---

_@ntBre reviewed on 2025-04-08 21:42_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:623 on 2025-04-08 21:42_

Ah good catch. I think I need to write another linter notebook test here, this is the second mistake I've made in this check!

---

_@ntBre reviewed on 2025-04-08 22:08_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-08 22:08_

> I'd still prefer to re-use the existing scope information from the `Checker` than to add new state here.

The one major downside of shifting responsibility to the trait is that it makes the inline tests harder to write. We have to duplicate the state-tracking logic in the test visitor/context, and the correspondence between the real use (in `Checker` and the red-knot analog) and test use breaks down. That's one reason I've leaned toward tracking state here, but relying on the trait has benefits too. And this breakdown is already happening with the deferred function bodies anyway, so it probably does make sense to move into the trait.

> Oh, interesting. Do we need to gate that behind preview as it's an increase in the rule scope? I think it's fine as it's mainly a syntax error so I'd consider this a bug fix instead.

That's also a good question that applies to #17285 as well. I think they can both be considered bug fixes, but it also seems reasonable to preview-gate.



---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:604 on 2025-04-09 06:36_

Are lambdas consider to be in a function scope or class scope or do they not change the scope?

For example, the following (in module scope) shouldn't raise an error)

```
a = lambda a: yield a
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-09 06:40_

> We have to duplicate the state-tracking logic in the test visitor/context, and the correspondence between the real use (in Checker and the red-knot analog) and test use breaks down.

I agree that this is unfortunate. I still think that reusing the scope from `Checker` ultimately leads to more correct code because that scope tracking has much more extenisve test coverage. 

I'd suggest that we move the basic scope tracking into the context, extend the test visitor to do some basic scope tracking, and write an integration test that demonstrate that scope tracking in combination with `Checker` works as expected (we now have the infrastructure to just do this and the integration test should verify that all methods exposed on `Context` work as expected)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:28 on 2025-04-09 06:40_

Should this be a method on `ctx` similar to `PythonVersion` because it isn't state? 

---

_@MichaReiser reviewed on 2025-04-09 06:41_

---

_@ntBre reviewed on 2025-04-09 12:05_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:619 on 2025-04-09 12:05_

Sounds good. I think I'll start this in a separate PR and then rebase this one. Last time I tried it, the test context changes were pretty extensive, and there were also a couple of changes to the `Checker'`s `SemanticModel`.

---

_Comment by @ntBre on 2025-04-09 20:41_

The PR is now rebased onto the context changes, and I think I addressed the other review feedback, but it's still a huge number of changes to existing snapshots. Those are mostly just noise, but I will say they alerted me to one issue in the test context (how it visited generators) at least.

One other way of avoiding these snapshot diffs is just to rely on linter integration tests instead of inline tests at all. It feels a bit unreliable having to update the test context to pass these tests anyway, so fully reusing the new test infrastructure in the linter could solve several problems at once.

To be more concrete, the inline tests are still great for cases that don't rely on information from the `Context`, but I'm suggesting moving the `Context`-dependent tests to the linter and restoring the `SemanticSyntaxCheckerVisitor` to a very, very simple visitor like it was before and leaving it with dummy context methods:

```rust
fn visit_stmt(&mut self, stmt: &ast::Stmt) {
	self.with_semantic_checker(|semantic, context| semantic.visit_stmt(stmt, context));
	ast::visitor::walk_stmt(self, stmt);
}
```

I'm interested in hearing y'all's thoughts on that.

---

_Comment by @MichaReiser on 2025-04-10 12:54_

This PR is in my review queue, but my afternoon is rather busy. I'm not sure if I get to reviewing it today

---

_Comment by @ntBre on 2025-04-10 12:58_

No rush, I was thinking about playing with one of these test options (the one I mentioned or Dhruv's idea for a separate inline test mechanism) to make this more reviewable anyway. 

Oh, I also hadn't noticed the new ecosystem report. I'm guessing those are a lot of false positives, so really no rush on reviewing this.

---

_Comment by @ntBre on 2025-04-10 17:06_

I hope this should be much more manageable to review now. I reverted to dummy methods on the test visitor, which also reverted all of the unrelated snapshot changes. All of the tests are now in the linter and rely on the real visitor.

The false positives should also be addressed.

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__await_scope_notebook.snap`:6 on 2025-04-10 17:10_

This looks very suspicious, but this is covered by PLE1142, which I'm planning to handle separately. The same is true for the `lambda: await 1` case in the `.py` test. 

---

_@ntBre reviewed on 2025-04-10 17:10_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__yield_scope.py.snap`:31 on 2025-04-10 20:33_

Unrelated to this PR and does not need to be done in this PR but maybe we should update the diagnostic message for `await` keyword to "`await` statement outside of an async function".

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__await_scope_notebook.snap`:6 on 2025-04-10 20:35_

Can you say more about what exactly is suspicious?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__await_scope_notebook.snap`:6 on 2025-04-10 20:45_

Regardless, the snapshots for Notebook looks off as it doesn't contain cell numbers and the line numbers are not isolated to each cell. Refer to https://github.com/astral-sh/ruff/blob/8b2727cf67ef4ed43723ff5c11ea936838e25c26/crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__unused_variable.snap as an example snapshot. I think the `assert_messages` macro usage should be updated for notebook tests, refer to https://github.com/astral-sh/ruff/blob/8b2727cf67ef4ed43723ff5c11ea936838e25c26/crates/ruff_linter/src/linter.rs#L836-L851 as an example.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:622 on 2025-04-10 20:52_

I think we should start adding some documentation for these methods. I'd find it confusing because there are both `in_function_scope` and `in_function_context` but I don't have a better suggestion for those methods. One solution would be to inline `in_function_scope` which a lot of the linter code does.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:655 on 2025-04-10 20:53_

nit: What about the following to avoid multiple function calls?
```suggestion
		matches!(kind, ScopeKind::Function(_) | ScopeKind::Lambda(_))
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:618 on 2025-04-10 20:54_

What's the reason for special casing the class scope? Should this be bundled with the final match arm instead?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1060 on 2025-04-10 20:57_

I appreciate the examples mentioned in the documentation but should we limit them to 1 or 2 to avoid them getting outdated or that we forget updating them?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:622 on 2025-04-10 20:58_

(I see that the documentation would be part of the trait.)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1431 on 2025-04-10 20:59_

Thank you for adding documentation for this :) 

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1492 on 2025-04-10 20:59_

Let's document this method especially w.r.t `in_function_scope` (mainly a follow-up from my other comment)

---

_@dhruvmanila reviewed on 2025-04-10 21:01_

This is great! I think there are couple of follow-ups and minor changes but otherwise looks good.

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__await_scope_notebook.snap`:6 on 2025-04-10 21:07_

> Can you say more about what exactly is suspicious?

I thought it would look suspicious in the review that I commented `SyntaxError` next to a line without an error in the snapshot, so I just wanted to call out that this was intentional up front. Related to your comment about "await outside _async_ function," that's handled by PLE1142, and we currently emit both of those ([playground](https://play.ruff.rs/fb6643ff-e9d1-4618-808d-47dd62fba2cc)), so I was going to emit two syntax errors for them too: one here and one in a follow-up PR, which will add an error here in this snapshot.

---

_@ntBre reviewed on 2025-04-10 21:07_

---

_@dhruvmanila reviewed on 2025-04-10 21:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1492 on 2025-04-10 21:07_

I see that there's a writeup regarding this in the trait documentation, I think it'd be more useful to have it here.

---

_@ntBre reviewed on 2025-04-10 21:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:622 on 2025-04-10 21:13_

Yeah I put it on the trait in case there were more of these later, but this is the only one for now, so I can just move it down.

Unfortunately we can't really inline `in_function_scope` (unless I'm missing something) because it would require access to the `Scope` type, which I tried to avoid in #17314. That would actually remove the need for a lot of these little methods, but I think it would add complications of its own, especially once we integrate with red-knot.

---

_@dhruvmanila reviewed on 2025-04-10 21:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__await_scope_notebook.snap`:6 on 2025-04-10 21:14_

Understood, thanks! That makes sense.

---

_@dhruvmanila reviewed on 2025-04-10 21:15_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:622 on 2025-04-10 21:15_

> Unfortunately we can't really inline `in_function_scope` (unless I'm missing something) because it would require access to the `Scope` type, which I tried to avoid in #17314. That would actually remove the need for a lot of these little methods, but I think it would add complications of its own, especially once we integrate with red-knot.

Ah right, ofcourse. Sorry for the noise.

---

_@ntBre reviewed on 2025-04-10 21:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:622 on 2025-04-10 21:17_

No worries, it _would_ be nice to have!

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1060 on 2025-04-10 21:18_

Sure, I think I ended up moving most of these into the tests, so that's a good idea.

---

_@ntBre reviewed on 2025-04-10 21:18_

---

_@ntBre reviewed on 2025-04-10 21:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:618 on 2025-04-10 21:28_

Yeah this is probably my least favorite part of the PR and one of the reasons that I wish we could just access the `Scope` directly. This is not really a general `in_function_context` method, it's more like an `await_allowed_in_this_scope` method, but that seemed too specific of a name. Basically, only classes break an `async` scope, unlike generators, which is also different from how `yield` and `yield from` work (they _are_ broken by generators).

Should I just rename it? Or do you have another suggestion here?
<p>
<details>
<summary>Python REPL session</summary>

```pycon
>>> async def f():
...     [await x for x in y]
...
>>> async def f():
...     class C:
...         [await x for x in y]
...
  File "<python-input-1>", line 3
    [await x for x in y]
    ^^^^^^^^^^^^^^^^^^^^
SyntaxError: asynchronous comprehension outside of an asynchronous function
>>> def f():
...     yield 1
...
>>> def f():
...     [(yield 1) for x in y]
...
  File "<python-input-3>", line 2
    [(yield 1) for x in y]
      ^^^^^^^
SyntaxError: 'yield' inside list comprehension
```

</details></p>

---

_@ntBre reviewed on 2025-04-10 21:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:618 on 2025-04-10 21:29_

Resolving this will also help to clear up the docs and relationship between the other methods, I think.

---

_@ntBre reviewed on 2025-04-10 22:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__await_scope_notebook.snap`:6 on 2025-04-10 22:05_

Good catch on the notebook too. I don't think I can use `assert_notebook_path` for the other, old notebook test because it filters out syntax errors, but it works perfectly here since this error corresponds to a ruff rule.

---

_@dhruvmanila reviewed on 2025-04-10 22:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:618 on 2025-04-10 22:09_

Oh, yeah that isn't obvious from the function name 😅 

In that case, it's a lot similar to `in_async_context` which is why I guess you choose the name `in_function_context`.

It would be useful to rename it and add relevant documentation.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:579 on 2025-04-11 06:54_

Nit: You could implement `From<YieldOutsideFunctionKind> for DeferralKeyword`. You can then add a `new` function to `YieldOutsideFunction` that takes an `Into<DeferralKeyword>, so that `DeferralKeyword` can remain private.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:617 on 2025-04-11 06:59_

Is your plan here to emit a different `SemanticSyntaxErrorKind` for `await` outside of `async` so that they can be re-coded to `PLE1142`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:618 on 2025-04-11 07:00_

Nit: I'd prefer to have the notebook branch be its own if expression and have some documentation explaining why we skip the module scope for notebooks (as this isn't something that's very obvious why)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1492 on 2025-04-11 07:02_

I like the trait level documentation but I agree, that it would be useful to have a short summary of what it is. It could also refer to the trait documentation for more details.

---

_@MichaReiser approved on 2025-04-11 07:04_

---

_@ntBre reviewed on 2025-04-11 12:55_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:617 on 2025-04-11 12:55_

Yes, exactly! It seems a bit redundant, but it will preserve the current behavior.

That's the last error I'm planning to handle after this and #17300.

---

_@ntBre reviewed on 2025-04-11 12:56_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:618 on 2025-04-11 12:56_

That's a good point. I'll bring back the comment from the deleted F704 implementation.

---

_@dhruvmanila approved on 2025-04-11 13:56_

Nice! Thanks for updating the notebook tests, I think `test_async_comprehension_notebook` would also need to be updated but can be done in a follow-up PR as well.

---

_Comment by @ntBre on 2025-04-11 13:58_

Thank you both for the reviews! I think I've addressed everything. 

I went with `in_await_allowed_context` instead of `in_function_context` and added docs on the method itself. My other name candidate was `in_await_context`, but that seemed way too close to `in_async_context`. I also considered `await_allowed`, but I did still want to point back to the trait-level docs, which refer to the related methods as `in_*_context` methods. Happy to iterate further here if you have any suggestions. 

At some point in the future, we may want to get rid of the `await` behavior here and only emit `await` outside _async_ function (PLE1142) anyway.

---

_Merged by @ntBre on 2025-04-11 14:16_

---

_Closed by @ntBre on 2025-04-11 14:16_

---

_Branch deleted on 2025-04-11 14:16_

---
