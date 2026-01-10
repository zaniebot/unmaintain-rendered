```yaml
number: 17300
title: "[syntax-errors] `return` outside function"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: main
head: brent/syn-return-outside-function
created_at: 2025-04-08T20:14:23Z
updated_at: 2025-04-11T17:12:07Z
url: https://github.com/astral-sh/ruff/pull/17300
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] `return` outside function

---

_Pull request opened by @ntBre on 2025-04-08 20:14_

Summary
--

This PR reimplements [return-outside-function (F706)](https://docs.astral.sh/ruff/rules/return-outside-function/) as a semantic syntax error.

These changes are very similar to those in https://github.com/astral-sh/ruff/pull/17298.

Test Plan
--

New linter tests, plus existing F706 tests.

---

_Label `rule` added by @ntBre on 2025-04-08 20:14_

---

_Comment by @github-actions[bot] on 2025-04-08 20:28_

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

_Marked ready for review by @ntBre on 2025-04-08 20:59_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-08 20:59_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-08 20:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:257 on 2025-04-08 21:47_

nit: I think the function names don't matter here so would prefer to just keep it `_`

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-08 21:50_

What is the plan for this and other related semantic syntax errors that are implemented as rules? Should this be converted as other semantic syntax errors and deprecate the rules when stabilizing these syntax errors?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:127 on 2025-04-08 21:51_

As stated in the other PR, I'd prefer if we can re-use the scope information from the `Checker`.

---

_@dhruvmanila reviewed on 2025-04-08 21:51_

---

_@ntBre reviewed on 2025-04-08 21:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-08 21:59_

I think the long-term plan is to deprecate these as rules, but I'm not sure if stabilization will happen too soon for that deprecation to be included. I'm interested in other thoughts on that!

The main benefit of reimplementing them like this is that red-knot will be able to reuse the syntax error versions, regardless of how they continue to be used in ruff.

But in short, yes, this whole function should eventually just be `self.semantic_errors.borrow_mut().push(error);`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:606 on 2025-04-08 22:02_

Yeah, that makes sense, thanks.

---

_@dhruvmanila reviewed on 2025-04-08 22:02_

---

_@dhruvmanila approved on 2025-04-08 22:02_

---

_@MichaReiser approved on 2025-04-09 06:42_

---

_Comment by @ntBre on 2025-04-11 16:01_

Could I get one more quick review here? Like in https://github.com/astral-sh/ruff/pull/17298, I reverted the parser snapshot changes and moved everything to a linter test. It's much, much smaller now.

The function scope tracking is now on the `Context` too, of course.

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-11 16:01_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-11 16:01_

---

_@MichaReiser approved on 2025-04-11 16:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/syntax_errors/return_outside_function.py`:1 on 2025-04-11 16:47_

nit: maybe run `ruff format` on this file?

---

_@dhruvmanila approved on 2025-04-11 16:47_

---

_Merged by @ntBre on 2025-04-11 17:05_

---

_Closed by @ntBre on 2025-04-11 17:05_

---

_Branch deleted on 2025-04-11 17:05_

---
