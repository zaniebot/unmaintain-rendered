```yaml
number: 18008
title: "[ty] Don't warn `yield` not in function when `yield` is in function"
type: pull_request
state: merged
author: maxmynter
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: yield-outside-fn-duplication
created_at: 2025-05-10T19:03:31Z
updated_at: 2025-05-21T16:16:25Z
url: https://github.com/astral-sh/ruff/pull/18008
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Don't warn `yield` not in function when `yield` is in function

---

_Pull request opened by @maxmynter on 2025-05-10 19:03_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes: https://github.com/astral-sh/ty/issues/299
## Summary
Don't warn `yield` not in function when `yield` is in function.

This is achieved by iterating through the scope stack to check if it contains a function scope. 
<!-- What's the purpose of the change? What does it do, and why? -->

The issue was found when creating the semantic syntax `md` integration tests (https://github.com/astral-sh/ruff/pull/17748#discussion_r2082080479). 
## Test Plan
Updated integration tests to reflect this behavior. 
<!-- How was it tested? -->

----

I don't know if I fully get how issues / PR's work across the boundaries of `ruff` and `ty`. Let me know if i'm doing something wrong.

---

_Review requested from @carljm by @maxmynter on 2025-05-10 19:03_

---

_Review requested from @AlexWaygood by @maxmynter on 2025-05-10 19:03_

---

_Review requested from @sharkdp by @maxmynter on 2025-05-10 19:03_

---

_Review requested from @dcreager by @maxmynter on 2025-05-10 19:03_

---

_Comment by @github-actions[bot] on 2025-05-10 19:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @dylwil3 on 2025-05-11 20:24_

---

_Renamed from "Don't warn `yield` not in function when `yield` is in function" to "[ty] Don't warn `yield` not in function when `yield` is in function" by @dylwil3 on 2025-05-11 20:24_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:309 on 2025-05-12 01:26_

Can you move the class example down below into a function too? I think that should be the last one.

---

_Review comment by @ntBre on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2507 on 2025-05-12 01:36_

I think we should also return false if we see a `ScopeKind::Class`:

```pycon
>>> def f():
...     class C:
...         yield 5
...
  File "<python-input-1>", line 3
    yield 5
    ^^^^^^^
SyntaxError: 'yield' outside function
```

I'm also realizing that I gave bad advice in my comment on the issue. The `in_*_scope` methods _are_ supposed to refer to the directly-enclosing scope:

https://github.com/astral-sh/ruff/blob/7a48477c6797ae817c33994e5cddb7f8375118bb/crates/ruff_python_parser/src/semantic_errors.rs#L1649-L1663

We may just want to reuse [`in_await_allowed_context`](https://github.com/astral-sh/ruff/blob/7a48477c6797ae817c33994e5cddb7f8375118bb/crates/ruff_python_parser/src/semantic_errors.rs#L1718) for [detecting](https://github.com/astral-sh/ruff/blob/7a48477c6797ae817c33994e5cddb7f8375118bb/crates/ruff_python_parser/src/semantic_errors.rs#L782) this syntax error and also possibly rename it to something like `in_function_context`.

That should fix the ruff implementation of the rule too, which I guess I hadn't seen issues with either.

---

_@ntBre reviewed on 2025-05-12 01:37_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-12 01:42_

---

_Review requested from @MichaReiser by @maxmynter on 2025-05-13 01:42_

---

_Review requested from @dhruvmanila by @maxmynter on 2025-05-13 01:42_

---

_Comment by @github-actions[bot] on 2025-05-13 01:52_

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

_Comment by @maxmynter on 2025-05-13 02:04_

All right, this took longer than I thought for a quite small diff.

- Reverted the change to `in_function_scope` to only consider the immediate scope.

- I'm on the fence with renaming `in_await_allowed_context`. Especially considering the contextual comment in the `trait` definition it seems the current name better conveys what the function does.

- The upward scope stack traversal in `in_await_allowed_context` needs to be checked against generator and comprehensions to not give false positives/negatives.

-  Rename `in_sync_comprehension` to `in_sync_comprehension_scope` for consistency with other locator methods in scope. 

## Questions:
- How important is performance here? Specifically, how expensive is traversing the scope stack? We can avoid the duplicate call of `is_await_allowed_context` at some cost of readability with more nesting. I've already ordered the early returns in increasing expensiveness. 


---

_Comment by @maxmynter on 2025-05-13 02:08_

The [notebook](https://github.com/openai/openai-cookbook/blob/main/examples/gpt4-1_prompting_guide.ipynb) in the ecosystem changes gives the same `Invalid JSON` error when seen through the Git web ui. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-13 02:11_

---

_Review request for @sharkdp removed by @sharkdp on 2025-05-13 11:59_

---

_Review requested from @ntBre by @maxmynter on 2025-05-13 22:53_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1737 on 2025-05-14 15:28_

The implementations of this method traverse the parent scopes, which would make it a `_context` method rather than a `_scope` method based on the trait documentation. I think I thought `in_sync_comprehension_context` was a bit redundant when naming it originally (or it may have come before the context/scope convention), but if we really want to be consistent that's what it should be.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:776 on 2025-05-14 15:29_

I thought there was a reason I had this where it was before, but if all the tests are passing, it must be okay.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:792 on 2025-05-14 15:40_

This seems a bit strange because `in_sync_comprehension_scope` and `in_await_allowed_context` both traverse the parent scopes, but `in_generator_scope` doesn't. Is there a way to break this check by nesting generators somehow?

I think I would probably lean toward a separate context method anyway, just for clarity. Traversing scopes should be pretty cheap since it's just iterating backwards over a vector for both ruff and ty, but I think this whole check would be simplified if we had an `in_yield_allowed_context` method or similar. And then we can traverse the stack once anyway.

Ah, actually I think that's the reason for the `else-if` on `kind.is_await`. If we don't return from there, we'll run all of these checks again, even though we already know we want a diagnostic for `await`. So I think I would put this whole section back to something like this:

```rust
if kind.is_await { 
    // ...
} else if ctx.in_yield_allowed_context() {
    return
}
```

---

_@ntBre reviewed on 2025-05-14 15:41_

---

_Comment by @ntBre on 2025-05-14 15:43_

> The [notebook](https://github.com/openai/openai-cookbook/blob/main/examples/gpt4-1_prompting_guide.ipynb) in the ecosystem changes gives the same `Invalid JSON` error when seen through the Git web ui.

Yeah no worries about this, I've been seeing it on every PR. If the issue persists we can ignore the file from the ecosystem check.

Edit: looks like they fixed it 6 hours ago. Next time the ecosystem check runs it should update!

---

_@maxmynter reviewed on 2025-05-14 20:22_

---

_Review comment by @maxmynter on `crates/ruff_python_parser/src/semantic_errors.rs`:792 on 2025-05-14 20:22_

Makes sense. Adressed in [1d0e43b](https://github.com/astral-sh/ruff/pull/18008/commits/1d0e43b2a963df255c84e93e516657f1da2ca24e)

I was shying away from adding a new method to the trait because then the implementation in `crates/ruff_linter/src/checkers/ast/mod.rs` isn't used.

But I now agree it's not that bad. 

---

_@maxmynter reviewed on 2025-05-14 20:23_

---

_Review comment by @maxmynter on `crates/ruff_python_parser/src/semantic_errors.rs`:1737 on 2025-05-14 20:23_

Thanks for catching this. Must've been late.

I get your point on redundancy and reverted in [a62a990](https://github.com/astral-sh/ruff/pull/18008/commits/a62a99078faa611cc0ee72578cc37179c792f578)

---

_Comment by @maxmynter on 2025-05-14 20:26_

It seems that backticked parts were sliced out of the commit messages. 

They were supposed to be

- Drop `_scope` from `in_sync_comprehension_scope` ([a62a990](https://github.com/astral-sh/ruff/pull/18008/commits/a62a99078faa611cc0ee72578cc37179c792f578))

- (refactor) Add `in_yield_allowed_context` to `SemanticSyntaxContext` trait ([1d0e43b](https://github.com/astral-sh/ruff/pull/18008/commits/1d0e43b2a963df255c84e93e516657f1da2ca24e))

But since we squash, i'll leave them as is. 



---

_Review requested from @ntBre by @maxmynter on 2025-05-14 20:31_

---

_@ntBre reviewed on 2025-05-14 21:23_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:792 on 2025-05-14 21:23_

We could also add a test case for ruff (is that what you mean by the `Checker` version not being used?)

---

_@ntBre approved on 2025-05-14 21:27_

Thanks, this looks good to me! The only additional thing would be adding a test in ruff. I think it could have been susceptible to the same, or at least similar, initial issue. But the existing tests should at least exercise the new context method anyway.

It might be good to get a quick review from someone on the ty side too, but this is otherwise good to go.

---

_Comment by @maxmynter on 2025-05-15 03:26_

Added tests in [7460049](https://github.com/astral-sh/ruff/pull/18008/commits/7460049b27f8f7c82ce5fbf0c195c0cabb8496a8)

---

_@ntBre approved on 2025-05-15 13:48_

---

_Comment by @maxmynter on 2025-05-17 00:35_

Ping @carljm @dhruvmanila @MichaReiser (because Brent said it would be good if someone from the TY side would review and you're code owners and recent ty committers :) )

---

_Merged by @MichaReiser on 2025-05-21 16:16_

---

_Closed by @MichaReiser on 2025-05-21 16:16_

---
