```yaml
number: 11612
title: Fix all compilation errors
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/compile
created_at: 2024-05-30T06:36:27Z
updated_at: 2024-05-30T11:19:22Z
url: https://github.com/astral-sh/ruff/pull/11612
synced_at: 2026-01-12T15:55:38Z
```

# Fix all compilation errors

---

_@dhruvmanila_

## Summary

This PR fixes all the compilation error as raised by `clippy`. It doesn't look at the tests. Apart from that it also does the following:

* Expose a `lex` method which creates the `Lexer` object. This is used in the benchmark crate. Ideally, we shouldn't expose the lexer and in the future we should move the lexer benchmark in the parser crate
* Update the parser reference in `red_knot` crate 
* Change all references of `&[LexResult]` to `&Tokens`
* Add `CommentRanges` to the `LinterResult` as it's required after the linter run. The `Program` is consumed because it requires an owned `ParseError`


---

_Comment by @codspeed-hq[bot] on 2024-05-30 06:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/compile)

### Merging #11612 will **not alter performance**

<sub>Comparing <code>dhruv/compile</code> (39a9845) with <code>dhruv/compile</code> (b422554)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@dhruvmanila reviewed on 2024-05-30 10:31_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/linter.rs`:211 on 2024-05-30 10:31_

I'm not sure if it makes sense to consume the `Program` and include `CommentRanges` to `LinterResult`. I'm thinking of just cloning the `ParseError` here, it shouldn't affect performance at all.

---

_@dhruvmanila reviewed on 2024-05-30 10:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/test.rs`:179 on 2024-05-30 10:32_

Um, this is actually a bug. Sorry, it's difficult to separate all this changes very carefully

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1876 on 2024-05-30 10:33_

We need to expose the lexer because the benchmark crate requires it.

---

_Review comment by @dhruvmanila on `crates/ruff_python_trivia_integration_tests/tests/simple_tokenizer.rs`:26 on 2024-05-30 10:33_

We're only interested in the comment ranges and the source can contain syntax errors.

---

_Review comment by @dhruvmanila on `crates/ruff_wasm/src/lib.rs`:265 on 2024-05-30 10:34_

This is just maintaining the existing behavior where the playground displays the token stream even if it contains syntax errors.

---

_@dhruvmanila reviewed on 2024-05-30 10:34_

---

_Marked ready for review by @dhruvmanila on 2024-05-30 10:34_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-30 10:34_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/print_tokens.rs`:29 on 2024-05-30 10:49_

Could we implement `IntoIter` for `&Tokens`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:211 on 2024-05-30 10:50_

Yeah we could do that too. Or you could consider adding a `take_errors` method to `Program`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/stylist.rs`:215 on 2024-05-30 10:55_

What's the reason for changing the expected output in this test?

---

_@MichaReiser approved on 2024-05-30 10:55_

---

_@dhruvmanila reviewed on 2024-05-30 10:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_codegen/src/stylist.rs`:215 on 2024-05-30 10:57_

This is coming from `main` via https://github.com/astral-sh/ruff/commit/bd46cd1fcf3dbc0da1cff763d061293c9bc09087 but I failed to recognize that while rebasing.

---

_Merged by @dhruvmanila on 2024-05-30 11:14_

---

_Closed by @dhruvmanila on 2024-05-30 11:14_

---

_Branch deleted on 2024-05-30 11:14_

---
