```yaml
number: 20051
title: "[syntax-errors] Detect `yield from` inside async function"
type: pull_request
state: merged
author: 11happy
labels:
  - ty
assignees: []
merged: true
base: main
head: yield_from_in_async
created_at: 2025-08-22T19:39:45Z
updated_at: 2025-09-03T14:13:06Z
url: https://github.com/astral-sh/ruff/pull/20051
synced_at: 2026-01-10T17:46:21Z
```

# [syntax-errors] Detect `yield from` inside async function

---

_Pull request opened by @11happy on 2025-08-22 19:39_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements https://docs.astral.sh/ruff/rules/yield-from-in-async-function/  as a syntax semantic error

## Test Plan

<!-- How was it tested? -->
I have written a simple inline test as directed in [https://github.com/astral-sh/ruff/issues/17412](https://github.com/astral-sh/ruff/issues/17412)


---

_Review requested from @MichaReiser by @11happy on 2025-08-22 19:39_

---

_Review requested from @dhruvmanila by @11happy on 2025-08-22 19:39_

---

_Review requested from @ntBre by @ntBre on 2025-08-22 19:55_

---

_Comment by @ntBre on 2025-08-22 21:13_

Thanks for working on this! It looks like there are a few unrelated test failures in the CI results. The inline tests also produce a couple of files that have to be checked in. Let me know if you need help resolving either of those issues!

---

_Label `ty` added by @ntBre on 2025-08-22 21:14_

---

_Comment by @ntBre on 2025-08-22 21:15_

(labeling this as `ty` since it shouldn't cause any external behavior to change in Ruff but will be a new syntax error detected in ty)

---

_Review requested from @carljm by @11happy on 2025-08-23 12:36_

---

_Review requested from @AlexWaygood by @11happy on 2025-08-23 12:36_

---

_Review requested from @sharkdp by @11happy on 2025-08-23 12:36_

---

_Review requested from @dcreager by @11happy on 2025-08-23 12:36_

---

_Comment by @11happy on 2025-08-23 12:49_

Have updates to fix the CI, however this test is failing
```
thread 'valid_syntax' panicked at crates/ruff_python_parser/tests/fixtures.rs:119:9:
"/home/happy/ruff/crates/ruff_python_parser/resources/valid/expressions/arguments.py": Expected no semantic syntax errors for a valid program:
   |
29 | # Yield expression
30 | call((yield x))
31 | call((yield from x))
   |       ^^^^^^^^^^^^ Syntax Error: `yield from` statement in async function; use `async for` instead
32 |
33 | # Named expression
```
file https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_parser/resources/valid/expressions/arguments.py

---

_@AlexWaygood had review dismissed on 2025-08-23 18:04_

Thanks for the PR!

I fixed the issue in ty's tests (just a pre-existing bug in the test that your PR exposed!), and checked in the snapshot for your new test.

It looks like there are a bunch of changes to existing ruff_python_parser snapshots too, though, and I'm not sure if they're correct. Could you take a look? You can review the changes to the snapshots locally by running `cargo test -p ruff_python_parser` and then running `cargo insta review`. `cargo insta review` will prompt you to take a look at each snapshot change and see whether it looks good or not -- if it does, you can accept the change; if it doesn't, you know you have some more work to do on the PR to fixup the logic somewhere :-)

---

_Comment by @11happy on 2025-08-24 17:27_

I have a couple of doubts :

first: while reviewing snapshots I found this 
```
        241 │+1 | x: *int = 1
        242 │+2 | x: yield a = 1
        243 │+3 | x: yield from b = 1
        244 │+  |    ^^^^^^^^^^^^ Syntax Error: `yield from` statement in async function; use `async for` instead
        245 │+4 | x: y := int = 1
```
Here this `yield from`  syntax error should not be there as its not in a async scope its a module level scope ?
for this I looked at the implementation of `in_async_context`  & I am not perfectly sure how's here the `in_async_context` is returning true, Am I missing something here?
```
fn in_async_context(&self) -> bool {
        for scope_info in self.scope_stack.iter().rev() {
            let scope = &self.scopes[scope_info.file_scope_id];
            match scope.kind() {
                ScopeKind::Class | ScopeKind::Lambda => return false,
                ScopeKind::Function => {
                    return scope.node().expect_function(self.module).is_async;
                }
                ScopeKind::Comprehension
                | ScopeKind::Module
                | ScopeKind::TypeAlias
                | ScopeKind::TypeParams => {}
            }
        }
        false
    }
```

second: should I commit all the updated snapshots ?

Thank you

---

_Comment by @11happy on 2025-08-26 06:47_

Hii @ntBre,
I’d really appreciate your help in moving this PR forward when you get a chance.
Thank you : )

---

_Comment by @ntBre on 2025-08-26 12:55_

I think the test failures are running into a limitation of our inline parser error test suite:

https://github.com/astral-sh/ruff/blob/32cb86351bfadda29a335d6d3aa6bc52585f4bdf/crates/ruff_python_parser/tests/fixtures.rs#L530-L532

You can see there that we've hard-coded `true` in `in_async_context`. We do have some handling for scopes, as you can see in the function just above:

https://github.com/astral-sh/ruff/blob/32cb86351bfadda29a335d6d3aa6bc52585f4bdf/crates/ruff_python_parser/tests/fixtures.rs#L534-L541

So I think you'll also need to add a real implementation of `in_async_context` to avoid the  unrelated test failures. Sorry about that, I guess this is the first rule that needs it. I'm happy to help with this if you run into trouble.

Along these lines, we usually add two other kinds of tests for these errors since they depend on ruff's and ty's implementations of `SemanticSyntaxContext`. You can see an example in  Ruff:

https://github.com/astral-sh/ruff/blob/32cb86351bfadda29a335d6d3aa6bc52585f4bdf/crates/ruff_linter/src/linter.rs#L1185-L1190

and in ty:

https://github.com/astral-sh/ruff/blob/32cb86351bfadda29a335d6d3aa6bc52585f4bdf/crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md?plain=1#L17-L24

> second: should I commit all the updated snapshots ?

Yes, once you get a set of snapshot failures that look correct, you'll need to commit the updated snapshots to pass the tests in CI.

---

_Comment by @11happy on 2025-08-28 17:48_

@ntBre thank you for guiding this PR!

I have updated it accordingly , could you please take a look.
Thank you

---

_Comment by @ntBre on 2025-08-28 18:14_

I'll try to take a look soon! It looks like the representation of  AST nodes changed slightly last week (#20028), you might need to rebase/merge main and update your snapshots once more to get CI passing. There's also one small clippy suggestion.

---

_Comment by @11happy on 2025-08-29 15:43_

have rebased it, CI should pass now

---

_Comment by @11happy on 2025-08-29 16:04_

Updated the snapshot & fixed the clippy suggestion as per new CI results.

---

_Comment by @github-actions[bot] on 2025-08-29 16:20_

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

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__yield_from_in_async_function.py.snap`:1 on 2025-08-29 17:59_

It looks like we're getting duplicate diagnostics. I think there should be some existing PLE1700 code that we need to delete now that it's being detected and reported as a syntax error:

https://github.com/astral-sh/ruff/blob/9b1b58a4513eae2c3cf206eb5fc3aea19380883c/crates/ruff_linter/src/rules/pylint/rules/yield_from_in_async_function.rs#L40-L42

We should be able to delete this function and any callers.

---

_@ntBre reviewed on 2025-08-29 17:59_

Thanks! This looks great overall, we just need to delete the old rule implementation to avoid duplicate diagnostics.

---

_@11happy reviewed on 2025-08-30 06:49_

---

_Review comment by @11happy on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__yield_from_in_async_function.py.snap`:1 on 2025-08-30 06:49_

done

---

_Review requested from @ntBre by @11happy on 2025-09-01 15:26_

---

_Comment by @11happy on 2025-09-02 15:33_

Hi maintainers, please let me know if any further input or changes are required from my end for this PR.

Thank you

---

_@ntBre approved on 2025-09-03 13:58_

Thank you!

---

_Renamed from "[syntax-errors] yield from inside async function" to "[syntax-errors] Detect `yield from` inside async function" by @ntBre on 2025-09-03 14:00_

---

_Merged by @ntBre on 2025-09-03 14:13_

---

_Closed by @ntBre on 2025-09-03 14:13_

---
