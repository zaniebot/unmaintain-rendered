```yaml
number: 17363
title: "[syntax-errors] `await` outside async functions"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/syn-await-outside-async-function
created_at: 2025-04-11T19:26:45Z
updated_at: 2025-04-22T14:48:39Z
url: https://github.com/astral-sh/ruff/pull/17363
synced_at: 2026-01-10T19:33:02Z
```

# [syntax-errors] `await` outside async functions

---

_Pull request opened by @ntBre on 2025-04-11 19:26_

Summary
--

This PR implements detecting the use of `await` expressions outside of async functions. This is a reimplementation of
[await-outside-async (PLE1142)](https://docs.astral.sh/ruff/rules/await-outside-async/) as a semantic syntax error.

Despite the rule name, PLE1142 also applies to `async for` and `async with`, so these are covered here too.

Test Plan
--

Existing PLE1142 tests.

I also deleted more code from the `SemanticSyntaxCheckerVisitor` to avoid changes in other parser tests.


---

_Label `rule` added by @ntBre on 2025-04-11 19:26_

---

_Comment by @mikeshardmind on 2025-04-11 19:35_

This one might not belong in syntax-errors and might be better to remain as a disableable rule. Top level await is allowed if the module is compiled with https://docs.python.org/3/library/ast.html#ast.PyCF_ALLOW_TOP_LEVEL_AWAIT. This is useful when working with pyodide, as modules can assume an event loop is running prior to them being imported.

---

_Comment by @github-actions[bot] on 2025-04-11 19:36_

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

_Comment by @ntBre on 2025-04-11 19:42_

> This one might not belong in syntax-errors and might be better to remain as a disableable rule. Top level await is allowed if the module is compiled with https://docs.python.org/3/library/ast.html#ast.PyCF_ALLOW_TOP_LEVEL_AWAIT. This is useful when working with pyodide, as modules can assume an event loop is running prior to them being imported.

Oh interesting, thank you! Is there any indication of this option in the file itself, or is this passed externally to the compiler?

This doesn't _necessarily_ need to block this PR because we're still converting the syntax error to a suppressible PLE1142 diagnostic, but we will definitely need to consider this before deprecating the rule or emitting this as a pure syntax error.

---

_Marked ready for review by @ntBre on 2025-04-11 19:43_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-11 19:43_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-11 19:43_

---

_Comment by @mikeshardmind on 2025-04-11 19:47_

>  Is there any indication of this option in the file itself, or is this passed externally to the compiler?

Not visible from the file itself. It can be passed as an option to compile, which can be done in a module meta path finder. It's a little advanced use,, so I'm not sure how many projects currently or in the future will do anything with it. The builtin asyncio repl (python -m asyncio) uses it.

---

_Comment by @dhruvmanila on 2025-04-11 21:44_

> Despite the rule name, PLE1142 also applies to `async for` and `async with`, so these are covered here too.

Interesting. I think we could improve the diagnostic message by replacing "`await`" with "`async with`" and "`async for`" for those cases.

---

_Comment by @dhruvmanila on 2025-04-11 21:46_

> This doesn't _necessarily_ need to block this PR because we're still converting the syntax error to a suppressible PLE1142 diagnostic, but we will definitely need to consider this before deprecating the rule or emitting this as a pure syntax error.

Yeah, maybe we should label this and other PRs as "internal" because there are no user-facing changes except for if we decide to update the error message but that could be done in a follow-up to keep it isolated. What do you think?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:864 on 2025-04-11 21:50_

We could use statement specific error message here as an improvement

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1646 on 2025-04-11 21:53_

Should we instead have a `scope` method and keep this and other related checks as methods to `SemanticSyntaxChecker` or do you think it'll be an issue with `SemanticSyntaxCheckerVisitor` ?

---

_@dhruvmanila approved on 2025-04-11 21:53_

---

_@ntBre reviewed on 2025-04-11 22:10_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1646 on 2025-04-11 22:10_

What return type are you picturing for the `scope` method?

---

_Label `internal` added by @ntBre on 2025-04-11 22:16_

---

_Label `rule` removed by @ntBre on 2025-04-11 22:16_

---

_@ntBre reviewed on 2025-04-11 22:17_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:864 on 2025-04-11 22:17_

That's a good idea. This string isn't actually used yet since the error is converted to the normal ruff `Diagnostic`, which just says `await` in every case, but we might as well update it for the future.

---

_@dhruvmanila reviewed on 2025-04-11 23:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1646 on 2025-04-11 23:44_

Ah right, I don't think that's possible because of circular dependency. Ignore this comment.

---

_Comment by @ntBre on 2025-04-14 16:38_

I added an `AwaitOutsideAsyncFunctionKind` enum for better error messages in the future, and also noticed that I forgot to wire up the check for dictionary comprehensions. I added some tests for these specifically and copied over the loop from set and list comprehensions.

---

_Merged by @ntBre on 2025-04-14 17:01_

---

_Closed by @ntBre on 2025-04-14 17:01_

---

_Branch deleted on 2025-04-14 17:01_

---

_@dhruvmanila reviewed on 2025-04-22 14:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:864 on 2025-04-22 14:44_

@ntBre Do you mind opening an issue for this to make sure it's tracked to be updated in the future?

---

_@ntBre reviewed on 2025-04-22 14:48_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:864 on 2025-04-22 14:48_

It's used now in red-knot! [Here](https://github.com/astral-sh/ruff/blob/bfbc79f5361cce361583ab0739333306f46fe0df/crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md#await-outside-async-function)

or it will be once #17463 lands at least. Do the messages look like what you had in mind?

---
