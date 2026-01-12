```yaml
number: 11494
title: Update parser API to merge lexing and parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/parser-api
created_at: 2024-05-22T10:05:04Z
updated_at: 2024-05-31T13:53:25Z
url: https://github.com/astral-sh/ruff/pull/11494
synced_at: 2026-01-12T15:55:38Z
```

# Update parser API to merge lexing and parsing

---

_@dhruvmanila_

## Summary

This PR updates the parser API within the `ruff_python_parser` crate. It doesn't change any of the references in this PR.

The final API looks like:
```rs
pub fn parse_module(source: &str) -> Result<Program<ModModule>, ParseError> {}

pub fn parse_expression(source: &str) -> Result<Program<ModExpression>, ParseError> {}

pub fn parse_expression_range(
    source: &str,
    range: TextRange,
) -> Result<Program<ModExpression>, ParseError> {}

pub fn parse(source: &str, mode: Mode) -> Result<Program<Mod>, ParseError> {}

// Temporary. The `parse` will replace this function once we update the downstream
// tools to work with programs containing syntax error.
pub fn parse_unchecked(source: &str, mode: Mode) -> Program<Mod> {}

// Same as `parse_unchecked` but using `PySourceType` instead of the `Mode`
pub fn parsed_unchecked_source(source: &str, source_type: PySourceType) -> Program<ModModule> {}
```

Following is a detailed list of changes:
* Make `Program` generic over `T` which can be either `Mod` (enum), `ModModule` or `ModExpression`
	* Add helper methods to cast `Mod` into `ModModule` or `ModExpression`
	* Add helper method `Program::into_result` which converts a `Program<T>` into a `Result<Program<T>, ParseError>` where the `Err` variant contains the first `ParseError`
* Update `TokenSource` to store the comment ranges
	* Parser crate depends on `ruff_python_trivia` because of `CommentRanges`. This struct could possibly be moved in the parser crate itself at the end
* Move from `parse_expression_starts_at` to `parse_expression_range` which parses the source code at the given range using `Mode::Expression`. Unlike the `starts_at` variant, this accepts the entire source code
* Remove all access to the `Lexer`
* Remove all `parse_*` functions which works on the tokens provided by the caller

## Test Plan

The good news is that the tests in `ruff_python_parser` can be run. So,
```
cargo insta test --package ruff_python_parser
```


---

_Renamed from "Make `Lexer` lazy (#11244)" to "Update parser API to merge lexing and parsing" by @dhruvmanila on 2024-05-22 10:05_

---

_Label `parser` added by @dhruvmanila on 2024-05-22 13:49_

---

_Marked ready for review by @dhruvmanila on 2024-05-22 13:50_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-22 13:50_

---

_@dhruvmanila reviewed on 2024-05-22 13:52_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:361 on 2024-05-22 13:52_

The `Tokens` struct will gain new methods as I understand how the lexer APIs are being used by the downstream tools but two APIs have been added:
1. `up_to_first_unknown` - Slice of tokens up to (and excluding) the first unknown token. This is what the linter sees.
2. `tokens_in_range` - This will replace most usages of `lex_starts_at`

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-22 13:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:146 on 2024-05-27 13:40_

What's the sizse of a `TokenSourceCheckpoint` and `ParserCheckpoint` at this point? It seems to becoming somewhat big ;)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:29 on 2024-05-27 13:41_

I'm not entirely sure if we should collect the comments in the parse phase or if it is actually faster to extract them after by simply looping over the tokens where needed. 

Doint it later has the advantage that it doesn't require any rewinding. I'm okay going with this solution but it might be worth testing that the performance difference is between doing it inside of the parser and doing it as a separate pass. I somewhat suspect that both are equally fast, in which case I would probably prefer doing it outside of the parser.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/typing.rs`:27 on 2024-05-27 13:43_

The inline comments are uncommon for Rust. 

It's more common to refer to the arguments using their name.

Parses the value of a string literal `parsed_contents` with `range` as a type annotation.  

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/typing.rs`:41 on 2024-05-27 13:44_

I suspect that this will fail if the string has any prefixes. While uncommon, it is valid python code to e.g. have `r"test"` 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:225 on 2024-05-27 13:46_

Nit: Maybe `parse_with_recovery` to make it clear that this parsing mode performs error recovery. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:240 on 2024-05-27 13:55_

You brought it up in Discord that we'll have a conflict with red-knot. 

How about `Parsed` or `SyntaxTree`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:241 on 2024-05-27 13:56_

Nit: I would rename this to `root` if you choose to rename the struct to `SyntaxTree`.

---

_@MichaReiser approved on 2024-05-27 13:56_

---

_@dhruvmanila reviewed on 2024-05-27 16:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:146 on 2024-05-27 16:13_

Yeah, it is getting quite big:
* `ParserCheckpoint` - 176 bytes
* `TokenSourceCheckpoint` - 152 bytes
* `LexerCheckpoint` - 136 bytes

---

_@dhruvmanila reviewed on 2024-05-27 16:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:29 on 2024-05-27 16:14_

Yeah, I can add it to my todo list to look at once the code starts to compile :)

It makes sense to keep it outside the parser.

---

_@dhruvmanila reviewed on 2024-05-27 16:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/typing.rs`:41 on 2024-05-27 16:34_

I think the name of the function is misleading, it does match against both prefixes and quotes:

https://github.com/astral-sh/ruff/blob/c30d34e802ecffc6e820465adc4f7190ede1efb8/crates/ruff_python_ast/src/str.rs#L207-L221

---

_@dhruvmanila reviewed on 2024-05-27 16:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:240 on 2024-05-27 16:50_

Yeah, I like `Parsed`. Thanks!

---

_Merged by @dhruvmanila on 2024-05-27 16:53_

---

_Closed by @dhruvmanila on 2024-05-27 16:53_

---

_Branch deleted on 2024-05-27 16:53_

---

_@dhruvmanila reviewed on 2024-05-27 16:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:240 on 2024-05-27 16:54_

I'll change this in a follow-up PR.

---
