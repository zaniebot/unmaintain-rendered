```yaml
number: 21034
title: "[`ISC001`] fix panic when string literals are unclosed"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-isc001-unterminated-literal
created_at: 2025-10-22T18:56:33Z
updated_at: 2025-10-28T19:18:52Z
url: https://github.com/astral-sh/ruff/pull/21034
synced_at: 2026-01-12T15:57:15Z
```

# [`ISC001`] fix panic when string literals are unclosed

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #21023

Return None from trailing_quote for single-quote tokens

## Test Plan

<!-- How was it tested? -->

I created `crates/ruff_linter/resources/test/fixtures/flake8_implicit_str_concat/ISC_syntax_error_2.py`.


---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:311 on 2025-10-22 19:09_

I'll have a closer look tomorrow but I wonder if we could change the rule instead to get the quote style from the token flags (and/or not call this function if the string token is unterminated)

---

_@MichaReiser reviewed on 2025-10-22 19:09_

---

_Comment by @github-actions[bot] on 2025-10-22 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@TaKO8Ki reviewed on 2025-10-22 19:28_

---

_Review comment by @TaKO8Ki on `crates/ruff_python_ast/src/str.rs`:311 on 2025-10-22 19:28_

I guess it's possible. The problem is `concatenate_strings` that calls `trailing_quote`, so If we check the string flags `a_token` and `b_token` and avoid calling `concatenate_strings` when there’s an issue, it will resolve the problem.

https://github.com/astral-sh/ruff/blob/735c3e087e6a31dd45e8507007a6743348a94113/crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs#L170

---

_@MichaReiser reviewed on 2025-10-23 06:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:311 on 2025-10-23 06:24_

I think I'd change `concatenate_strings` to take a `Token` instead and make it return `None` if `a_range.string_flags()?` is `None`. I'm also leaning towards not providing a fix when either of the strings are uncloded, but I could be convinced that we should still provide a fix.

Using `StringFlags` has the benefit that it also works for triple quoted strings (where the `content.len() <= 1 check isn't sufficient)

---

_@TaKO8Ki reviewed on 2025-10-28 12:24_

---

_Review comment by @TaKO8Ki on `crates/ruff_python_ast/src/str.rs`:311 on 2025-10-28 12:24_

I updated `concatenate_strings` to validate `token.string_flags`.

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-10-28 12:24_

---

_Renamed from "[`syntax-error`] Return None from trailing_quote for single-quote tokens" to "[`ISC001`] fix panic when string literals are unclosed" by @MichaReiser on 2025-10-28 19:06_

---

_Label `bug` added by @MichaReiser on 2025-10-28 19:06_

---

_Merged by @MichaReiser on 2025-10-28 19:14_

---

_Closed by @MichaReiser on 2025-10-28 19:14_

---
