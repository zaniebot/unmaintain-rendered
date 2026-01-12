```yaml
number: 11529
title: "Update token-based rules to use `Tokens`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/token-rules
created_at: 2024-05-24T10:43:20Z
updated_at: 2024-05-29T05:48:42Z
url: https://github.com/astral-sh/ruff/pull/11529
synced_at: 2026-01-12T15:55:38Z
```

# Update token-based rules to use `Tokens`

---

_@dhruvmanila_

## Summary

This PR updates the token-based rules to use the `Tokens` struct instead of the iterator approach used earlier.

This also adds a new method on `Tokens`:
* `as_tuple` which returns a tuple of `(TokenKind, TextRange)`


---

_Label `internal` added by @dhruvmanila on 2024-05-24 10:43_

---

_Marked ready for review by @dhruvmanila on 2024-05-28 04:59_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-28 04:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:30 on 2024-05-28 07:12_

What's the motivation for renaming the type here? I'm asking because I find `RuleToken` a somewhat awkward name. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:433 on 2024-05-28 07:14_

Are the fields on Token public? Could we instead write
```suggestion
        while let Some(Token { kind, range }) = self.tokens.next() {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:409 on 2024-05-28 07:16_

I remember that the implementation here is somewhat performance sensitive. I wonder if we should instead use the slice iterator directly and use an internal `peek` method that clones the slice and calls next on it to avoid the overhead of testing if `peeked` is set in every iteration.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/compound_statements.rs`:219 on 2024-05-28 07:18_

Nit: Maybe consider storing the `range` in `colon` (and the other fields) instead of a `start` `end` tuple.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/compound_statements.rs`:288 on 2024-05-28 07:20_

This is unrelated to this PR but I wonder if we can use a single variable to track all clauses with a body (`case`, `class`, `elif`, ...) since we always test them at once and reset them at once.

---

_@MichaReiser approved on 2024-05-28 07:21_

---

_@dhruvmanila reviewed on 2024-05-28 08:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:30 on 2024-05-28 08:24_

Mainly to avoid conflicts with the actual `Token` stored on the program.

---

_@dhruvmanila reviewed on 2024-05-28 08:25_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:433 on 2024-05-28 08:25_

No, currently they're private. I guess it's fine to make it public? Probably not in this PR though

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:409 on 2024-05-29 05:18_

Yeah, I can look into it. I basically did that with the `TokenKindIter` :)

---

_@dhruvmanila reviewed on 2024-05-29 05:18_

---

_@dhruvmanila reviewed on 2024-05-29 05:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:30 on 2024-05-29 05:32_

What about `SimpleToken` ?

---

_@dhruvmanila reviewed on 2024-05-29 05:48_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:30 on 2024-05-29 05:48_

I went with `SimpleToken` but feel free to suggest something else.

---

_Merged by @dhruvmanila on 2024-05-29 05:48_

---

_Closed by @dhruvmanila on 2024-05-29 05:48_

---

_Branch deleted on 2024-05-29 05:48_

---
