```yaml
number: 11845
title: Implement re-lexing logic for better error recovery
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/re-lexing
created_at: 2024-06-12T06:34:23Z
updated_at: 2024-06-17T07:02:46Z
url: https://github.com/astral-sh/ruff/pull/11845
synced_at: 2026-01-12T15:55:39Z
```

# Implement re-lexing logic for better error recovery

---

_@dhruvmanila_

## Summary

This PR implements the re-lexing logic in the parser.

This logic is only applied when recovering from an error during list parsing. The logic is as follows:
1. During list parsing, if an unexpected token is encountered and it detects that an outer context can understand it and thus recover from it, it invokes the re-lexing logic in the lexer
2. This logic first checks if the lexer is in a parenthesized context and returns if it's not. Thus, the logic is a no-op if the lexer isn't in a parenthesized context
3. It then reduces the nesting level by 1. It shouldn't reset it to 0 because otherwise the recovery from nested list parsing will be incorrect
4. Then, it tries to find last newline character going backwards from the current position of the lexer. This avoids any whitespaces but if it encounters any character other than newline or whitespace, it aborts.
5. Now, if there's a newline character, then it needs to be re-lexed in a logical context which means that the lexer needs to emit it as a `Newline` token instead of `NonLogicalNewline`.
6. If the re-lexing gives a different token than the current one, the token source needs to update it's token collection to remove all the tokens which comes after the new current position.

It turns out that the list parsing isn't that happy with the results so it requires some re-arranging such that the following two errors are raised correctly:
1. Expected comma
2. Recovery context error

For (1), the following scenarios needs to be considered:
* Missing comma between two elements
* Half parsed element because the grammar doesn't allow it (for example, named expressions)

For (2), the following scenarios needs to be considered:
1. If the parser is at a comma which means that there's a missing element otherwise the comma would've been consumed by the first `eat` call above. And, the parser doesn't take the re-lexing route on a comma token.
2. If it's the first element and the current token is not a comma which means that it's an invalid element.

resolves: #11640 

## Test Plan

- [x] Update existing test snapshots and validate them
- [x] Add additional test cases specific to the re-lexing logic and validate the snapshots
- [x] Run the fuzzer on 3000+ valid inputs
- [x] Run the fuzzer on invalid inputs
- [x] Run the parser on various open source projects
- [x] Make sure the ecosystem changes are none


---

_Label `parser` added by @dhruvmanila on 2024-06-12 06:34_

---

_@MichaReiser reviewed on 2024-06-12 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@from_import_missing_rpar.py.snap`:145 on 2024-06-12 06:45_

Woah, this is so much better. Nice!

---

_Comment by @github-actions[bot] on 2024-06-12 09:00_

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

_Marked ready for review by @dhruvmanila on 2024-06-12 17:42_

---

_Comment by @MichaReiser on 2024-06-13 06:25_

> It then reduces the nesting level by 1. It shouldn't reset it to 0 because otherwise the recovery from nested list parsing will be incorrect

What happens if we have something like 

```python(a, [b, 
c
)
```

In this case, the parser should recover from the unclosed `[`, but the parser and lexer should remain in a parenthesied context because of the `(`. Or does that work out of the box already, because `)` is a list terminator, and the parser won't recover past it?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1371 on 2024-06-13 06:27_

Should the below logic run when the nesting level remains greater than one? I woudl expect that it shouldn't change the lexed token because newlines must still be lexed as `NonLogicalNewline`.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1364 on 2024-06-13 06:29_

What's your reasoning for returning a `bool` over the re-lexed kind of the token?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1317 on 2024-06-13 06:29_

I would expand the comment with some explanation for which tokens this is relevant. Like which tokens may change in this situation or what kind of different tokens could be emitted. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:545 on 2024-06-13 06:32_

Nit: We can always unset it. Checking that it's true isn't realy necessary. All that matters here is that it's always `false` after. 

```suggestion
                
                first_element = false;

```

what I would find more interesting in the comment is the reason why we should only unset it when we've completely parsed the first element. 

---

_Comment by @dhruvmanila on 2024-06-13 06:38_

> In this case, the parser should recover from the unclosed `[`, but the parser and lexer should remain in a parenthesied context because of the `(`. Or does that work out of the box already, because `)` is a list terminator, and the parser won't recover past it?

Is the code snippet up-to date as you've mentioned `[` and `(` which isn't present in the snippet? :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:75 on 2024-06-13 06:39_

Is it possible that more than the last token changes? I would have expected that it is just the last token for which the range and kind changes.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@class_def_unclosed_type_param_list.py.snap`:111 on 2024-06-13 06:40_

this is pretty cool! Nice to see how all the work accumulated to now having a single, accurate error message. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__arguments__unclosed_1.py.snap`:86 on 2024-06-13 06:42_

Yes, no more EOF parse errors!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@function_def_unclosed_parameter_list.py.snap`:216 on 2024-06-13 06:43_

Awesome!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lex_logical_token.py.snap`:560 on 2024-06-13 06:44_

Should we map `NonLogicalNewline` to newline? It seems an odd error message to show to users. 

---

_@MichaReiser reviewed on 2024-06-13 06:46_

I love this. It's so exciting to see how the hard work is paying off and we now get much better error messages. 

I've left some open questions before approving but this is definetely looking good!

Do you know how our messages compare with CPython or Pyright? Do you see any significant difference?

---

_Comment by @MichaReiser on 2024-06-13 06:48_

> Is the code snippet up-to date as you've mentioned [ and ( which isn't present in the snippet? :)

Uhm, not sure what happened here

```python
(a, [b, 
	c
)
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1371 on 2024-06-13 08:06_

That's a good point. It shouldn't really be required but in case of nested list parsing like for example:

```py
if call(foo, [a, b
    def bar():
        pass
```

If we don't move the lexer back to the newline character after `b`, the "expected `]`" error will be at the `def` token:

```
Syntax Error: Expected ']', found 'def' at byte range 23..26
  |
1 | if call(foo, [a, b
2 |     def bar():
  |     ^^^ Syntax Error: Expected ']', found 'def'
3 |         pass
  |
```

---

_@dhruvmanila reviewed on 2024-06-13 08:06_

---

_@dhruvmanila reviewed on 2024-06-13 08:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1364 on 2024-06-13 08:07_

I think initially I kept it as `Option<TokenKind>` but the current usage doesn't really require the token itself. I don't think there's any other particular reason to use `bool` apart from that.

---

_@dhruvmanila reviewed on 2024-06-13 08:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:75 on 2024-06-13 08:08_

Yes, it is possible because this vector contains trivia tokens as well. So, for something like:
```py
if call(foo


    def bar():
        pass
```

There are three `NonLogicalNewline` tokens which needs to be removed and replace with a single `Newline` token.

---

_@dhruvmanila reviewed on 2024-06-13 08:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@re_lex_logical_token.py.snap`:560 on 2024-06-13 08:10_

Yeah, it is odd. I could map it to "newline" in the `Display` implementation.

---

_Comment by @dhruvmanila on 2024-06-13 08:36_

> Uhm, not sure what happened here
> 
> ```python
> (a, [b, 
> 	c
> )
> ```

Ok, so this is a bit problematic because when the lexer emitted the `)` token, it reduced the nesting level by 1 and now the re-lexing also reduces the nesting level by 1 making it 0 which means that there'll be `Newline` token after `c` which is incorrect. We could check if the current token would reduce the nesting level in re-lexing logic and avoid reducing it again. I'll need to do some more testing for these kind of scenarios.

---

_@MichaReiser reviewed on 2024-06-13 08:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1371 on 2024-06-13 08:36_

Should we set `self.nesting = 0` in that case?

---

_@MichaReiser reviewed on 2024-06-13 08:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1364 on 2024-06-13 08:37_

What I meant was to return the re-lexed token kind or the current token kind if no re-lexing happens, so that it's just a `TokenKind` (and in line with `lex_token`). But if all we need is a bool, than that's fine.

---

_@MichaReiser reviewed on 2024-06-13 08:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:75 on 2024-06-13 08:38_

Hmm, I wonder if there's a case where testing for `start() >=` won't be sufficiently accurate. But I think the only zero-width tokens that exist are dedent tokens and we should never see them when calling this function

---

_@dhruvmanila reviewed on 2024-06-13 08:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1371 on 2024-06-13 08:46_

I'm not sure how that would help. Can you expand a bit?

If we set it to 0, then the outer context (the one considering `(`) will not recover correctly as it'll not try to re-lex the token.

---

_@dhruvmanila reviewed on 2024-06-13 09:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:75 on 2024-06-13 09:41_

Yeah, that's correct. We should never see a `Dedent` token here because the lexer is in a parenthesized context when it was moved.

---

_Comment by @dhruvmanila on 2024-06-13 10:21_

> > Uhm, not sure what happened here
> > ```python
> > (a, [b, 
> > 	c
> > )
> > ```
> 
> Ok, so this is a bit problematic because when the lexer emitted the `)` token, it reduced the nesting level by 1 and now the re-lexing also reduces the nesting level by 1 making it 0 which means that there'll be `Newline` token after `c` which is incorrect. We could check if the current token would reduce the nesting level in re-lexing logic and avoid reducing it again. I'll need to do some more testing for these kind of scenarios.

Ok, I've fixed this bug and expanded the documentation and inline comments.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-13 10:21_

---

_Comment by @dhruvmanila on 2024-06-13 10:28_

> Do you know how our messages compare with CPython or Pyright? Do you see any significant difference?

The CPython parser higlights two kinds of errors which is probably done by the lexer:

1. Highlighting the opening parenthesis which doesn't have a closing parenthesis:
```
    if call(foo, [a, b]
           ^
SyntaxError: '(' was never closed
```

2. Highlighting the closing parenthesis to consider it a mismatch with the opening one:
```
    if call(foo, [a, b)
                      ^
SyntaxError: closing parenthesis ')' does not match opening parenthesis '['
```

While Pyright gets really confused:
* https://pyright-play.net/?code=JYMwBAxghgNjAUID2SA0YDaV0CMC6AUGMWACYCm4OUATvAJQBcRJrADlAM6dA
* https://pyright-play.net/?code=JYMwBAxghgNjAUID2SA0YDaV0CMCUAUGMWACYCm4OUATvHgFxEksAOUAzh0A

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1371 on 2024-06-13 12:10_

I don't know if it would help with anything but I find it confusing that the lexer might return a `Newline` token or similar while `self.nesting > 0`. Or, if that's never the case, why it is necessary to run the relexing if `self.nesting > 0`, if there's never the chance that the lexed token is going to be different.



---

_@MichaReiser reviewed on 2024-06-13 12:10_

---

_@dhruvmanila reviewed on 2024-06-13 15:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1371 on 2024-06-13 15:17_

I don't think it returns a `Newline` token if `nesting > 0`. Considering the above example, the inner list parsing (within `[`) will try to recover and re-lex it as a `NonLogicalNewline` and then the outer list parsing (within `(`) will re-lex it as a `Newline` because only now the nesting is 0.

---

_@MichaReiser approved on 2024-06-14 07:41_

---

_Comment by @codspeed-hq[bot] on 2024-06-14 12:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/re-lexing)

### Merging #11845 will **not alter performance**

<sub>Comparing <code>dhruv/re-lexing</code> (1989946) with <code>main</code> (1f654ee)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Merged by @dhruvmanila on 2024-06-17 06:47_

---

_Closed by @dhruvmanila on 2024-06-17 06:47_

---

_Branch deleted on 2024-06-17 06:47_

---
