```yaml
number: 17177
title: "[syntax-errors] Async comprehension in sync comprehension"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-async-comprehensions
created_at: 2025-04-03T14:21:21Z
updated_at: 2025-04-08T16:50:53Z
url: https://github.com/astral-sh/ruff/pull/17177
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Async comprehension in sync comprehension

---

_Pull request opened by @ntBre on 2025-04-03 14:21_

Summary
--

Detect async comprehensions nested in sync comprehensions in async functions before Python 3.11, when this was [changed].

The actual logic of this rule is very straightforward, but properly tracking the async scopes took a bit of work. An alternative to the current approach is to offload the `in_async_context` check into the `SemanticSyntaxContext` trait, but that actually required much more extensive changes to the `TestContext` and also to ruff's semantic model, as you can see in the changes up to 31554b473507034735bd410760fde6341d54a050. This version has the benefit of mostly centralizing the state tracking in `SemanticSyntaxChecker`, although there was some subtlety around deferred function body traversal that made the changes to `Checker` more intrusive too (hence the new linter test).

The `Checkpoint` struct/system is obviously overkill for now since it's only tracking a single `bool`, but I thought it might be more useful later.

[changed]: https://github.com/python/cpython/issues/77527

Test Plan
--

New inline tests and a new linter integration test.

---

_Label `rule` added by @ntBre on 2025-04-03 14:21_

---

_Label `preview` added by @ntBre on 2025-04-03 14:21_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-03 14:21_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-03 14:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:338 on 2025-04-03 16:27_

Do we need to store the `checkpoint` for every statement or could we only store and restore it for some statements (e.g functions, lambdas, classes?)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:50 on 2025-04-03 16:31_

I wonder if we could avoid having to store the checkpoints inside and instead move it to the stack by changing `SemanticSyntaxChecker::enter_stmt` to return a state which the caller needs to pass to `exit_stmt` (consumes the state, the state can't be created externally).

The checker would always store the current state and `enter_stmt` would return the *previous* state from `enter_stmt` (but update its internal state). The `exit_stmt` call (or expression) then restores the internal state.

The downside of this is that it requires some more bookkeeping at the call site, but it seems fairly reasonable.

This approach would mirror the Checker's approach with updating `SemanticModelFlags`, where it updates the flags before walking the children and then restores the flags once it's done.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:530 on 2025-04-03 16:33_

Can we move the python version check outside the loop. Maybe return early if the python version is newer. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:477 on 2025-04-03 16:36_

Do we need to set `in_async_context` to false when entering a `Class`, `lambda`, or a sync with context? 

---

_@MichaReiser reviewed on 2025-04-03 16:42_

Do you think it would simplify the implementation if we only tracked whether we're in a sync comprehension rather than tracking if we're in an async context? That seems easier than correctly tracking if we're also in an async function. It has the downside that we might emit one extra diagnostic if someone uses an async comprehension in a sync function, but I'd expect this to be rare. On the other hand, it doesn't add that much complexity. It's just a bit tricky to get the handling right when to unset the `async` flag. Do you know if it's necessary to unset the flag in a sync context manager (in an async function)?


---

_@ntBre reviewed on 2025-04-03 16:48_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:530 on 2025-04-03 16:48_

Ah yes, of course. Thanks!

---

_@MichaReiser reviewed on 2025-04-03 16:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:50 on 2025-04-03 16:57_

If you decide to keep the stack solution, we should then change the implementation to only snapshot and restore when necessary. For example, it isn't necessary to store the snapshot for most expressions because the state won't be changed (and, therefore, there isn't anything to restore either). 

---

_@ntBre reviewed on 2025-04-03 17:05_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:477 on 2025-04-03 17:05_

Oh good catch. It looks like `class` and `lambda` do cause problems even inside of an outer `async` function, but `with` doesn't seem to add a sync scope:

```pycon
Python 3.13.2 (main, Feb  5 2025, 08:05:21) [GCC 14.2.1 20250128] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> async def foo(): lambda: [_ async for n in range(3)]
  File "<python-input-4>", line 1
    async def foo(): lambda: [_ async for n in range(3)]
                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: asynchronous comprehension outside of an asynchronous function
>>> async def foo():
...     class C: [_ async for n in range(3)]
...
  File "<python-input-7>", line 2
    class C: [_ async for n in range(3)]
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: asynchronous comprehension outside of an asynchronous function
>>> async def foo():
...     with open("foo.txt", "w") as f:
...         [_ async for n in range(3)]
...
>>>
```

However, these are both still errors on 3.13, so this might be a separate, version-independent rule.

This is making me think we should go back to the trait implementation and defer to the full visitor's scope-based implementation. Or at least try tracking nested scopes here.

---

_@ntBre reviewed on 2025-04-03 17:09_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:477 on 2025-04-03 17:09_

We do have a lint rule that detects these, PLE1142, but not an error.

https://play.ruff.rs/f0be27e4-9cb1-4bdb-9ebe-b095d81815e5

---

_Comment by @ntBre on 2025-04-03 17:26_

> Do you think it would simplify the implementation if we only tracked whether we're in a sync comprehension rather than tracking if we're in an async context? That seems easier than correctly tracking if we're also in an async function. It has the downside that we might emit one extra diagnostic if someone uses an async comprehension in a sync function, but I'd expect this to be rare. On the other hand, it doesn't add that much complexity. It's just a bit tricky to get the handling right when to unset the `async` flag. Do you know if it's necessary to unset the flag in a sync context manager (in an async function)?

This sounded very promising at first, but I think we would still need the checkpoints, and it would only remove the function-specific code, which I don't think adds that much "complexity" per se, like you said. Although I guess we could avoid statement checkpoints and `exit_stmt` entirely since everything else is an expression.

On the other hand, it sounds like PLE1142 should also be a syntax error and needs even more careful scope tracking, so it might be worth setting up for that here.

I'll work on your suggestions about removing the checkpoint stack and see how it looks after that.

As I said in a reply above, we could also give the trait approach another try. That just gets annoying for inline testing. It requires a lot of new test code, and that test code doesn't really guarantee anything about the actual users. However, that's also somewhat the case here because of the tricky enter/exit calls in `Checker`.

---

_@ntBre reviewed on 2025-04-03 18:05_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:50 on 2025-04-03 18:05_

Good idea, it really wasn't bad to plumb this through at the call sites.

---

_Comment by @github-actions[bot] on 2025-04-03 18:19_

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

_Comment by @ntBre on 2025-04-03 19:50_

The ecosystem check is showing a false positive in a notebook cell, which I think should have an implicit async scope. It's probably also picking up a default python version since it mentions 3.9.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:24 on 2025-04-03 21:10_

We could potentially include a [`DebugDropBomb`](https://docs.rs/drop_bomb/latest/drop_bomb/) to make sure that the consumer of the enter API don't forgot to call the `exit_stmt` / `exit_expr` methods which will defuse the bomb.

---

_@dhruvmanila reviewed on 2025-04-03 21:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-04 06:49_

I don't understand the reasoning for skipping function definitions here. Won't this result in stale `in_async_context` flags because we don't update the state until after we visited the entire body? 

If there's a need for deferred visiting, then I'd prefer to have a `enter_deferred_stmt` or, better, move the necessary checks into `exit_stmt` and also pass the statement and context because `exit` is exactly the hook called **after** visiting the node's children

---

_@MichaReiser reviewed on 2025-04-04 06:54_

---

_@ntBre reviewed on 2025-04-04 12:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-04 12:17_

I think I might be missing something here, but my reasoning for this was that the `ast::Checker` doesn't actually visit the body of the function until `visit_deferred_function`:

https://github.com/astral-sh/ruff/blob/5cee34674472b976998ee79683f08dcd2fde090a/crates/ruff_linter/src/checkers/ast/mod.rs#L2588-L2591

Before I added this logic, the function body was being visited twice by the `SemanticSyntaxChecker` but only once by the `ast::Checker` and this integration test was failing even though the inline version passed:

```python
async def test(): return [[x async for x in elements(n)] async for n in range(3)]
```

which pointed to an issue in the visit order because the test visitor is much simpler:

```rust
    fn visit_stmt(&mut self, stmt: &'_ Stmt) {
        let checkpoint = self.checker.enter_stmt(stmt, &self.context);
        ruff_python_ast::visitor::walk_stmt(self, stmt);
        self.checker.exit_stmt(checkpoint);
    }
```

Again I might be misunderstanding, but I don't think an `enter_deferred_stmt` helps here because we still need this logic in `visit_stmt` itself to avoid duplicating the visit. Unless you mean hiding this logic inside of `SemanticSyntaxChecker::enter_stmt`. I guess that could work if I also update the test visitor to defer function bodies like the real `ast::Checker`.

---

_@ntBre reviewed on 2025-04-04 12:19_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:24 on 2025-04-04 12:19_

Oh interesting, I think I saw some mention of this around the new diagnostics. I could also mark `enter_stmt` as `#[must_use]`, which could help a bit in the same direction.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-04 14:29_

Oh I see. This seems fragile. I wouldn't be aware of this out-of-order visiting when working on the `SemanticSyntaxChecker`. We should at least document the constraints in which the `enter` methods are called. I assumed it would be in semantic visiting order but it seems its in semantic visiting order except for functions which may be deferred (or not, depending on the caller)

---

_@MichaReiser reviewed on 2025-04-04 14:29_

---

_@ntBre reviewed on 2025-04-04 17:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-04 17:46_

Yes, very fragile. I was so confused when the integration test was failing but the inline test was fine, until I realized the function bodies were deferred. I'll work on expanding the docs for this.

---

_Comment by @ntBre on 2025-04-07 12:46_

I made a few improvements here:
- documented when `enter_stmt` should be called. I didn't add anything for `enter_expr` because it doesn't have any of these fragile cases yet
- added `#[must_use]` to both `enter_` methods to require using the returned `Checkpoint`. We could add some kind of drop bomb if we want to be even more sure
- add a `PySourceType` argument to `SemanticSyntaxChecker::new` to avoid notebook false positives (the top level scope should allow async code in notebooks)

---

_@dhruvmanila reviewed on 2025-04-07 18:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:56 on 2025-04-07 18:47_

I think it would be useful to add a test case for this. We could add a notebook similar to https://github.com/astral-sh/ruff/blob/27ecf350d821d0fa147bc5fb1388626990477890/crates/ruff_linter/resources/test/fixtures/pylint/await_outside_async.ipynb and a new `#[test]` function that uses two Python version.

---

_@dhruvmanila approved on 2025-04-07 18:48_

Looks good to me, might want to wait on @MichaReiser as he has the most context for this one.

---

_@ntBre reviewed on 2025-04-07 20:16_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:56 on 2025-04-07 20:16_

Oh good idea! I inlined the notebook contents as a CLI test like I saw in the `checks_notebooks_in_stable` test but happy to move this to a separate file if you prefer. It looks like `crates/ruff/resources/test/fixtures` would be the place to put it?

---

_@dhruvmanila reviewed on 2025-04-07 23:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:56 on 2025-04-07 23:27_

Either is fine. The inlined version (what you have currently) looks good 👍 

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5569 on 2025-04-08 06:54_

It's not quiet clear to me why we need those cli tests. Could some of those tests be parser tests instead? The parser tests already support `# parse_options: {"target-version": "3.7"}`

---

_@MichaReiser approved on 2025-04-08 06:54_

Looks good to me. It would be great if we could move some of the CLI tests to parser tests (maybe not the jupyter one because that would be very noisy unless the out-of-bound parser test have options support)

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:5569 on 2025-04-08 12:24_

We probably don't need *all* of these, but the third and fourth helped to detect bugs in the `enter/exit` pairs and the deferred function body traversal. The first two are probably less valuable, if you want me to trim it down a bit.

---

_@ntBre reviewed on 2025-04-08 12:24_

---

_@MichaReiser reviewed on 2025-04-08 12:27_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5569 on 2025-04-08 12:27_

It would be great if we could narrow them down and maybe document why it's important that they're CLI tests. 

An alternative would be to implement them as custom tests in `ruff_linter` (that instantiate checker). Maybe there's something you can reuse from the linter test infra?

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:5569 on 2025-04-08 15:21_

I adapted the `pyflakes` test runner (as suggested on Discord) and moved all of the CLI tests to `ruff_linter`. They're just in the top-level `linter::tests` module for now, but I'm happy to move them around if there's a better place. I also considered nesting them under `rules/syntax_errors`, for example.

---

_@ntBre reviewed on 2025-04-08 15:21_

---

_@MichaReiser reviewed on 2025-04-08 15:33_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5569 on 2025-04-08 15:33_

I think having them in `linter::tests` is fine. They are integration tests after all

---

_@ntBre reviewed on 2025-04-08 16:18_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:5569 on 2025-04-08 16:18_

Sounds good, thanks! I'll merge then

---

_Merged by @ntBre on 2025-04-08 16:50_

---

_Closed by @ntBre on 2025-04-08 16:50_

---

_Branch deleted on 2025-04-08 16:50_

---
