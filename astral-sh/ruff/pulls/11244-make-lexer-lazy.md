```yaml
number: 11244
title: "Make `Lexer` lazy"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/lazy-lexer
created_at: 2024-05-02T09:12:52Z
updated_at: 2024-05-17T11:34:12Z
url: https://github.com/astral-sh/ruff/pull/11244
synced_at: 2026-01-10T22:05:26Z
```

# Make `Lexer` lazy

---

_Pull request opened by @dhruvmanila on 2024-05-02 09:12_

## Summary

This PR updates the `Lexer` to make it lazy in the sense that the tokens are emitted only when it's requested.

### Lexer
* Remove `Iterator` implementation
* Remove `SoftkeywordTransformer`
* Collect all `LexicalError` in the lexer and return it on `finish` call
* Store the `current` token and provide methods to query it
* Implement a new `TokenValue` struct which stores the owned value for certain tokens [^1]
* Update all `lex_*` methods to return the token instead of `Result`
* Update `Lexer::new` to take a `start_offset` parameter
	* This will be used to start lexing from a specific offset
* Add checkpoint - rewind logic for the lexer, f-strings and indentation stack

### Token Source

* Remove `Iterator` implementation
* Provide lookahead via checkpoint - rewind logic on the `Lexer`
* Store all the lexed tokens

### Parser

* Update the soft keywords to identifier when parsing as an identifier
* Add `bump_value` to bump the given token kind and return the corresponding owned value
* Add `bump_any` to bump any token except for end of file

[^1]: At the end, we'll remove the `Tok` completely


---

_@MichaReiser reviewed on 2024-05-02 09:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1331 on 2024-05-02 09:15_

I think we also need to store the current token or `.current()` will return a stale token after rewinding.

---

_@MichaReiser reviewed on 2024-05-02 09:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/indentation.rs`:136 on 2024-05-02 09:16_

Nit: We may want to use newtype wrappers here and call the methods `snapshot` and `rewind`

---

_@dhruvmanila reviewed on 2024-05-02 09:21_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1331 on 2024-05-02 09:21_

If it's after rewinding then I think we should be fine because I introduced a `next_token_with_context` method for it with `LexerContext::Peeking`. We won't be overriding `current` if the lexer is in peeking context.

---

_@MichaReiser reviewed on 2024-05-02 09:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1331 on 2024-05-02 09:29_

But what if we do speculative parsing? In that case the parser might call into `token_value` or `current` to get the kind of the current token.

---

_@dhruvmanila reviewed on 2024-05-02 09:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1331 on 2024-05-02 09:54_

Yeah, in that case we'd need to store it. This would make caching lookahead a requirement

---

_@MichaReiser reviewed on 2024-05-09 07:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer/cursor.rs`:6 on 2024-05-09 07:39_

I love it how you add documentation as you go! It's very obvious on what code you've been working or that you touched. It's the code that is well documented. Thanks for doing it.

---

_@MichaReiser reviewed on 2024-05-09 07:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:463 on 2024-05-09 07:40_

This is nice.

Would you see an advantage of having a `TokenValueKind` to avoid the case that no value exists for the given token kind?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:475 on 2024-05-09 07:47_

Nit: I would recommend using `TokenKind::is_soft_keyword` method here (you'll have to add `type`). 

Nit: You could consider implementing the soft keyword check as a range comparison by:

* Implement `Ord` on `TokenKind`
* Have all keywords come first
* Followed by soft keywords


You can then implement different test methods:

* `is_keyword`: Compare if the `TokenKind` is in between the first keyword and the last soft keyword (inclusive)
* `is_soft_keyword`: Compare if the `TokenKind` is between the first and last soft keyword (inclusive)
* `is_non_soft_keyword`: Compare if the `TokenKind` is between the first keyword and the first soft keyword (exclusive)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:117 on 2024-05-09 07:51_

Do we need to update `prev_token_end` as well? I think I would make this a constructor function as well to avoid any other invariants

Related. Do we need to offset the tokens by `start_offset`? I think the current `lex_starts_at` doesn't do that but I think it's odd if the AST nodes are all offseted by `start_offset` and the tokens arent. Or is this also not the case? 

I think we also just have to do it or all range operations will return rather obscure results. For example, `src_text` computes `range - self.start_offset` but if `range` is relative to the start, than the offset underflows.

I think the way I would expect this to work is that we pass `start_offset` right through to the lexer and the lexer sets its current position to `start_offset`. 



---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:373 on 2024-05-09 07:59_

Okay, I see why it takes a `kind` :) 

Maybe call it `bump_value` to make it clear that we bump the token AND take the value (although the taking the value is abstracted away and, therefore, less important). 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:407 on 2024-05-09 08:00_

It looks a bit strange to create a tuple here just to destruct again. I think I would inline the expression now.

```suggestion
        let (found, range) = (self.current_token_kind(), self.current_token_range());
        self.add_error(
        	ParseErrorType::ExpectedToken { found: self.current_token_kind(), expected },
        	self.current_token_range()
      	);
```



---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/soft_keywords.rs`:1 on 2024-05-09 08:01_

It almost makes me sad to say goodby to @charliermarsh's genius implementation for supporting soft keywords with lalrpop. 

---

_@dhruvmanila reviewed on 2024-05-09 08:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:463 on 2024-05-09 08:06_

By `TokenValueKind`, do you mean that instead of `TokenValue::None`, we'd have a `TokenValue::Other(TokenValueKind)`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:112 on 2024-05-09 08:15_

Ah nice, we can finally move the errors into the lexer where they belong!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:825 on 2024-05-09 08:21_

I'm not sure what this will do to performance. It adds some additional branches to every `next_token` call even when the error case is extremely rare. 

I think I would prefer if we instead change all lex methods to simply return `TokenKind` and add a new `error_token(error)` method that returns `TokenKind::Unknown` and pushes the error. I think this has a few advantages

* We move out the error branching from the very hot `next_token` function to the few places where errors can happen and we already have an error branch there (we only pay the cost of an error branch once instead of twice)
* The lexer can now push multiple errors for a single `next_token` call. It even gets the option to push an error, but continue lexing (e.g. push an error for an unterminated string literal but continue lexing)
* We drastically reduce the return type size of all lex functions because I think `LexicalError` is somewhat heavy. It certainly is heavier than a single `TokenKind` :) 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1326 on 2024-05-09 08:22_

Nit: You can use `std::mem::take` if you implement `Default` for `TokenValue`
```suggestion
        std::mem::take(&mut self.value)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1333 on 2024-05-09 08:23_

```suggestion
            current: self.current,
```

I don't think the `clone` call is needed here because `Token` implements `Copy`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1568 on 2024-05-09 08:25_

I believe this is no longer used.

---

_@MichaReiser reviewed on 2024-05-09 08:27_

I like this a lot! Nice work. 

I think we can improve the lexer performance by change how we handle errors in `next_token` (see inline comment). 

We also need to think about how `parse_expression_at` should work. I think we need to pass the offset through to the lexer so that it can offset all token ranges and start lexing after `offset` in the source. 

---

_@dhruvmanila reviewed on 2024-05-09 09:22_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:825 on 2024-05-09 09:22_

Yeah, actually I had the same plan but I did this just to simplify the implementation for now. I should've made that clear 😅

---

_@MichaReiser reviewed on 2024-05-09 10:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:463 on 2024-05-09 10:27_

What I had in mind is to add a `TokenValueKind` that has a variant for each variant in `TokenValue`. It would ensure that `take_value` is never called for a `TokenKind` that has no associated value. However, I'm no longer sure if it's worth the effort, considering that `take_value` also calls `bump` 

---

_@dhruvmanila reviewed on 2024-05-10 05:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:463 on 2024-05-10 05:41_

Yeah, I'm not sure either as it'll still involve converting to `TokenKind` because of the `bump` call.

---

_@MichaReiser reviewed on 2024-05-10 06:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:463 on 2024-05-10 06:46_

Yeah. The only benefit I see is that it would help to find e.g. all usages of `TokenValueKind::String`. 

---

_@dhruvmanila reviewed on 2024-05-10 07:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:117 on 2024-05-10 07:57_

> Related. Do we need to offset the tokens by `start_offset`? I think the current `lex_starts_at` doesn't do that but I think it's odd if the AST nodes are all offseted by `start_offset` and the tokens arent. Or is this also not the case?

Yes, that is true. I missed that.

> I think the way I would expect this to work is that we pass `start_offset` right through to the lexer and the lexer sets its current position to `start_offset`.

Yes, it should be passed directly to the `Lexer` which will set the correct range for the tokens and then the parser can just use the range.

I'll still store the offset on the parser to correctly extract the source code for `src_text` function.

---

_@dhruvmanila reviewed on 2024-05-10 08:01_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:117 on 2024-05-10 08:01_

> Do we need to update `prev_token_end` as well? I think I would make this a constructor function as well to avoid any other invariants

Yeah, that makes sense. That should be updated as well, I'll make it a separate constructor function.

---

_@dhruvmanila reviewed on 2024-05-15 11:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:117 on 2024-05-15 11:31_

Fixed in #11433 

I'm going to test this out in the local fork of the parser to make sure the ranges are correct.

---

_@dhruvmanila reviewed on 2024-05-17 10:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:121 on 2024-05-17 10:06_

This is how the lexer will start lexing from the given offset.

---

_Label `parser` added by @dhruvmanila on 2024-05-17 10:12_

---

_Renamed from "WIP: Make `Lexer` lazy" to "Make `Lexer` lazy" by @dhruvmanila on 2024-05-17 11:32_

---

_Comment by @dhruvmanila on 2024-05-17 11:33_

I'm going to merge this into another PR to keep going. I'm finding it difficult to refactor with changes separated in different branches.

---

_Marked ready for review by @dhruvmanila on 2024-05-17 11:33_

---

_Merged by @dhruvmanila on 2024-05-17 11:34_

---

_Closed by @dhruvmanila on 2024-05-17 11:34_

---

_Branch deleted on 2024-05-17 11:34_

---
