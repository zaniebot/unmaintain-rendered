```yaml
number: 20556
title: "[syntax-errors] Reimplement `break-outside-loop` (`F701`) as a semantic syntax error"
type: pull_request
state: merged
author: 11happy
labels:
  - internal
assignees: []
merged: true
base: main
head: F701
created_at: 2025-09-24T16:52:30Z
updated_at: 2025-10-13T20:18:39Z
url: https://github.com/astral-sh/ruff/pull/20556
synced_at: 2026-01-10T17:34:34Z
```

# [syntax-errors] Reimplement `break-outside-loop` (`F701`) as a semantic syntax error

---

_Pull request opened by @11happy on 2025-09-24 16:52_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR implements https://docs.astral.sh/ruff/rules/break-outside-loop/ (F701) as a semantic syntax error.

## Test Plan

<!-- How was it tested? -->


---

_@ntBre reviewed on 2025-09-24 17:15_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:656 on 2025-09-24 17:15_

We shouldn't be recursing here. The `SemanticSyntaxChecker` isn't a full `Visitor`, it only checks the single `Stmt` it's given and relies on the parent visitor (either `Checker` in Ruff or `SemanticIndexBuilder` in ty) to drive the AST traversal.

I think this also means that managing the loop state isn't really feasible in the `SemanticSyntaxChecker` itself. Instead, we'll probably need to add `in_loop_context` as a method on the `SemanticSyntaxContext` trait and implement it for Ruff and ty (and the test visitor if we want to write inline tests).

You could also consider trying to add a `SemanticSyntaxContext::current_statements()` method, which would be more general and match how Ruff currently implements this check:

https://github.com/astral-sh/ruff/blob/83f80effecf682a487abfcd101ea6bbab3471740/crates/ruff_linter/src/checkers/ast/analyze/statement.rs#L54-L61

Those are just some ideas based on a quick look, hope that helps!

---

_@11happy reviewed on 2025-09-24 17:25_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:656 on 2025-09-24 17:25_

Sure, Thank you for these pointers, I will try them out & keep you updated : )

---

_Comment by @github-actions[bot] on 2025-10-05 13:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-05 13:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@11happy reviewed on 2025-10-05 13:20_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:656 on 2025-10-05 13:20_

done, I have added a new `current_statements` .

Thank you : )

---

_Marked ready for review by @11happy on 2025-10-05 13:21_

---

_Review requested from @carljm by @11happy on 2025-10-05 13:21_

---

_Review requested from @AlexWaygood by @11happy on 2025-10-05 13:21_

---

_Review requested from @sharkdp by @11happy on 2025-10-05 13:21_

---

_Review requested from @dcreager by @11happy on 2025-10-05 13:21_

---

_Review requested from @MichaReiser by @11happy on 2025-10-05 13:21_

---

_Review requested from @dhruvmanila by @11happy on 2025-10-05 13:21_

---

_Comment by @github-actions[bot] on 2025-10-05 13:28_

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

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:1 on 2025-10-06 19:26_

I don't think we should delete this test case. We still need to emit F701, even if the main implementation is as a syntax error.

---

_Review comment by @ntBre on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2781 on 2025-10-06 19:27_

```suggestion
    
    fn in_loop_context(&self) -> bool {
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:676 on 2025-10-06 19:27_

```suggestion
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:823 on 2025-10-06 19:30_

I don't think we need to track a separate `Vec` of current statements on the `Checker`. This should already be available on the `SemanticModel` field, as in the code I linked earlier.


```suggestion
        let statements = self.semantic.current_statements();
```

My earlier suggestion about adding a `current_statements` method was not for `Checker`, I meant that instead of `SemanticSyntaxContext::in_loop_context`, we could consider adding `SemanticSyntaxContext::current_statements`, which would return an iterator over `Stmt` nodes:

```rust
fn current_statements(&self) -> impl Iterator<Item = &Stmt> {
    self.semantic.current_statements()
}
```

This should be easier to implement for both Ruff and ty, and then the actual `in_loop_context` check could be shared between the two. But I think `SemanticSyntaxContext::in_loop_context` is also okay, especially if you tried this already.

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:830 on 2025-10-06 19:32_

I think we should just pass the current statement in, like in the original F701 implementation, to avoid needing to `unwrap` here.

---

_@ntBre requested changes on 2025-10-06 19:38_

Thanks! But I think we can make a few improvements here.

---

_@11happy reviewed on 2025-10-11 15:56_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:1 on 2025-10-11 15:56_

done

---

_@11happy reviewed on 2025-10-11 15:56_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:830 on 2025-10-11 15:56_

done

---

_@11happy reviewed on 2025-10-11 15:56_

---

_Review comment by @11happy on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2781 on 2025-10-11 15:56_

done

---

_@11happy reviewed on 2025-10-11 15:56_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:676 on 2025-10-11 15:56_

done

---

_@ntBre approved on 2025-10-13 19:56_

Thank you! I pushed a couple of very small cleanups, but this looks good to me now.

---

_Label `internal` added by @ntBre on 2025-10-13 19:57_

---

_Renamed from "[syntax-errors]: break outside loop F701" to "[syntax-errors]: Reimplement `break-outside-loop` (`F701`) as a semantic syntax error" by @ntBre on 2025-10-13 19:57_

---

_Renamed from "[syntax-errors]: Reimplement `break-outside-loop` (`F701`) as a semantic syntax error" to "[syntax-errors] Reimplement `break-outside-loop` (`F701`) as a semantic syntax error" by @ntBre on 2025-10-13 19:57_

---

_Merged by @ntBre on 2025-10-13 20:00_

---

_Closed by @ntBre on 2025-10-13 20:01_

---
