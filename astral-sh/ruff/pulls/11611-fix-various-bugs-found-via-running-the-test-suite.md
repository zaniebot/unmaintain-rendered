```yaml
number: 11611
title: Fix various bugs found via running the test suite
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/bugfixes
created_at: 2024-05-30T06:36:04Z
updated_at: 2024-05-30T10:52:11Z
url: https://github.com/astral-sh/ruff/pull/11611
synced_at: 2026-01-10T21:56:00Z
```

# Fix various bugs found via running the test suite

---

_Pull request opened by @dhruvmanila on 2024-05-30 06:36_

## Summary

This PR fixes various bugs found when running the test suite. Note that it doesn't update the code to make it compile which is done in a separate PR.

Refer to review comments for each individual bug.

---

_Label `bug` added by @dhruvmanila on 2024-05-30 06:36_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1163 on 2024-05-30 10:07_

The function requires the range and the right side of the binary expression so we let's just pass the binary expression as a whole.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:116 on 2024-05-30 10:07_

Must've been a copy paste mistake.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:478 on 2024-05-30 10:10_

I think this isn't a bug fix, but mostly a change from:
```rs
if matches!(self.tokens.peek(), Some((TokenKind::Def, _)))
```
To
```rs
                            if self
                                .tokens
                                .peek()
                                .is_some_and(|token| token.kind() == TokenKind::Def)
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyflakes/rules/invalid_literal_comparisons.rs`:114 on 2024-05-30 10:12_

The reason this offset exists was because the lexer function used was `lex` instead of `lex_starts_at`.

https://github.com/astral-sh/ruff/blob/b0a751012e2990f86ff58b17157bf86c73e9a64c/crates/ruff_linter/src/rules/pyflakes/rules/invalid_literal_comparisons.rs#L152-L157

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/fixes.rs`:28 on 2024-05-30 10:14_

Now, we use the locator containing the entire source code, so the start offset should be the statement start offset.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/fixes.rs`:61 on 2024-05-30 10:14_

Similarly, the final string piece should use the end offset from the statement range.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:446 on 2024-05-30 10:15_

This should use the start of the entire binary expression instead of just the string expression. For example,

```py
  ("%d" "%d") % (1, 2)
# ^^^^^^^^^^^^^^^^^^^^ binary expression
#  ^^^^^^^^^ string expression
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:486 on 2024-05-30 10:16_

Same as above.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/context.rs`:17 on 2024-05-30 10:18_

Indicate that the lifetime of tokens is independent of the context similar to the source lifetime. This allows `LogicalLineIter` to get the `Tokens` reference along with passing the mutable reference to the context to `fmt`.

---

_Review comment by @dhruvmanila on `crates/ruff_python_index/src/multiline_ranges.rs`:52 on 2024-05-30 10:19_

The range should be of `FStringMiddle`, not `FStringStart`.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:186 on 2024-05-30 10:19_

Oops! Forgot to bump the cursor.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:580 on 2024-05-30 10:20_

Ummmm

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:741 on 2024-05-30 10:20_

Yeah, not sure why I did this xD

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1467 on 2024-05-30 10:21_

Using `contains` is incorrect here.

For all other changes, I find `intersects` easier to reason about.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:152 on 2024-05-30 10:22_

The `EndOfFile` token shouldn't be included in the token stream, it's just to indicate the parser to stop. This isn't in `bump` because it only needs to be done once.

---

_@dhruvmanila reviewed on 2024-05-30 10:22_

---

_Marked ready for review by @dhruvmanila on 2024-05-30 10:22_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-30 10:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:116 on 2024-05-30 10:41_

That must have been annoying to find.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:478 on 2024-05-30 10:43_

Yeah, I think this is more a compilation error fix?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/verbatim.rs`:795 on 2024-05-30 10:44_

Nit: I think `'_` should just work fine here but there's no need to change it.

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/multiline_ranges.rs`:52 on 2024-05-30 10:45_

Is this a bug in main too or something you introduced by your refactor?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:152 on 2024-05-30 10:46_

I think this comment would be useful in the source. I personally wouldn't mind if the EOF token is in the token stream but I don't feel strongly (and I wouldn't make the change as part of this stack if it changes too many snapshot tests)

---

_@MichaReiser approved on 2024-05-30 10:46_

---

_@dhruvmanila reviewed on 2024-05-30 10:48_

---

_Review comment by @dhruvmanila on `crates/ruff_python_index/src/multiline_ranges.rs`:52 on 2024-05-30 10:48_

The later, it was introduced by me

---

_Merged by @dhruvmanila on 2024-05-30 10:52_

---

_Closed by @dhruvmanila on 2024-05-30 10:52_

---

_Branch deleted on 2024-05-30 10:52_

---
