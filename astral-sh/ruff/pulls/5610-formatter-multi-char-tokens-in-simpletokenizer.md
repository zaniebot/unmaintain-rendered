```yaml
number: 5610
title: "formatter: multi char tokens in SimpleTokenizer "
type: pull_request
state: merged
author: davidszotten
labels: []
assignees: []
merged: true
base: main
head: simple-tokenizer-multi-char-tokens
created_at: 2023-07-08T09:45:34Z
updated_at: 2023-07-10T08:01:00Z
url: https://github.com/astral-sh/ruff/pull/5610
synced_at: 2026-01-12T15:55:19Z
```

# formatter: multi char tokens in SimpleTokenizer 

---

_@davidszotten_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-07-08 10:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.02ms     4.9 MB/sec    1.01      8.4±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1777.2±2.45µs     9.4 MB/sec    1.00   1781.6±3.53µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    195.4±0.28µs    15.1 MB/sec    1.01    197.1±1.11µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     14.0±0.02ms     2.9 MB/sec    1.00     13.9±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.9±0.70µs     8.2 MB/sec    1.00    362.8±1.07µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1465.9±2.00µs    11.4 MB/sec    1.00   1443.7±2.13µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.3±0.47µs    18.9 MB/sec    1.00    155.1±0.27µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     10.6±0.61ms     3.8 MB/sec     1.03     10.9±0.67ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.11ms     7.5 MB/sec     1.03      2.3±0.13ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   265.9±18.58µs    11.1 MB/sec     1.01   267.3±19.84µs    11.0 MB/sec
formatter/pydantic/types.py                1.02      5.2±0.49ms     4.9 MB/sec     1.00      5.1±0.31ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     17.6±0.80ms     2.3 MB/sec     1.01     17.9±0.81ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.22ms     3.7 MB/sec     1.03      4.7±0.21ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   554.9±29.80µs     5.3 MB/sec     1.03   573.9±38.12µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.45ms     3.2 MB/sec     1.02      8.1±0.42ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.44ms     4.5 MB/sec     1.02      9.2±0.48ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1976.4±128.45µs     8.4 MB/sec    1.00  1985.3±130.29µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   235.5±13.96µs    12.5 MB/sec     1.01   237.0±15.92µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.23ms     6.2 MB/sec     1.00      4.1±0.17ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @davidszotten on 2023-07-08 10:46_

---

_@MichaReiser reviewed on 2023-07-08 13:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:309 on 2023-07-08 13:12_

I believe this will incorrectly lex `ify` as an `if` keyword because it doesn't test what follows after. Lexing out keyword is a bit harder than I assumed it is.

I'm also unsure if LLVM is able to short-circuit if the expression, for example, starts with a number.

I think the way I would implement this is to implement lexing for identifiers, and then use a match on the lexed identifier to test if it is a keyword. You'll need to add a dependency to `unic-ucd-ident` (used by RustPython's lexer) to detect the start/end of an identifier.

```rust
if is_identifier_start(c) {
	self.cursor.eat_while(is_identifier_continuation);

	// If we want to support all identifiers, then we need to handle the string prefixes `b'`, `rf` etc. but we can ignore those for keywords
	
	let range = TextRange::at(self.offset, token_len);
	let kind = match &self.source[range] {	 // This requires storing the whole source on the tokenizer. I think that's fine
		"if" => TokenKind::If,
		"else" => TokenKind::Else,
		"match" => TokenKind::Match // Match is a soft keyword that depends on the context but we can always lex it as a keyword and leave it to the caller (parser) to decide if it should be handled as an identifier or keyword.
		...,
		_ => TokenKind::Other // Potentially an identifier, but only if it isn't a string prefix. We can ignore this for now https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals
	}
}

fn is_identifier_start(c: char) -> bool {
	c.is_ascii_alphabetic() || c == '_' || is_non_ascii_identifier_start(c) 
}

// Checks if the character c is a valid continuation character as described
// in https://docs.python.org/3/reference/lexical_analysis.html#identifiers
fn is_identifier_continuation(c: char) -> bool {
    if c.is_ascii() {
        matches!(c, 'a'..='z' | 'A'..='Z' | '_' | '0'..='9')
    } else {
        is_xid_continue(c)
    }
}

fn is_non_ascii_identifier_start(c: char) -> bool {
    is_xid_start(c)
}
```

Something similar should work for the backward parsing, except that it must start with an `is_non_ascii_identifier_start` and the last (or first, depending on how you look at the problem) must be `is_identifier_start` . We should be able to move the match expression into some shared function. 

---

_Comment by @MichaReiser on 2023-07-08 13:16_

Neat. Thank you for working on this. I think there's an edge case that I didn't consider initially, making this a little less simple than I assumed (we may also need to rename the `SimpleTokenizer` at some point, it's getting close to a full python lexer)

---

_Comment by @davidszotten on 2023-07-08 16:19_

Thanks, will have a go

---

_Comment by @davidszotten on 2023-07-08 18:43_

in unicode fun, some multi-character grapheme clusters (including one from `black/simple_cases/tricky_unicode_symbols.py`, `ម`) now result in different tokens when parsed in reverse.

```
 left:  `[Token { kind: Bogus, range: 0..3 }, Token { kind: Other, range: 3..6 }]`,
 right: `[Token { kind: Other, range: 0..6 }]`
```

 we can maybe fix by using something like https://crates.io/crates/unicode-segmentation but i'm not sure of the perf implications and i guess we don't need this while we only support keyword tokens

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:439 on 2023-07-08 20:59_

I believe we need to change this to test if it is an id continuation because we are testing the last character and not the first 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:445 on 2023-07-08 21:00_

We need to test if the last processed character was an identifier start (otherwise 555 is a valid identifier)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:702 on 2023-07-08 21:01_

Hmm interesting. This is somewhat unexpected. I would expect that we consume the same text range 

---

_@MichaReiser reviewed on 2023-07-08 21:01_

---

_@davidszotten reviewed on 2023-07-09 06:43_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/trivia.rs`:439 on 2023-07-09 06:43_

ah yes. ~but i guess we also need to change the strategy a bit, since we must _finish with an identifier_start?~ ah you mentioned this below

---

_@davidszotten reviewed on 2023-07-09 06:45_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/trivia.rs`:445 on 2023-07-09 06:45_

unless i'm missing something, wouldn't this mean that say `555` would be parsed as a single `Other` backwards, but `[Other, Bogus, Bogus]` forwards?  to fix, don't we need to scan first (without bumping) to make sure we end on an `identifier_start`?

---

_@MichaReiser reviewed on 2023-07-09 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:445 on 2023-07-09 07:18_

Oh yeah. Good point. I think we can either scan first (similar to detecting if there are any comments), or we clone curser, lex it, and restore the cursor if first turns out not to be an identifier 

Did these changes fix the graphemes issue?

---

_@davidszotten reviewed on 2023-07-09 07:19_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/trivia.rs`:702 on 2023-07-09 07:19_

i think this is fixed by the above conversation

but strings of non-identifiers (or any chars that would be considered `Other` like `555`) would still be parsed as `Other` followed by `Bogus`, and depending on direction, the `Other` would be first or last. This is the case currently on `main`, eg with `555` or even `aaa`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1223 on 2023-07-09 18:20_

Nit: We can use `debug_assert_eq` now to compare the kinds. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1226 on 2023-07-09 18:20_

Nit: We can use `debug_assert_eq(..., None)` here. It should give us a better error message when it throws (I believe)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:331 on 2023-07-09 18:23_

Nit: The method name indicates to me that this parses an identifier and not a keyword. I haven't been able to come up with a name that I like, but was thinking of `match_keyword` or `to_keyword_or_other`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:454 on 2023-07-09 18:25_

Not really. You could use `let mut identifier_start = c` and then update the variable in the `eat_back_while` callback if it is an `identifier_continuation` but I would prefer what you have right now.

---

_@MichaReiser approved on 2023-07-09 18:26_

This is awesome. Well done! I'm sorry that it turned out much more complicated than I thought. Let me know when you're ready to merge.

---

_@davidszotten reviewed on 2023-07-09 18:47_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/trivia.rs`:331 on 2023-07-09 18:47_

sure, i don't mind. i also considered making it a method on `TokenKind` (similar to `from_non_trivia_char`)

---

_@davidszotten reviewed on 2023-07-09 18:48_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/trivia.rs`:200 on 2023-07-09 18:48_

oops i guess this is no longer true (length 1)

---

_Comment by @davidszotten on 2023-07-09 18:51_

ok i think that's ready from me now

---

_Comment by @davidszotten on 2023-07-09 18:51_

thanks for the impl help and the review!

---

_@MichaReiser reviewed on 2023-07-10 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:331 on 2023-07-10 07:59_

That would work too!

---

_@MichaReiser reviewed on 2023-07-10 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:200 on 2023-07-10 07:59_

Good find

---

_Comment by @MichaReiser on 2023-07-10 08:00_

Thank you for implementing. This is so cool :) Not long, and our `SimpleTokenizer` can replace RustPython's tokenizer :P

---

_Merged by @MichaReiser on 2023-07-10 08:00_

---

_Closed by @MichaReiser on 2023-07-10 08:01_

---
