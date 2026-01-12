```yaml
number: 6659
title: Add support for the new f-string tokens per PEP 701
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - python312
assignees: []
merged: true
base: dhruv/pep-701
head: dhruv/fstring-tokens
created_at: 2023-08-17T19:24:38Z
updated_at: 2023-09-14T01:46:51Z
url: https://github.com/astral-sh/ruff/pull/6659
synced_at: 2026-01-12T15:55:22Z
```

# Add support for the new f-string tokens per PEP 701

---

_@dhruvmanila_

## Summary

This PR adds support in the lexer for the newly added f-string tokens as per PEP 701. The following new tokens are added:
* `FStringStart`: Token value for the start of an f-string. This includes the `f`/`F`/`fr` prefix and the opening quote(s).
* `FStringMiddle`: Token value that includes the portion of text inside the f-string that's not part of the expression part and isn't an opening or closing brace.
* `FStringEnd`: Token value for the end of an f-string. This includes the closing quote.

Additionally, a new `Exclamation` token is added for conversion (`f"{foo!s}"`) as that's part of an expression.

## Test Plan

New test cases are added to for various possibilities using snapshot testing. The output has been verified using python/cpython@f2cc00527e.

## Benchmarks

_I've put the number of f-strings for each of the following files after the file name_

```
lexer/large/dataset.py (1)       1.05   612.6±91.60µs    66.4 MB/sec    1.00   584.7±33.72µs    69.6 MB/sec
lexer/numpy/ctypeslib.py (0)     1.01    131.8±3.31µs   126.3 MB/sec    1.00    130.9±5.37µs   127.2 MB/sec
lexer/numpy/globals.py (1)       1.02     13.2±0.43µs   222.7 MB/sec    1.00     13.0±0.41µs   226.8 MB/sec
lexer/pydantic/types.py (8)      1.13   285.0±11.72µs    89.5 MB/sec    1.00   252.9±10.13µs   100.8 MB/sec
lexer/unicode/pypinyin.py (0)    1.03     32.9±1.92µs   127.5 MB/sec    1.00     31.8±1.25µs   132.0 MB/sec
```

It seems that overall the lexer has regressed. I profiled every file mentioned above and I saw one improvement which is done in (098ee5d493ca83238754a8cb4629fa1b91144b84). But otherwise I don't see anything else. A few notes by isolating the f-string part in the profile:
* As we're adding new tokens and functionality to emit them, I expect the lexer to take more time because of more code.
* The `lex_fstring_middle_or_end` takes the most amount of time followed by the `current_mut` line when lexing the `:` token. The latter is to check if we're at the start of a format spec or not.
* In a f-string heavy file such as https://github.com/python/cpython/blob/main/Lib/test/test_fstring.py [^1] (293), most of the time in `lex_fstring_middle_or_end` is accounted by string allocation for the string literal part of `FStringMiddle` token (https://share.firefox.dev/3ErEa1W)

I don't see anything out of ordinary for `pydantic/types` profile (https://share.firefox.dev/45XcLRq)

fixes: #7042

[^1]: We could add this in lexer and parser benchmark

---

_Comment by @dhruvmanila on 2023-08-17 19:24_

Current dependencies on/for this PR:
* main
  * **PR #7035** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7035" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7013** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7013" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #6659** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6659" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
        * **PR #7041** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7041" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #7211** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7211" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #7263** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7263" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #7331** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7331" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                  * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                      * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                        * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6659?utm_source=stack-comment).

---

_Label `parser` added by @MichaReiser on 2023-08-18 06:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:55 on 2023-08-18 06:06_

Is there a technical reason why we want to limit the fstring nesting?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:546 on 2023-08-18 06:08_

Could we pass the fstring context instead of using an `unwrap` here? It isn't just safer but also guarantees that we don't have a panic branch in release builds.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:634 on 2023-08-18 06:10_

Why do we need to clone chars?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:627 on 2023-08-18 06:12_

```suggestion
        if self.cursor.first() == context.quote_char() {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1422 on 2023-08-18 06:14_

Nit: Maybe `Kind`

---

_@MichaReiser reviewed on 2023-08-18 06:15_

---

_@dhruvmanila reviewed on 2023-08-18 13:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:55 on 2023-08-18 13:11_

No, PEP says that the lower bound is 2. While there's no upper bound, CPython implementation itself have an [upper bound of 3](https://github.com/python/cpython/blob/fd195092204aa7fc9f13c5c6d423bc723d0b3520/Parser/tokenizer.h#L43) and [here's](https://github.com/python/cpython/blob/fd195092204aa7fc9f13c5c6d423bc723d0b3520/Parser/tokenizer.c#L2650-L2652) where they throw an error if it goes beyond that. Currently, we have an upper bound of 2

https://github.com/astral-sh/ruff/blob/0cea4975fcfe09716c084c0a18d1b4c4cd9b8f05/crates/ruff_python_parser/src/string.rs#L420-L422

---

_@dhruvmanila reviewed on 2023-08-18 13:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:546 on 2023-08-18 13:13_

Not really. The `self.try_lex_fstring` functions borrow `self` as mutable but then we pass in an immutable borrow of `FStringContext` which belongs to `self`:

```
error[E0502]: cannot borrow `*self` as mutable because it is also borrowed as immutable
   --> crates/ruff_python_parser/src/lexer.rs:730:36
    |
727 |         if let Some(fstring_context) = self.fstring_stack.last() {
    |                                        ------------------------- immutable borrow occurs here
...
730 |                 if let Some(tok) = self.try_lex_fstring_middle(fstring_context)? {
    |                                    ^^^^^----------------------^^^^^^^^^^^^^^^^^
    |                                    |    |
    |                                    |    immutable borrow later used by call
    |                                    mutable borrow occurs here
```

---

_@dhruvmanila reviewed on 2023-08-18 13:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1422 on 2023-08-18 13:14_

There's a similar implementation for the string quote and size in the `tokenizer.rs` file where `SimpleTokenizer` is present. But, we can't really depend on `ruff_python_trivia`. Currently, I've added an additional implementation here but maybe it could be possible to move it to a common place like `ruff_python_ast`?

---

_Comment by @dhruvmanila on 2023-08-18 13:15_

@MichaReiser Thanks for your initial review even though the PR is still in draft :)

---

_@dhruvmanila reviewed on 2023-08-18 13:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:627 on 2023-08-18 13:17_

We actually need the `quote` for the lookaheads if it's a triple quoted string.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:55 on 2023-08-18 13:20_

I would remove the bound because different interpreter implementations may use different limits. However, it would be a good idea to have a lint rule that warns about too deeply nested FString depending on your target runtime.

---

_@MichaReiser reviewed on 2023-08-18 13:20_

---

_@dhruvmanila reviewed on 2023-08-18 13:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:634 on 2023-08-18 13:20_

Oh right, we don't.

---

_@MichaReiser reviewed on 2023-08-18 13:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:627 on 2023-08-18 13:20_

Could we use `context.quote_char`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:561 on 2023-08-21 13:37_

We could avoid the `StringQuoteChar` abstraction and store the quote `char` directly as the only use case is as comparing against the ending quote `char`.

---

_Review comment by @konstin on `crates/ruff_python_parser/src/lexer.rs`:580 on 2023-08-21 17:50_

Do we want to add `eat_if` and `eat_if2` methods?

---

_@konstin reviewed on 2023-08-21 17:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:580 on 2023-08-22 04:40_

Actually, we could use `eat_char` in the same order as it will short circuit if the first char isn't `N`.

---

_@dhruvmanila reviewed on 2023-08-22 04:40_

---

_@dhruvmanila reviewed on 2023-08-22 18:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:55 on 2023-08-22 18:15_

There's also the limitation of nested f-strings itself, so `f"foo {f"bar {f"baz"}"}"` which is also [limited to 150](https://github.com/python/cpython/blob/0cb0c238d520a8718e313b52cffc356a5a7561bf/Parser/tokenizer.h#L15). I'm avoiding that in our implementation for the same reason.

---

_@dhruvmanila reviewed on 2023-08-23 05:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:714 on 2023-08-23 05:38_

I'm thinking of removing the `StringKind::FString` and `StringKind::RawFString` as that is an invalid representation now that we'll be emitting different tokens for f-strings. Instead, I'm thinking of updating the `lex_identifier` to directly check for `f` (and related `F`, `r`, `R`) and call `lex_fstring_start` directly.

---

_Marked ready for review by @dhruvmanila on 2023-08-23 05:39_

---

_Comment by @dhruvmanila on 2023-08-23 05:39_

(This is ready for review, but not to be merged)

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-08-23 05:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:49 on 2023-08-23 07:01_

We'll need to go through all logical line rules to make sure they handle f-strings correctly

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1365 on 2023-08-23 07:04_

Nit: This method name sounds like an accessor, but it actually updates the parentheses count

```suggestion
    fn inc_open_parentheses(&mut self) {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1391 on 2023-08-23 07:07_

Nit: It's rather unexpected that a `is_` function mutates self. Maybe `try_start_format_spec`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1392 on 2023-08-23 07:08_

This could potentially overflow, or is it guaranteed that `parentheses_count` is always larger than `format_spec_count` (even for invalid input)? 

Edit: This doesn't seem to be given, see `is_in_expression`. We need to use `saturating_sub` here

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1433 on 2023-08-23 07:09_

Can we document these fields and why tracking them in the state is necessary? E.g. why do we need to track both `parenthesis_count` and `format_spec_count` (Should it be `format_spec_depth`?). What are the different possible states?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1395 on 2023-08-23 07:14_

Nit: Implement `Copy` for these value enums. Rust will then tell you that it's more performant to pass `StringQuoteChar` by value rather by reference (1 byte vs 8 byte + dereferencing)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1086 on 2023-08-23 07:15_

It is unfortunate that we now need to increment open parentheses twice if inside of an `fstring` context (and we have more branching inside a hot function. 

Would it be possible to store the initial `parentheses_count` on the `FStringContext` instead? You can then compute the fstring parentheses by doing `self.nesting.saturating_sub(self.initial_count)`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:164 on 2023-08-23 07:18_

Nit: Have you considered introducing a `FStringStack` newtype wrapper around `Vec`? Could that reduce some repetitive code inside of the lexer? Similar to `Indentations`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1124 on 2023-08-23 07:19_

Is this because `:=` has a different meaning inside of a format spec?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1434 on 2023-08-23 07:21_

Nit: We could potentially squeeze `quote_char`, `quote_size` and `raw` (3 bytes) into a single byte by using bitflags. It won't give us any space-saving because of padding but may be easier to expand.

You would have three flags:

* TRIPLE (missing implies single)
* DOUBLE (missing implies single)
* RAW (missing implies non-raw)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:772 on 2023-08-23 07:26_

Nit: I wonder if it would make sense to refactor `next_token` a bit so that we could call into a `lex_fstring_expression` that:

* Doesn't handle indentation
* overrides the lexing for fstring- specific tokens 

But I'm not sure if its worth the complexity (and work)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:615 on 2023-08-23 07:34_

Nit
```suggestion
        self.cursor.start_token();
        loop {
            match self.cursor.first() {
                EOF_CHAR => {
                    let error = if context.allow_multiline() {
                        FStringErrorType::UnterminatedTripleQuotedString
                    } else {
                        FStringErrorType::UnterminatedString
                    };
                    self.fstring_stack.pop();
                    return Err(LexicalError {
                        error: LexicalErrorType::FStringError(error),
                        location: self.offset(),
                    });
                }
                '\n' if !context.allow_multiline() => {
                    self.fstring_stack.pop();
                    return Err(LexicalError {
                        error: LexicalErrorType::FStringError(FStringErrorType::UnterminatedString),
                        location: self.offset(),
                    });
                }
                '\\' => {
                    self.cursor.bump(); // '\'
                    if matches!(self.cursor.first(), '{' | '}') {
                        // Don't consume `{` or `}` as we want them to be consumed as tokens.
                        break;
                    } else if !context.is_raw_string {
                        if self.cursor.eat_char('N') && self.cursor.eat_char('{') {
                            in_named_unicode = true;
                            continue;
                        }
                    }
                    // Consume the escaped character.
                    self.cursor.bump();
                }
                quote @ ('\'' | '"') if quote == context.quote_char() => {
                    match context.quote_size {
                        StringQuoteSize::Single => break,
                        StringQuoteSize::Triple => {
                            let mut remaining = self.cursor.rest().chars().skip(1);
                            if remaining.next() == Some(quote) && remaining.next() == Some(quote) {
                                break;
                            }
                        }
                    }
                    self.cursor.bump();
                }
                '{' => {
                    if self.cursor.second() == '{' {
                        self.cursor.bump();
                        self.cursor.bump();
                    } else {
                        break;
                    }
                }
                '}' => {
                    if in_named_unicode {
                        in_named_unicode = false;
                        self.cursor.bump();
                    } else if self.cursor.second() == '}' && !context.is_in_format_spec() {
                        self.cursor.bump();
                        self.cursor.bump();
                    } else {
                        break;
                    }
                }
                _ => {
                    self.cursor.bump();
                }
            }
        }
        let range = self.token_range():

        if range.is_empty() {
            return Ok(None);
        }

        let value = self.source[range]
            .to_string()
            .replace("{{", "{")
            .replace("}}", "}");
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:613 on 2023-08-23 07:36_

The `to_string` call here is unnecessary. `replace` always allocates a new string already
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:615 on 2023-08-23 07:40_

Calling replace twice is `O(2N)` on the string length. This may not matter much considering that most strings are small but can add up. It is also unfortunate that we do this even for strings that we know never contain a `{{` or `}}` sequence. There are two ways of how we can avoid this additional pass over the string:

* Track whether the string contained any `{{` or `}}` and only then call replace
* Implement your own `replace` that replaces both sequences in a single loop 
* Create a normalized string, but only initialize it when you see a `{{` or `}}`. Otherwise use the full text range, see https://github.com/astral-sh/ruff/blob/0cea4975fcfe09716c084c0a18d1b4c4cd9b8f05/crates/ruff_python_formatter/src/expression/string.rs#L607-L679

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:619 on 2023-08-23 07:45_

```suggestion
    fn eat_fstring_end(&mut self) -> Option<Tok> {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:659 on 2023-08-23 07:51_

I prefer to use `eat` whenever possible because it avoids bugs where I accidentally forgot to call `bump`, which results in an infinite loop:

```suggestion
        match context.quote_size {
            StringQuoteSize::Single => {
                if self.cursor.eat_char(context.quote_char()) {
                    return Some(Tok::FStringEnd);
                }
            }
            StringQuoteSize::Triple => {
                let expected = match context.quote_char {
                    StringQuoteChar::Single => "'''",
                    StringQuoteChar::Double => r#"""""#,
                };
                if self.cursor.rest().starts_with(expected) {
                    self.cursor.bump();
                    self.cursor.bump();
                    self.cursor.bump();
                    return Some(Tok::FStringEnd);
                }
            }
        }
```

I like @konstin's suggestion to add a `eat_char2` and `eat_char3` that only eat if all two or three chars match. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:771 on 2023-08-23 07:55_

For which tokens does fstring_middle` and `fstring_end` both return None? Could we instead branch early on these tokens and unconditionally either return the middle or end part? 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:758 on 2023-08-23 07:56_

I dislike adding a stack lookup into the hot path but I don't see a way of how we could avoid it :(

---

_@MichaReiser reviewed on 2023-08-23 07:57_

This is impressive work. I left a few follow-up questions and I want to wait to approve this PR until we have the first benchmarks in.



---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1433 on 2023-08-24 03:26_

There are 3 reasons to track these fields:
1. To check if we're in a f-string expression `f"{<here>}"`
2. To check if we're in a format spec within a f-string expression `f"{foo:<here>}"`
3. To check if the `:` is to start a format spec or is it part of, for example, named expression `:=`:
	* For `f"{x:=1}"`, the colon is to indicate the format spec start
	* For `f"{(x:=1)}"`, the colon is part of `:=` named expression because the colon is not at the same level of parentheses. Another example is `f"{x,{y:=1}}"` where the colon is part of `:=` named expression

From [PEP 701: How to produce these new tokens](https://peps.python.org/pep-0701/#how-to-produce-these-new-tokens):
> 3. [..] This mode tokenizes as the “Regular Python tokenization” until a `:` or a `}` character is encountered with the same level of nesting as the opening bracket token that was pushed when we enter the f-string part. [..]

With this explanation, do you think `_depth` is a better suffix?

---

_@dhruvmanila reviewed on 2023-08-24 03:26_

---

_@dhruvmanila reviewed on 2023-08-24 03:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1124 on 2023-08-24 03:27_

Does https://github.com/astral-sh/ruff/pull/6659#discussion_r1303742193 answer your question here?

---

_@dhruvmanila reviewed on 2023-08-24 03:37_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1086 on 2023-08-24 03:37_

This is a good point! This would mean that to get the `parentheses_count` inside any method of `FStringContext`, the `self.nesting` needs to be passed to these methods.

Also, note that for closing parentheses we still need to call the method to decrement the format spec count.

---

_@dhruvmanila reviewed on 2023-08-24 04:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:771 on 2023-08-24 04:13_

So, `maybe_lex_fstring_middle` will return `None` when:
1. The first character is `{` not followed by another `{` which is the start of f-string expression. For example, `f"{foo}"` and not `f"{{foo}}"`
2. Empty f-string which means that we should directly consume the f-string end. For example, `f""`, `f''`, `f""""""`, `f''''''`.

I think you're referring to (1) then.

---

_@dhruvmanila reviewed on 2023-08-24 04:22_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:659 on 2023-08-24 04:22_

Oh yes, I can totally relate to the part where we forgot to call `bump` :)

I've added these methods.

---

_@dhruvmanila reviewed on 2023-08-24 04:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:771 on 2023-08-24 04:31_

This is a really good suggestion. Thanks, I've added it here [`5b41241` (#6659)](https://github.com/astral-sh/ruff/pull/6659/commits/5b41241b0f37d7539439c93a3408d91f69c29861) with some test cases.

---

_@dhruvmanila reviewed on 2023-08-24 04:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:771 on 2023-08-24 04:38_

I've added `unreachable` to check in what other cases this happens.

---

_@dhruvmanila reviewed on 2023-08-24 04:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:771 on 2023-08-24 04:55_

There's another case as well:

We're in format specifier mode and we've reached the end i.e., the next character is `}` which should be emitted as RBrace.

For example, in `f"{foo:.3f}"`, the format specifier `.3f` is emitted as `FStringMiddle`, then in the next iteration we need to emit `}` (RBrace) but as we're still in f-string context we'll try to lex f-string middle (returns `None`) and f-string end (not there yet).

I'm unable to find any other case which reaches `unreachable`. This means we can avoid branching but I would need to make sure that (maybe `debug_assertion`?) that the invariant holds true.

---

_@dhruvmanila reviewed on 2023-08-24 05:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:615 on 2023-08-24 05:26_

Thanks! I've implemented this in [`2de590d` (#6659)](https://github.com/astral-sh/ruff/pull/6659/commits/2de590d2341dfd7f2f5b86458dc1bcba097ab99d)

---

_Label `python312` added by @zanieb on 2023-08-24 20:02_

---

_@dhruvmanila reviewed on 2023-08-25 11:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:164 on 2023-08-25 11:23_

We can introduce that abstraction but can you expand on "Could that reduce some repetitive code inside of the lexer?". What part is repetitive?

---

_@dhruvmanila reviewed on 2023-08-25 11:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:758 on 2023-08-25 11:50_

Maybe we could utilize `State`? The problem here is that it'll get updated when inside a f-string expression as it's not persistent.

---

_@dhruvmanila reviewed on 2023-08-25 11:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:743 on 2023-08-25 11:56_

I don't really like this that much. I think we're sacrificing readability a bit. Without this, the `lex_fstring_middle_or_end` function would return `Result<Option<Tok>, ...>` and this condition will be identified initially in the function loop itself.

---

_@dhruvmanila reviewed on 2023-08-25 11:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:771 on 2023-08-25 11:57_

I do like this idea but it sacrifices readability a bit (https://github.com/astral-sh/ruff/pull/6659#discussion_r1305569804). I'll give it a bit of thought but I'm mostly leaning towards reverting the change.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:183 on 2023-08-29 02:47_

This separation is because the `StringKind::FString` and `StringKind::RawFString` variants will be removed after all the linter changes.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:538 on 2023-08-29 02:50_

This is repetitive from the `lex_string` function, maybe we could define a `consume_open_quote` function:

```rust
fn consume_open_quote(&mut self, quote: char) -> (StringQuoteChar, StringQuoteSize) {
	// ...
}
```

Or, return bitflags?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:695 on 2023-08-29 02:51_

This is required for the parser changes.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:704 on 2023-08-29 02:52_

Source: https://github.com/python/cpython/blob/21a7420190778fb6e9237bf12e029a26cd18d82d/Parser/tokenizer.c#L2444-L2455

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser.rs`:704 on 2023-08-29 02:55_

The tests are for `try ... except ...` so let's use normal strings to avoid putting the test behind feature flag.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser.rs`:720 on 2023-08-29 02:55_

The tests are for `try ... except ...` so let's use normal strings to avoid putting the test behind feature flag.

---

_@dhruvmanila reviewed on 2023-08-29 02:55_

---

_@dhruvmanila reviewed on 2023-08-29 02:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:743 on 2023-08-29 02:56_

I've reverted this change although I'm open to suggestions.

---

_@dhruvmanila reviewed on 2023-08-29 04:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1434 on 2023-08-29 04:32_

This is a good idea! I've implemented it and it makes the code which uses the `FStringContext` better to reason about. Thanks!

---

_Review comment by @konstin on `crates/ruff_python_parser/src/lexer.rs`:561 on 2023-09-04 12:32_

```suggestion
        // We have to decode `{{` and `}}` into `{` and `}` respectively. As an optimization, we 
        // only allocate a new string we find any escaped curly braces, otherwise this string will
        // remain empty and we'll use a source slice instead.
```

---

_@konstin approved on 2023-09-04 12:37_

---

_@dhruvmanila reviewed on 2023-09-05 01:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:714 on 2023-09-05 01:43_

This will be done after the linter changes are complete.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1086 on 2023-09-05 01:46_

I've made this change. It turns out that the `last_mut` part was taking on average 10% of the lexing time. I think this is because we were performing this check for _every_ parentheses irrespective of whether we're inside a f-string or not. This has improved the benchmarks. Thanks!

---

_@dhruvmanila reviewed on 2023-09-05 01:46_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-09-05 04:22_

---

_Comment by @dhruvmanila on 2023-09-05 04:26_

## Major changes since last review:

* Combine lexing of `FStringMiddle` and `FStringEnd` tokens (earlier they were separate).
* Added `FStrings` (similar to `Indentations`), `FStringContextFlags` and related changes in the lexer.
* Emit empty `FStringMiddle` token for special case to signal the lexer that we're in a lambda expression without parentheses so that the parser throws an error instead.
* Avoid multiple increment/decrement for nesting. Instead store the initial nesting in `FStringContext` and pass in the current nesting to compute the f-string open parentheses on demand.

For other changes, refer to the last review comment threads.

---

_Closed by @MichaReiser on 2023-09-06 08:14_

---

_Reopened by @MichaReiser on 2023-09-06 08:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:178 on 2023-09-06 08:22_

I'm undecided if it makes it easier but we could instead branch on the string kind instead of adding more match arms:

```
return if string_kind.is_any_fstring() {
    Ok(self.lex_fstring_start(quote, string_kind.is_raw()));
} else {
    self.lex_string(string_kind, quote)
};
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:538 on 2023-09-06 08:23_


```suggestion
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/fstring.rs`:7 on 2023-09-06 08:25_

`pub(super)` should be sufficient for all the types in this module and this can be an `u8`

```suggestion
    pub(crate) struct FStringContextFlags: u8 {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/fstring.rs`:47 on 2023-09-06 08:26_


```suggestion
    pub(crate) const fn quote_char(&self) -> char {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/fstring.rs`:38 on 2023-09-06 08:26_


```suggestion
    pub(crate) const fn new(flags: FStringContextFlags, nesting: u32) -> Self {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/fstring.rs`:56 on 2023-09-06 08:27_


```suggestion
    pub(crate) const fn quote_size(&self) -> TextSize {
```

and use `TextSize::new`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/fstring.rs`:92 on 2023-09-06 08:27_


```suggestion
    pub(crate) const fn is_raw_string(&self) -> bool {
        self.flags.contains(FStringContextFlags::RAW)
    }

    /// Returns `true` if the current f-string is a triple-quoted f-string.
    pub(crate) const fn is_triple_quoted(&self) -> bool {
        self.flags.contains(FStringContextFlags::TRIPLE)
    }
```

And many other functions of this type

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:767 on 2023-09-06 08:39_

Nit
```suggestion
                    if tok == Tok::FStringEnd {
                        self.fstrings.pop();
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/fstring.rs`:88 on 2023-09-06 08:52_

Rename `parentheses` to `curly` if you mean `{` `}` parentheses to align with the terminology used througout the parser/lexer.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:754 on 2023-09-06 08:59_

I see some improvements by storing a `in_fstring` boolean and pass that to `consume_ascii_char` and use it before performing any `self.fstrings` lookups (and then use unwrap). Could you take a look if the improvements are meaningful (You have a better understanding on how stable the lexer benchmarks are.)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:792 on 2023-09-06 09:05_

Nit: We can make this an `else if` branch of the above. Intentation handling isn't necessary inside of fstrings.  This together with #7184 could speed up lexing when inside of an fstring

---

_@MichaReiser approved on 2023-09-06 09:27_

I've a few more nit comments but this looks good to me. I'm not too worried about the performance regression because we move logic from the parser to the lexer, meaning these changes should ultimately lead to a perf improvement in the parser (at least for files with fstrings. There will be a regression for lexing non-fstring code)
 

---

_Comment by @MichaReiser on 2023-09-06 09:33_

To add some more context on the perf regression. The size of `next_token` increases by about 40% from 6KB to 10KB. That obviously has a negative impact. In addition, `lex_identifier` increases by 33% from 3KB to 4KB which has a negative impact too because identifiers are the most common tokens. 

I don't see a way of reducing `next_token` but maybe there's a way to simplify `lex_identifier`? For example, could we match on `f`, `F` and `r` |`R` in `consume_ascii_char` to avoid matching on the first identifier character twice?

---

_@dhruvmanila reviewed on 2023-09-06 13:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:178 on 2023-09-06 13:41_

My reasoning behind this change is that the `FString` variant would be an invalid state as there won't be any `String` token of that kind. It just becomes an intermediate state to determine whether we want to lex a string or f-string start token. So, I was planning to remove it once all the linter changes are complete because they still use this variant.

---

_@dhruvmanila reviewed on 2023-09-06 13:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer/fstring.rs`:88 on 2023-09-06 13:44_

This actually includes all the three types of parentheses: `(`, `{` and `[`.

---

_Comment by @dhruvmanila on 2023-09-11 17:55_

***The implementation have been removed in #7263. Keeping this here for posterity:***

> ### Empty `FStringMiddle` token
>
> `FStringMiddle` includes the portion of text inside the f-string that's not part of the expression and isn't an opening or closing brace which also includes the format spec. For example, in `f"foo {bar:.3{x}f} bar"`, the `foo `, `.3`, `f` and ` bar` are `FStringMiddle` tokens (4 in total).
>
> In the lambda example (`f"{lambda x: {x}"`) as there's a whitespace after `:` and before the `{` token, that is a `FStringMiddle` token whose content is a single space character. The lambda pattern in LALRPOP doesn't contain `FStringMiddle` token which means it'll error with an unexpected token. But, if you remove the space (`f"{lambda x:{x}}"`), there won't be a `FStringMiddle` token anywhere and it'll match the lambda pattern in LALRPOP definition.
>
> We'll emit an empty `FStringMiddle` token so that the parser will throw an error and disallow non-parenthesized lambda expressions. The way it is done is:
> 1. A `}` token while lexing for `FStringMiddle` is only encountered when we're inside a format spec (the part after the `:`).
> 2. We'll emit an empty `FStringMiddle` token when we encounter `}` and reduce the format spec depth to avoid going into infinite mode where the lexer then keeps emitting the empty token.

---

_Merged by @dhruvmanila on 2023-09-14 01:46_

---

_Closed by @dhruvmanila on 2023-09-14 01:46_

---

_Branch deleted on 2023-09-14 01:46_

---
